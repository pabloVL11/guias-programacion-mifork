<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Clases y Objetos". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: ninguno.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

### Respuesta
A la programación orientada a objetos se le atribuyen cuatro características fundamentales: abstracción, encapsulación, herencia y polimorfismo.

La abstracción consiste en centrarse en los aspectos esenciales de un objeto, ocultando los detalles irrelevantes. En la práctica, esto implica diseñar clases que representen conceptos sencillos y comprensibles sin necesidad de conocer al detalle su código.

La encapsulación se refiere a agrupar datos y funciones dentro de una misma entidad, la clase, y controlar qué partes pueden ser accedidas desde fuera. En Java esto se logra mediante modificadores como private, public o protected. Encapsular permite evitar modificaciones accidentales de datos internos y obliga a interactuar con los objetos a través de métodos bien definidos, creando interfaces más seguras y coherentes.

La herencia permite crear nuevas clases basadas en otras ya existentes, reutilizando código y evitando duplicación. Una clase hija hereda atributos y métodos de la clase padre, pudiendo extender o modificar su comportamiento de forma jerárquica.

Finalmente, el polimorfismo permite que un mismo método pueda comportarse de forma distinta según el tipo de objeto que lo utilice. En Java esto se consigue principalmente mediante la sobreescritura de métodos en clases hijas. Gracias al polimorfismo, es posible escribir código más general que trabaje con referencias a clases base, mientras que la implementación concreta se decide en tiempo de ejecución, produciendo programas más flexibles y adaptables.

## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Respuesta
C++, C#, Java, Python

## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### Respuesta
La programación estructurada es un paradigma que organiza el código en bloques lógicos utilizando únicamente tres estructuras básicas de control: secuencia, selección e iteración. Este enfoque surgió para combatir el uso excesivo de saltos incontrolados que dificultaban entender y mantener los programas.

Por su parte, la programación modular representa un paso adicional dentro del paradigma estructurado. En lugar de escribir todo el programa como un único bloque, se divide la solución en módulos independientes que se pueden comprender y revisar más fácilmente.

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### Respuesta
En programación orientada a objetos, un objeto se define principalmente mediante estado, comportamiento e identidad. Estos tres elementos permiten representar entidades del mundo real o del propio programa de forma coherente, combinando datos y operaciones dentro de una misma unidad.

El estado se refiere a los datos que almacena un objeto, es decir, los valores de sus atributos en un momento dado. Este estado puede cambiar a lo largo de la ejecución, igual que las variables dentro de un struct cambian según las operaciones del programa. En una clase Java, estos datos suelen declararse como variables de instancia, y representan las características que describen al objeto.

El comportamiento está definido por los métodos del objeto, que especifican las acciones que puede realizar o las operaciones que pueden modifican su estado. A diferencia de C, donde las funciones están separadas de las estructuras, en Java los métodos forman parte directa de la clase, permitiendo que cada objeto "sepa" cómo actuar sobre sí mismo. Este acoplamiento entre datos y métodos es uno de los pilares que distingue la POO de la programación estructurada.

Finalmente, la identidad permite diferenciar un objeto de otro, incluso si ambos comparten el mismo estado y el mismo comportamiento. Dos objetos pueden tener idénticos valores en sus atributos, pero seguir siendo entidades independientes en memoria. En Java, esta identidad se gestiona internamente mediante referencias, que funcionan como direcciones o punteros seguros hacia cada instancia. Gracias a este concepto, la POO permite manejar múltiples objetos similares sin confundirlos entre sí.

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Respuesta
Una clase es una plantilla o modelo que describe cómo serán los objetos que se creen a partir de ella. En su interior se definen los atributos (datos) y los métodos (comportamientos) que caracterizan a ese tipo de entidad.

La clase es el diseño, el objeto es el resultado concreto creado a partir de ese diseño. Cada objeto tiene su propio estado, puede ejecutar los métodos definidos en la clase y ocupa un lugar específico en memoria.

