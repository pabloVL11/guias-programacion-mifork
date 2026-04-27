<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Genericidad". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia y polimorfismo.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

### Respuesta

Un ejemplo clásico de estructura de datos genérica sin soporte de genéricos es un array dinámico que almacena direcciones de memoria en lugar del dato concreto. En C esto se logra usando void*, y en Java usando referencias de tipo Object. En ambos casos, la estructura permite guardar cualquier tipo de dato, pero se pierde información de tipo, por lo que el programador debe responsabilizarse de las conversiones correctas.

En C, un array de void* puede almacenar direcciones a datos de cualquier tipo (enteros, struct, float, etc.). La estructura solo gestiona punteros, sin conocer qué tipo de dato real se encuentra detrás. Al recuperar un elemento, es necesario hacer un cast explícito al tipo correcto. Esto permite genericidad, pero introduce riesgos si el tipo usado al leer no coincide con el tipo real almacenado.

#include <stdlib.h>

typedef struct {
    void** datos;
    int tamaño;
    int capacidad;
} ArrayGenerico;

void insertar(ArrayGenerico* a, void* elemento) {
    a->datos[a->tamaño++] = elemento;
}

/* Uso */
int x = 10;
double y = 3.14;

ArrayGenerico a;
a.datos = malloc(10 * sizeof(void*));
a.tamaño = 0;

insertar(&a, &x);
insertar(&a, &y);

En Java, el mismo efecto se consigue usando un array de Object, ya que todas las clases heredan de Object. Esto permite almacenar cualquier objeto, aunque no tipos primitivos directamente (estos deben envolverse, por ejemplo con Integer). Al extraer los elementos, también es necesario realizar una conversión explícita, lo que puede provocar errores en tiempo de ejecución si es incorrecta.

class ArrayGenerico {
    private Object[] datos;
    private int tamaño = 0;

    ArrayGenerico(int capacidad) {
        datos = new Object[capacidad];
    }

    void insertar(Object o) {
        datos[tamaño++] = o;
    }

    Object obtener(int i) {
        return datos[i];
    }
}

/* Uso */
ArrayGenerico a = new ArrayGenerico(10);
a.insertar(5);              // Integer
a.insertar("hola");         // String

int x = (Integer) a.obtener(0);

En ambos lenguajes, este enfoque permite reutilizar la misma estructura para múltiples tipos, pero a costa de seguridad de tipos y claridad. Estos problemas son precisamente los que la genericidad real (plantillas en C++ o genéricos en Java) pretende resolver.

## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

### Respuesta

La programación genérica es un paradigma cuyo objetivo es escribir código independiente del tipo de dato concreto, de forma que los mismos algoritmos y estructuras puedan reutilizarse con distintos tipos sin necesidad de duplicar el código. En lugar de programar una versión para int, otra para double y otra para String, se define un único componente genérico que se adapta al tipo que se necesite. El beneficio principal es la reutilización y la reducción de errores, manteniendo el mismo comportamiento para todos los tipos.

Este enfoque se apoya en la idea de que muchas operaciones no dependen del tipo exacto de los datos, sino de ciertas propiedades comunes (por ejemplo, poder almacenarlos, compararlos o intercambiarlos). En lenguajes como Java, esta idea se formaliza mediante genéricos, mientras que en C++ se implementa con plantillas. Ambos mecanismos preservan la información de tipo y permiten comprobar errores en tiempo de compilación.

El ejemplo anterior usando void* en C o Object en Java sí puede considerarse un ejemplo muy básico de programación genérica, ya que permite trabajar con datos de distintos tipos usando una misma estructura. Sin embargo, se trata de una forma manual e incompleta de genericidad, ya que el lenguaje no impone ningún control sobre los tipos realmente almacenados.

Por tanto, aunque conceptualmente cumple la idea de “programar para cualquier tipo”, el ejemplo carece de seguridad de tipos, obligando a realizar conversiones explícitas y desplazando los errores al tiempo de ejecución. La programación genérica moderna surge precisamente para resolver estas limitaciones, proporcionando reutilización sin perder control ni seguridad.

## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

### Respuesta

El principal problema respecto al chequeo de tipos al emplear void* en C o Object en Java es que el lenguaje pierde la información del tipo real de los datos almacenados. La estructura de datos solo sabe que contiene un puntero genérico o una referencia a Object, pero no conoce qué tipo concreto hay detrás. Esto impide que el compilador pueda verificar si las operaciones realizadas sobre los datos son correctas.