Una instancia es simplemente otro nombre para un objeto creado a partir de una clase. Instanciar una clase significa construir un objeto real usando su definición. Por ejemplo, crear new Coche() en Java implica generar una instancia de la clase Coche. Cada una de estas instancias puede tener valores distintos en sus atributos; así, dos coches pueden ser de diferentes colores aunque provengan de la misma clase.

No todos los lenguajes orientados a objetos manejan clases. Existen lenguajes basados en prototipos, como JavaScript, donde la orientación a objetos se logra sin clases formales en su diseño original, aunque versiones modernas añadan una sintaxis de clase como azúcar sintáctico. En estos lenguajes, los objetos se crean a partir de otros objetos, y la herencia se basa en cadenas de prototipos. Por tanto, aunque la mayoría de lenguajes OOP sí utilizan clases, no es un requisito universal del paradigma.

## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### Respuesta
En la mayoría de lenguajes orientados a objetos, los objetos se almacenan en regiones de memoria dinámicas, normalmente conocidas como heap. Esta zona permite crear estructuras cuya vida útil no depende del alcance de una función concreta, a diferencia de las variables locales almacenadas en la pila (stack). En Java, por ejemplo, todos los objetos se alojan en el heap y se accede a ellos mediante referencias, lo que evita la gestión manual de direcciones típica de C o C++.

No todos los lenguajes manejan la memoria de la misma manera. En C++, aunque también se utiliza el heap para crear objetos dinámicos con new, es posible crear objetos en el stack, algo que no existe en Java. Además, existen lenguajes que combinan ambos enfoques o que utilizan técnicas específicas de optimización, como la stack allocation optimizada en algunos casos de Python o las estrategias híbridas en lenguajes funcionales orientados a objetos. Por tanto, la ubicación exacta de los objetos depende del diseño del lenguaje y de su modelo de ejecución.

La recolección de basura (garbage collection) es un proceso automático mediante el cual el lenguaje libera la memoria ocupada por objetos que ya no tienen referencias activas. Este mecanismo evita fugas de memoria, ya que el programador no necesita liberar manualmente los recursos, a diferencia de lo que ocurre en C y C++. En Java, el recolector analiza periódicamente qué objetos ya no son accesibles por el programa y elimina su espacio en memoria, permitiendo una gestión más segura y menos propensa a errores.

Sin embargo, la recolección de basura no es universal. Lenguajes como C++ dejan la gestión de memoria en manos del programador, lo que proporciona mayor control pero también más responsabilidad. En cambio, lenguajes como Java, C#, Python o Kotlin dependen de la recolección automática, priorizando la seguridad y la facilidad de uso. Esta diferencia fundamental influye directamente en el estilo de programación y en la forma de diseñar aplicaciones en cada lenguaje.

## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

### Respuesta
Un método es una operación asociada a una clase que define el comportamiento de sus objetos. Conceptualmente, es similar a una función en C, pero se define directamente dentro de la clase y suele operar sobre el estado del objeto (sus atributos). En Java, los métodos de instancia actúan sobre cada objeto concreto, mientras que los métodos estáticos (static) pertenecen a la clase en general y se invocan sin necesidad de crear una instancia. Esta integración de datos (atributos) y operaciones (métodos) es lo que distingue a la POO de la programación estructurada.

La sobrecarga de métodos (method overloading) consiste en declarar varios métodos con el mismo nombre dentro de una misma clase, pero con listas de parámetros diferentes (distinto número, tipos o ambos). En Java, la sobrecarga se decide en tiempo de compilación (resolución estática): el compilador elige qué versión invocar según los tipos y la cantidad de argumentos. Es importante notar que no se puede sobrecargar solo cambiando el tipo de retorno; la firma relevante para la sobrecarga considera el nombre y la lista de parámetros. Esto difiere de la sobrescritura (overriding), que cambia la implementación de un método heredado y se resuelve en tiempo de ejecución (polimorfismo).