Como consecuencia, los errores de tipo no se detectan en tiempo de compilación, sino en tiempo de ejecución. En C, un cast incorrecto desde void* puede provocar accesos a memoria inválidos, comportamientos indefinidos o fallos difíciles de detectar. En Java, una conversión incorrecta desde Object genera una excepción ClassCastException, que solo aparece cuando el programa ya se está ejecutando.

Además, este enfoque obliga al programador a recordar manualmente el tipo real de cada elemento, lo que reduce la legibilidad y aumenta la probabilidad de errores, especialmente en estructuras grandes o compartidas entre distintos módulos. No existe ninguna restricción que obligue a insertar siempre el mismo tipo de objeto en la estructura, lo que puede dar lugar a colecciones heterogéneas involuntarias.

Por último, la falta de chequeo de tipos hace que estas estructuras sean menos seguras y menos expresivas que las soluciones basadas en genéricos reales. Este problema es precisamente uno de los motivos por los que se introducen mecanismos de programación genérica en lenguajes modernos: permitir reutilización de código manteniendo verificación de tipos en compilación.


## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

### Respuesta

Los parámetros de tipo son un mecanismo de la programación genérica que permite definir clases, métodos o interfaces sin especificar de antemano el tipo concreto de los datos con los que van a trabajar. En lugar de usar un tipo fijo (como int o Object), se emplea un parámetro que representa un tipo aún desconocido, y que será concretado en el momento de usar la clase o el método. De este modo, el mismo código puede reutilizarse con distintos tipos manteniendo un comportamiento coherente.

En Java, los parámetros de tipo se introducen entre los símbolos < > y suelen representarse mediante letras como T, E o K. Estos parámetros actúan como marcadores de tipo, permitiendo al compilador conocer qué tipo se está usando en cada caso concreto. A diferencia del uso de Object, el tipo real no se pierde, sino que se propaga a lo largo de la clase o método, lo que permite realizar comprobaciones estáticas.

Gracias a los parámetros de tipo, el chequeo de tipos se realiza en tiempo de compilación, evitando conversiones explícitas y reduciendo errores en tiempo de ejecución. Por ejemplo, una estructura genérica definida con un parámetro de tipo solo permitirá insertar y extraer elementos de ese tipo concreto, impidiendo usos incorrectos desde el principio.

En resumen, los parámetros de tipo formalizan la programación genérica dentro del propio lenguaje. Proporcionan reutilización de código, claridad semántica y seguridad de tipos, resolviendo los problemas que aparecían al emplear técnicas más primitivas como void* en C o Object en Java.


## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

### Respuesta

En Java, la programación genérica se realiza mediante generics, que permiten definir colecciones parametrizadas por un tipo concreto. Al instanciar una lista indicando String como parámetro de tipo, se garantiza que solo se podrán almacenar objetos de tipo String, y que al recuperarlos no será necesario realizar conversiones explícitas. El compilador impide insertar otros tipos, reforzando así la seguridad y claridad del código.

Al recorrer la colección, cada elemento se trata directamente como un String, lo que permite usar sus métodos sin riesgo de errores de tipo. Este comportamiento supone una mejora clara respecto al uso de Object, ya que cualquier uso incorrecto se detecta en tiempo de compilación y no en tiempo de ejecución.

import java.util.ArrayList;
import java.util.List;

List<String> lista = new ArrayList<>();
lista.add("uno");
lista.add("dos");
lista.add("tres");

for (String s : lista) {
    System.out.println(s.toUpperCase()); // s es String con total seguridad
}

En C++, la programación genérica se implementa mediante templates, que permiten definir clases y funciones parametrizadas por tipos. La clase std::vector es un ejemplo clásico de contenedor genérico. Al instanciar un vector<std::string>, se indica explícitamente que el vector solo contendrá cadenas, y el compilador genera una versión específica del código para ese tipo. Esto proporciona seguridad de tipos sin costes en tiempo de ejecución.

Durante el recorrido del contenedor, cada elemento es un std::string real, no un puntero genérico ni un objeto base. El acceso es directo y seguro, y cualquier intento de insertar un tipo distinto producirá un error de compilación. De este modo, las plantillas combinan reutilización de código y verificación estricta de tipos.

#include <vector>
#include <string>
#include <iostream>

std::vector<std::string> v;
v.push_back("uno");
v.push_back("dos");
v.push_back("tres");

for (const std::string& s : v) {
    std::cout << s.length() << std::endl; // s es std::string
}

En ambos lenguajes, el uso de generics o templates permite crear estructuras de datos reutilizables y seguras, donde el tipo concreto se conoce y se respeta en todo momento, resolviendo los problemas de chequeo de tipos presentes en enfoques más primitivos.


## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

### Respuesta

Cuando se instancia una clase con parámetros de tipo, el compilador debe adaptar el código genérico al tipo concreto indicado, pero la forma de hacerlo es distinta en Java y en C++. En ambos casos, el objetivo es permitir reutilización de código manteniendo la consistencia de tipos, pero el mecanismo interno y sus consecuencias son diferentes.

En Java, el compilador aplica una técnica denominada type erasure (borrado de tipos). Esto significa que la información del parámetro de tipo (<String>, <Integer>, etc.) solo existe en tiempo de compilación. Durante la compilación, el código genérico se verifica para asegurar que los usos de tipos son correctos, pero una vez generado el bytecode, los parámetros de tipo se eliminan y se sustituyen normalmente por Object (o por el límite superior si existe). Por ello, en tiempo de ejecución no se distingue entre, por ejemplo, List<String> y List<Integer>.

Como consecuencia del type erasure, Java no genera versiones distintas del código para cada tipo, sino que usa una única implementación compartida, insertando conversiones automáticas donde sea necesario. Esto explica por qué no es posible, por ejemplo, crear arrays de tipos genéricos o comprobar el tipo genérico en tiempo de ejecución con instanceof. La ventaja es la compatibilidad con versiones antiguas del lenguaje, pero a costa de perder información de tipo en ejecución.

En C++, el enfoque es completamente diferente y se basa en la instanciación de plantillas. Cuando se usa una plantilla con un tipo concreto, el compilador genera una versión específica del código para ese tipo. Por ejemplo, vector<string> y vector<int> producen dos implementaciones distintas. En este caso, la información de tipo se mantiene plenamente y se realiza todo el chequeo y la generación de código en tiempo de compilación, sin borrado de tipos. Esto proporciona gran eficiencia y seguridad de tipos, aunque puede aumentar el tamaño del código generado.


## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

### Respuesta

En Java, una clase con parámetros de tipo permite definir estructuras de datos que trabajan con varios tipos concretos sin perder seguridad de tipos. En este caso, se puede definir una clase Par con dos parámetros de tipo distintos, que representen los tipos de los dos valores almacenados. La clase no necesita conocer de antemano cuáles serán esos tipos, ya que se especifican en el momento de su uso.

La clase Par contiene dos atributos, uno por cada parámetro de tipo, un constructor para inicializarlos y un getter para acceder a cada uno. Gracias a los generics, el compilador garantiza que los valores recuperados tienen exactamente el tipo esperado, evitando conversiones explícitas y errores en tiempo de ejecución. Esta clase es comparable conceptualmente a una struct de C, pero con control de tipos mucho más estricto.

public class Par<T1, T2> {
    private T1 primero;
    private T2 segundo;