Un ejemplo sencillo en Java ayuda a fijar ideas, comparándolo con que en C no existe sobrecarga nativa (en C++ sí existe para funciones y operadores). En Java:

class Calculadora {
    // Método de instancia
    public int sumar(int a, int b) { return a + b; }

    // Sobrecarga: mismo nombre, distinta lista de parámetros
    public double sumar(double a, double b) { return a + b; }
    public int sumar(int a, int b, int c) { return a + b + c; }

    // Método estático (no requiere instancia)
    public static int abs(int x) { return x < 0 ? -x : x; }
}

En este ejemplo, sumar está sobrecargado para manejar distintos tipos y cantidades de argumentos, mientras que abs ilustra un método estático. Desde el punto de vista de alguien que viene de C, puede verse como tener “varias funciones con el mismo nombre” seleccionadas por la firma, algo que en C habría que emular con nombres distintos.

## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### Respuesta
// Archivo: Punto.java
class Punto {
    // Atributos con visibilidad por defecto (package-private)
    double x;
    double y;

    // Constructor por conveniencia (opcional)
    Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // Método de instancia que calcula la distancia al origen (0,0)
    double calculaDistanciaAOrigen() {
        // Distancia euclidiana: sqrt(x^2 + y^2)
        return Math.sqrt(x * x + y * y);
    }
}

Para ilustrar su uso, se crea una instancia de Punto y se invoca el método. En Java, el punto de entrada es el método main. Se imprime el resultado por consola. Nótese que no se usan modificadores como public en los atributos para mantener la visibilidad por defecto, como se solicitó.

// Archivo: EjemploUso.java
public class EjemploUso {
    public static void main(String[] args) {
        Punto p = new Punto(3.0, 4.0);   // Instancia con coordenadas (3, 4)
        double d = p.calculaDistanciaAOrigen();
        System.out.println("Distancia al origen: " + d); // Imprime 5.0
    }
}

Este ejemplo compila y ejecuta con:

javac Punto.java EjemploUso.java
java EjemploUso

Al ejecutarse, se obtiene 5.0 porque la distancia euclidiana de (3,4) a (0,0) es sqrt(3^2 + 4^2) = 5. Si se prefiere, el método podría declararse public, pero no es necesario para el objetivo del ejemplo. 

## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### Respuesta
El punto de entrada en un programa Java es siempre el método
public static void main(String[] args). Este método actúa como la primera función que se ejecuta, de forma similar a int main() en C. La máquina virtual de Java (JVM) busca este método en la clase indicada para iniciar la ejecución del programa. Si no existe, el programa no puede arrancar. Aunque un proyecto pueda contener muchas clases, sólo aquella que tenga un método main válido puede servir como punto de inicio.

La palabra clave static indica que un miembro pertenece a la clase, y no a instancias concretas de ella. Esto significa que el método o atributo marcado como static puede usarse sin crear objetos. En el caso del método main, se define como estático para que la JVM pueda llamarlo sin necesidad de instanciar antes la clase donde se encuentra. De lo contrario, sería necesario crear un objeto antes de ejecutar el propio punto de entrada, lo cual generaría una dependencia circular.

El uso de static no se limita al método main. Se emplea para crear métodos de utilidad, atributos compartidos por todas las instancias y constantes. Un ejemplo habitual es Math.sqrt(), que es un método estático porque no tiene sentido crear un objeto Math para usarlo. Asimismo, las variables estáticas permiten almacenar información común, como contadores de instancias o configuraciones globales, aunque su abuso puede provocar diseños menos limpios.

Cuando se combina static con final, el resultado suele ser una constante. final impide que una variable cambie de valor después de inicializarse, de forma parecida a const en C. Al declararse como static final, la constante se vuelve única para toda la clase y disponible sin instanciarla. En Java, estas constantes suelen nombrarse en mayúsculas, por ejemplo:

static final double PI = 3.1415926535;

Este patrón se utiliza para valores fijos que deben ser accesibles globalmente, mejorando claridad y evitando errores al permitir que el compilador optimice su acceso.

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Respuesta

Para compilar y ejecutar un programa Java desde la línea de comandos se utilizan dos herramientas básicas: javac y java. El proceso habitual consiste primero en compilar el archivo .java para generar uno o más archivos .class que contienen el bytecode. Por ejemplo, si se tiene un archivo Hola.java con una clase pública Hola, basta con escribir en la terminal:

javac Hola.java     # Compila y genera Hola.class
java Hola           # Ejecuta la clase que contiene main
``
El primer comando analiza el código, comprueba errores y emite el bytecode. El segundo comando no ejecuta directamente el archivo .class, sino que pone en marcha la máquina virtual, que interpreta ese bytecode o lo compila dinámicamente durante la ejecución.

Java se considera un lenguaje compilado e interpretado a la vez. Se compila porque el código fuente .java se transforma primero en bytecode independiente de la plataforma, y se interpreta porque ese bytecode es posteriormente ejecutado por la Máquina Virtual de Java (JVM). Esta combinación ofrece portabilidad, ya que el mismo archivo .class puede ejecutarse en cualquier sistema que disponga de una JVM compatible, independientemente del sistema operativo o del tipo de procesador.

La máquina virtual es un entorno de ejecución que actúa como capa intermedia entre el programa y el sistema operativo. Su responsabilidad principal es ejecutar el bytecode de manera segura, gestionando memoria, excepciones, hilos y otros recursos. Gracias a esta abstracción, Java garantiza que el programa se comporte igual en todas las plataformas, lo cual no ocurre en lenguajes nativos como C o C++ donde se generan binarios específicos por arquitectura.

El bytecode es un conjunto de instrucciones de nivel intermedio que generan los compiladores de Java. No es código máquina real, pero está muy cercano a él. Cada archivo .class contiene bytecode para una única clase, de modo que un programa puede generar múltiples ficheros .class al compilarse. La JVM interpreta o compila este bytecode a código nativo en tiempo de ejecución mediante técnicas como JIT compilation (Just-In-Time), permitiendo un rendimiento competitivo sin sacrificar portabilidad.


## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### Respuesta

En Java, new es el operador de creación de objetos. Su función es reservar memoria en el heap para una nueva instancia de la clase indicada y devolver una referencia a esa instancia. A diferencia de C, donde podría utilizarse malloc y luego inicializar manualmente, en Java new invoca automáticamente un constructor de la clase para dejar el objeto en un estado válido desde el primer momento. Por ejemplo, new Punto(3, 4) crea un objeto Punto y llama a su constructor con los argumentos 3 y 4.

Un constructor es un método especial cuyo nombre coincide con el de la clase y no tiene tipo de retorno (ni siquiera void). Se ejecuta justo después de reservar la memoria con new y se utiliza para inicializar los atributos del objeto. Si no se define ninguno, Java proporciona un constructor por defecto sin parámetros. Es habitual emplear this dentro del constructor para distinguir entre los parámetros y los atributos de instancia. También pueden existir constructores sobrecargados con diferentes listas de parámetros, permitiendo varias formas de crear un objeto en función de la información disponible.

A continuación se muestra un ejemplo de una clase Empleado con tres atributos (dni, nombre, apellidos) y un constructor que inicializa dichos campos. Se incluye además un ejemplo de uso en main para ilustrar la instanciación mediante new:

// Archivo: Empleado.java
public class Empleado {
    String dni;
    String nombre;
    String apellidos;

    // Constructor que inicializa todos los atributos
    public Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }

    // Método auxiliar para mostrar datos (opcional)
    public String descripcion() {
        return nombre + " " + apellidos + " (" + dni + ")";
    }

    public static void main(String[] args) {
        // Creación de una instancia con 'new' que llama al constructor
        Empleado e = new Empleado("12345678A", "Ana", "García López");
        System.out.println(e.descripcion());
    }
}