    public Par(T1 primero, T2 segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public T1 getPrimero() {
        return primero;
    }

    public T2 getSegundo() {
        return segundo;
    }
}

Un uso típico de esta clase es como tipo de retorno múltiple de un método, evitando tener que crear una clase específica para cada caso. Por ejemplo, se puede devolver en un Par<Double, Double> la media y la desviación típica de un array de double. El código que invoca al método sabe con total seguridad qué representa cada valor y de qué tipo es.

public static Par<Double, Double> mediaYDesviacion(double[] datos) {
    double suma = 0.0;
    for (double d : datos) {
        suma += d;
    }
    double media = suma / datos.length;

    double sumaCuadrados = 0.0;
    for (double d : datos) {
        sumaCuadrados += (d - media) * (d - media);
    }
    double desviacion = Math.sqrt(sumaCuadrados / datos.length);

    return new Par<>(media, desviacion);
}

/* Uso */
double[] valores = {1.0, 2.0, 3.0, 4.0};
Par<Double, Double> resultado = mediaYDesviacion(valores);

double media = resultado.getPrimero();
double desviacion = resultado.getSegundo();

Este ejemplo muestra cómo los parámetros de tipo permiten expresar con claridad la intención del código, reutilizar estructuras genéricas y mantener el chequeo de tipos en compilación, algo que no sería posible usando únicamente Object.

## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

### Respuesta

En Java, además de definir parámetros de tipo a nivel de clase, también es posible declararlos a nivel de método. Un método genérico introduce sus propios parámetros de tipo antes del tipo de retorno, y estos parámetros solo existen dentro del método. Esto es útil cuando la genericidad no es una propiedad permanente de la clase, sino solo de una operación concreta.

Si el método seleccionaUno se define usando Object, se pueden pasar dos objetos cualesquiera y devolver uno de ellos, pero el tipo concreto se pierde. El compilador no puede garantizar que ambos objetos sean del mismo tipo y, al recibir el resultado, es necesario realizar downcasting. Esto introduce riesgos de error en tiempo de ejecución y hace el código menos expresivo y seguro.

import java.util.Random;

public static Object seleccionaUno(Object a, Object b) {
    return new Random().nextBoolean() ? a : b;
}

/* Uso */
Object o = seleccionaUno("hola", "adios");
String s = (String) o; // downcasting obligatorio

En cambio, al definir seleccionaUno como método genérico, se introduce un parámetro de tipo (por ejemplo T) que representa el tipo común de ambos parámetros y del valor de retorno. Esto permite al compilador garantizar que ambos objetos son del mismo tipo y que el resultado será exactamente ese tipo, eliminando la necesidad de conversiones explícitas.

import java.util.Random;

public static <T> T seleccionaUno(T a, T b) {
    return new Random().nextBoolean() ? a : b;
}

/* Uso */
String s = seleccionaUno("hola", "adios"); // sin casting

// seleccionaUno("hola", 5); // Error de compilación

La diferencia clave es que el método genérico (i) evita el downcasting, ya que el tipo de retorno es conocido en compilación, y (ii) fuerza que ambos objetos sean del mismo tipo, impidiendo combinaciones incorrectas mediante un error de compilación. Esto convierte al método genérico en una solución más segura, clara y alineada con los principios de la programación genérica.


## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

### Respuesta

Sí, en Java se pueden establecer restricciones en los parámetros de tipo, lo que se conoce como parámetros de tipo acotados (bounded type parameters). Esto permite indicar que un tipo genérico debe ser, como mínimo, una subclase de otra clase o implementar una interfaz concreta. De este modo, el compilador garantiza que ciertos métodos o propiedades estarán disponibles, por ejemplo los métodos numéricos definidos en Number.

Una primera solución sencilla consiste en no usar genéricos y declarar directamente las coordenadas como Number. Esto permite almacenar cualquier tipo numérico (Integer, Double, Float, etc.) y operar con ellos usando métodos comunes como doubleValue(). El inconveniente es que se pierde información del tipo concreto, ya que no se sabe si el punto trabaja con enteros, dobles u otro número, y el chequeo de tipos es más débil.

public class Punto {
    private Number x;
    private Number y;

    public Punto(Number x, Number y) {
        this.x = x;
        this.y = y;
    }

    public Number getX() {
        return x;
    }

    public Number getY() {
        return y;
    }

    public double calcularDistanciaA(Punto otro) {
        double dx = x.doubleValue() - otro.x.doubleValue();
        double dy = y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}

Una segunda solución más robusta es emplear generics con una restricción, usando <T extends Number>. De este modo, el tipo de las coordenadas se parametriza, pero el compilador impone que sea algún subtipo de Number. Esto refuerza el chequeo de tipos y permite saber exactamente con qué tipo de número trabaja un Punto, evitando mezclas accidentales de tipos distintos.

public class Punto<T extends Number> {
    private T x;
    private T y;

    public Punto(T x, T y) {
        this.x = x;
        this.y = y;
    }

    public T getX() {
        return x;
    }

    public T getY() {
        return y;
    }

    public double calcularDistanciaA(Punto<T> otro) {
        double dx = x.doubleValue() - otro.x.doubleValue();
        double dy = y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}

/* Uso */
Punto<Double> p1 = new Punto<>(1.0, 2.0);
Punto<Double> p2 = new Punto<>(4.0, 6.0);
// Punto<Integer> p3 = new Punto<>(1, 2);
// p1.calcularDistanciaA(p3); // Error de compilación

Respecto al type erasure, tras la compilación el tipo genérico T se elimina y se sustituye por su límite superior. En este caso, tanto Punto<T extends Number> como sus atributos pasan a tratarse internamente como Number. Es decir, el tipo final en el bytecode es Number, pero el compilador ya ha garantizado en tiempo de compilación toda la seguridad de tipos que el programador necesita.
Proporcione sus comentarios sobre BizChat


## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

### Respuesta

Ambas soluciones permiten reutilizar la clase Punto para distintos tipos numéricos, pero no ofrecen el mismo nivel de chequeo de tipos. En la solución sin genéricos, al definir las coordenadas directamente como Number, sí es posible crear un punto con una coordenada entera y la otra real (por ejemplo, new Punto(3, 2.5)). Esto ocurre porque Number actúa como tipo común para todos los números, sin imponer ninguna relación adicional entre ellos. El compilador no considera que exista ningún problema en mezclar tipos numéricos distintos dentro del mismo objeto.

En cambio, en la solución con genéricos (Punto<T extends Number>), el parámetro de tipo T obliga a que ambas coordenadas sean exactamente del mismo tipo concreto. Así, se puede crear un Punto<Integer> o un Punto<Double>, pero no mezclar ambos en una misma instancia. Intentar construir un punto con coordenadas de distinto tipo provocaría un error de compilación. Este es un claro ejemplo de cómo los genéricos refuerzan el chequeo de tipos y evitan errores conceptuales desde el principio.

También existe una diferencia importante en el tipo devuelto por los métodos de acceso. En la solución sin genéricos, el método getX devuelve un Number, lo que obliga a tratar el valor de forma genérica o a realizar conversiones posteriores si se necesita un tipo concreto. La información sobre si la coordenada es un Integer, Double u otro tipo se pierde en la interfaz pública de la clase.

Por el contrario, en la solución con genéricos, getX devuelve el tipo T, es decir, el tipo numérico concreto con el que se ha instanciado el punto. Esto permite escribir código más preciso, legible y seguro, ya que el compilador conoce exactamente el tipo devuelto y puede verificar su uso correctamente. Este refuerzo del sistema de tipos es una de las principales ventajas prácticas del uso de generics frente a soluciones basadas únicamente en clases base como Number.


## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting.
```java
public interface Punto { 
    public double distanciaA(Punto p); 
} 

public class Punto2D implements Punto { 
     private final double x, y; 
     public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    @Override 
    public double distanciaA(Punto p) { 
        if (p instanceof Punto2D) { 
            Punto2D p2d = (Punto2D) p; 
            return Math.sqrt(Math.pow(x - p2d.x, 2) 
                    + Math.pow(y - p2d.y, 2)); 
        } else { 
            throw new RuntimeException("p debe ser Punto 2D"); 
        } 
    } 
} 
public class Punto3D implements Punto { 
    // Igual que Punto2D, pero con tres coordenadas
    ...
} 
```

### Respuesta

Para reforzar el chequeo de tipos y evitar tanto el uso de instanceof como el downcasting, se puede aplicar un patrón conocido como genericidad recursiva o F-bounded polymorphism. La idea consiste en parametrizar la interfaz Punto con su propio subtipo, de forma que el método distanciaA solo acepte puntos del mismo tipo concreto. Así, el compilador garantiza que nunca se intentará calcular la distancia entre puntos incompatibles (por ejemplo, un punto 2D y uno 3D).

La interfaz Punto pasa a tener un parámetro de tipo que representa “el propio tipo del punto”. El método distanciaA recibe entonces un parámetro de ese mismo tipo, eliminando cualquier ambigüedad. Cada implementación concreta especifica el parámetro de tipo como ella misma, forzando una correspondencia exacta entre emisor y receptor del método.

public interface Punto<T extends Punto<T>> {
    double distanciaA(T p);
}
``

Con esta definición, la implementación de Punto2D puede asumir con total seguridad que el parámetro recibido es también un Punto2D. No es necesario comprobar el tipo en tiempo de ejecución ni realizar conversiones, ya que el compilador impide usos incorrectos desde el principio. El código queda además más limpio y expresivo.

public class Punto2D implements Punto<Punto2D> {public class Punto2D implements Punto<Punto2 p) {
        return Math.sqrt(Math.pow(x - p.x, 2)
                       + Math.pow(y - p.y, 2));
    }
}
    private final double x, y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override


De forma análoga, Punto3D implementaría Punto<Punto3D> y su método distanciaA solo aceptaría puntos tridimensionales. Cualquier intento de calcular la distancia entre un Punto2D y un Punto3D provocaría un error de compilación, no una excepción en ejecución. Este ejemplo muestra cómo los generics permiten expresar invariantes del dominio directamente en el sistema de tipos, logrando un diseño más seguro y robusto.


## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

### Respuesta

El hecho de que String sea subtipo de Object no implica que List<String> sea subtipo de List<Object>. En Java, los tipos genéricos son invariantes respecto a su parámetro de tipo. Esto significa que, aunque String <: Object, List<String> y List<Object> no guardan relación de herencia entre sí. Permitir esa sustitución sería peligroso: si una List<String> pudiera tratarse como List<Object>, se podrían insertar objetos que no fueran String, rompiendo la coherencia interna de la lista.

En cambio, con los arrays la situación es distinta: String[] sí es subtipo de Object[]. Los arrays en Java son covariantes por diseño histórico. Esto permite escribir código más flexible, pero introduce un problema potencial en tiempo de ejecución. Por ejemplo, es legal asignar un String[] a una variable Object[], pero si luego se intenta almacenar un Integer en ese array, el error no se detecta en compilación, sino que se produce una ArrayStoreException en ejecución. Es decir, el sistema de tipos se relaja en compilación y compensa con una comprobación dinámica.

Esta diferencia se explica porque los genéricos priorizan la seguridad de tipos en compilación, mientras que los arrays priorizan compatibilidad hacia atrás. Con los genéricos, el compilador impide directamente operaciones peligrosas; con los arrays, el lenguaje permite más cosas, pero introduce fallos en ejecución para proteger la integridad del array real.

A partir de estos ejemplos, se dice que un tipo genérico es covariante si se cumple que, cuando A es subtipo de B, entonces G<A> es subtipo de G<B> (como ocurre con los arrays). Es contravariante si la relación va en sentido inverso (G<B> subtipo de G<A>), y es invariante si no existe ninguna relación de herencia entre G<A> y G<B>, aunque A y B sí la tengan, que es el caso de los genéricos en Java (List<T>). Esta invariancia es una decisión deliberada para garantizar seguridad de tipos estática.


## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

### Respuesta

Un wildcard (?) en Java es un marcador de tipo desconocido que se utiliza en tipos genéricos para relajar la invariancia de los genéricos de forma controlada. Permite expresar que una colección trabaja con “algún tipo”, sin especificar exactamente cuál, pero imponiendo ciertas restricciones. De este modo, se recupera covarianza o contravarianza sin perder seguridad de tipos en compilación.

La expresión List<? extends T> indica que la lista contiene elementos de algún subtipo de T. Es una forma de covarianza: permite aceptar listas de T o de cualquier clase que herede de T. Sin embargo, esta flexibilidad tiene una consecuencia importante: no se pueden añadir elementos a la lista (salvo null), ya que el compilador no sabe cuál es el subtipo concreto. Este tipo de wildcard se usa cuando la lista es solo productora de valores, es decir, cuando se leen elementos.

public static double sumar(List<? extends Number> numeros) {
    double suma = 0.0;
    for (Number n : numeros) {
        suma += n.doubleValue();
    }
    return suma;
}

Por el contrario, List<? super T> indica que la lista contiene elementos de algún supertipo de T. Esto corresponde a la contravarianza. En este caso, sí es seguro añadir elementos de tipo T (o subtipos de T), ya que cualquier supertipo de T puede almacenarlos. La contrapartida es que, al leer elementos, solo se garantiza que son de tipo Object. Este wildcard se utiliza cuando la lista es consumidora de valores, es decir, cuando se insertan elementos.

public static void añadirEnteros(List<? super Integer> lista) {
    lista.add(1);
    lista.add(2);
    lista.add(3);
}

En resumen, ? extends T se usa cuando se necesita leer elementos de una estructura genérica con distintos subtipos de T, y ? super T cuando se necesita escribir elementos de tipo T. Esta idea suele resumirse en la regla PECS (Producer Extends, Consumer Super), que ayuda a decidir qué tipo de wildcard utilizar en cada situación.