En este ejemplo, new Empleado(...) reserva memoria y ejecuta el constructor para dejar el objeto listo.

## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### Respuesta

La referencia this es un identificador especial disponible dentro de una clase que hace referencia a la instancia actual sobre la que se está ejecutando el método o el constructor. Sirve para acceder a los atributos y métodos de ese objeto concreto y, muy comúnmente, para desambiguar entre nombres de parámetros y nombres de atributos cuando coinciden. En un constructor, this ayuda a dejar claro que se está asignando al campo del objeto (por ejemplo, this.x = x;).

No se denomina ni se comporta igual en todos los lenguajes. En Java y C++ la palabra es this y se refiere implícitamente al objeto actual; en C# también es this. En Python no existe palabra reservada equivalente: se utiliza el primer parámetro de los métodos de instancia, por convención llamado self, pero no es una palabra clave del lenguaje. En JavaScript la palabra this existe, pero su valor depende del contexto de invocación (función libre, método, bind/call/apply, funciones flecha, etc.), por lo que su semántica difiere bastante de Java y puede resultar más confusa.

A continuación, un ejemplo de uso de this en la clase Punto, mostrando desambiguación en el constructor y una llamada encadenada a un método de instancia:

// Archivo: Punto.java
class Punto {
    double x; // visibilidad por defecto
    double y;

    // Constructor: desambiguación entre parámetros y atributos
    Punto(double x, double y) {
        this.x = x;   // 'this.x' = atributo; 'x' = parámetro
        this.y = y;
    }

    double calculaDistanciaAOrigen() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
    }

    // Método que traslada el punto y devuelve la propia instancia (fluido)
    Punto trasladar(double dx, double dy) {
        this.x += dx;   // uso explícito de 'this' (opcional aquí, pero claro)
        this.y += dy;
        return this;    // devuelve la instancia actual
    }
}

// Ejemplo de uso
class Demo {
    public static void main(String[] args) {
        Punto p = new Punto(3, 4);
        double d1 = p.calculaDistanciaAOrigen();       // 5.0
        double d2 = p.trasladar(-3, -4).calculaDistanciaAOrigen(); // 0.0
        System.out.println(d1 + " -> " + d2);
    }
}

En el ejemplo, this.x y this.y identifican inequívocamente los campos del objeto, evitando confusiones con los parámetros del constructor. Además, return this; ilustra que this es la referencia a la instancia actual, lo que permite encadenar operaciones (“fluent API”) sin crear nuevos objetos si no se desea.


## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Respuesta

Para calcular la distancia entre el objeto actual (this) y otro punto recibido como parámetro, basta con aplicar la fórmula de la distancia euclidiana entre dos coordenadas en el plano. En programación orientada a objetos, este método expresa claramente que es el propio objeto quien conoce su posición y sabe calcular la distancia respecto a otro. Desde el punto de vista conceptual, este método utiliza this para acceder a los atributos del punto actual y el parámetro para acceder a los del punto externo, de manera similar a cómo en C se pasaría un struct a una función, pero integrado dentro de la propia clase.

El uso de un parámetro de tipo Punto ilustra cómo una clase puede interactuar consigo misma, lo cual es habitual en geometría, grafos, listas enlazadas u otros escenarios donde los objetos del mismo tipo se relacionan entre sí. Además, este método complementa al ya existente calculaDistanciaAOrigen, mostrando cómo se pueden definir operaciones tanto relativas al origen como relativas a otro objeto. Aunque el uso explícito de this no siempre es obligatorio, en este contexto mejora la claridad al dejar claro qué coordenadas pertenecen a cada punto.

Aquí tienes la versión actualizada de la clase con el nuevo método distanciaA:


## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### Respuesta


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### Respuesta


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


### Respuesta


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Respuesta
