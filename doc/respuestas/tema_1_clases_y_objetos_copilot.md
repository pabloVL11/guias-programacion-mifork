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

La abstracción consiste en centrarse en los aspectos esenciales de un objeto, ocultando los detalles irrelevantes. En la práctica, esto implica diseñar clases que representen conceptos cuyo funcionamiento sea comprensible sin tener que conocerlo detalladamente.

La encapsulación se refiere a agrupar datos y funciones dentro de una misma entidad, la clase, y controlar qué partes pueden ser accedidas desde fuera. En Java esto se logra mediante modificadores como private, public o protected. Encapsular permite evitar modificaciones accidentales de datos internos y obliga a interactuar con los objetos a través de métodos bien definidos, creando interfaces más seguras y coherentes.

La herencia permite crear nuevas clases basadas en otras ya existentes, reutilizando código y evitando duplicación. Una clase hija hereda atributos y métodos de la clase padre, pudiendo extender o modificar su comportamiento mediante un sistema jerárquico.

Finalmente, el polimorfismo permite que un mismo método pueda comportarse de forma distinta según el tipo de objeto que lo utilice. En Java esto se consigue principalmente mediante la sobreescritura de métodos en clases hijas. Gracias al polimorfismo, es posible escribir código más general que trabaje con referencias a clases base, mientras que la implementación concreta se decide en tiempo de ejecución, produciendo programas más flexibles y adaptables.

## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Respuesta
Java, C++, Python, C#

## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### Respuesta
La programación estructurada es un paradigma que organiza el código en bloques lógicos utilizando únicamente tres estructuras básicas de control: secuencia, selección e iteración. Surge como medida para evitar los saltos incontrolados de lenguajes anteriores que dificultaban la comprensión del código.

La programación modular representa un paso adicional dentro del paradigma estructurado. En lugar de escribir todo el programa como un único bloque, se divide en funciones más concretas (de ahí "modular"). Esto facilita la comprensión y la depuración del código al estar dividido en fragmentos específicos.

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### Respuesta
En programación orientada a objetos, un objeto se define principalmente mediante estado, comportamiento e identidad. Estos tres elementos permiten representar entidades del mundo real o del propio programa de forma coherente, combinando datos y operaciones dentro de una misma unidad. Para alguien acostumbrado a C sin OOP, se puede pensar en un objeto como una evolución de un struct, pero con funciones asociadas y con un mecanismo formal para distinguir cada instancia.

El estado se refiere a los datos que almacena un objeto, es decir, los valores de sus atributos en un momento dado. Este estado puede cambiar a lo largo de la ejecución, igual que las variables dentro de un struct cambian según las operaciones del programa. En una clase Java, estos datos suelen declararse como variables de instancia, y representan las características que describen al objeto, como el color de un coche o el saldo de una cuenta bancaria.

El comportamiento está definido por los métodos del objeto, que especifican las acciones que puede realizar o las operaciones que pueden modifican su estado. A diferencia de C, donde las funciones están separadas de las estructuras, en Java los métodos forman parte directa de la clase, permitiendo que cada objeto "sepa" cómo actuar sobre sí mismo. Este acoplamiento entre datos y métodos es uno de los pilares que distingue la POO de la programación estructurada.

Finalmente, la identidad permite diferenciar un objeto de otro, incluso si ambos comparten el mismo estado y el mismo comportamiento. Dos objetos pueden tener idénticos valores en sus atributos, pero seguir siendo entidades independientes en memoria. En Java, esta identidad se gestiona internamente mediante referencias, que funcionan como direcciones o punteros seguros hacia cada instancia. Gracias a este concepto, la POO permite manejar múltiples objetos similares sin confundirlos entre sí.

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Respuesta
Una clase es una plantilla o modelo que describe cómo serán los objetos que se creen a partir de ella. En su interior se definen los atributos (datos) y los métodos (comportamientos) que caracterizan a ese tipo de entidad. Puede imaginarse como el plano de una casa: especifica qué tendrá la casa y cómo funcionará, pero no es una casa real todavía. En lenguajes como Java, la clase es el punto central desde el que se organiza el código y se definen las estructuras que después se utilizarán en un programa.

Un objeto no es lo mismo que una clase. Mientras que la clase es el diseño, el objeto es el resultado concreto creado a partir de ese diseño. Cada objeto tiene su propio estado, puede ejecutar los métodos definidos en la clase y ocupa un lugar específico en memoria. Siguiendo la metáfora anterior, si la clase es el plano de una casa, el objeto sería la casa construida físicamente, con su propio color, muebles y particularidades. Esta diferencia entre diseño y ejemplar es fundamental para entender cómo funciona la programación orientada a objetos.

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
Un método es una operación asociada a una clase que define el comportamiento de sus objetos. Conceptualmente, es similar a una función en C, pero “vive” dentro de la clase y suele operar sobre el estado del objeto (sus atributos). En Java, los métodos de instancia actúan sobre cada objeto concreto, mientras que los métodos estáticos (static) pertenecen a la clase en general y se invocan sin necesidad de crear una instancia. Esta integración de datos (atributos) y operaciones (métodos) es lo que distingue a la POO de la programación estructurada.

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

En este ejemplo, sumar está sobrecargado para manejar distintos tipos y cantidades de argumentos, mientras que abs ilustra un método estático. Desde el punto de vista de alguien que viene de C, puede verse como tener “varias funciones con el mismo nombre” seleccionadas por la firma, algo que en C habría que emular con nombres distintos o macros.

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


## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### Respuesta


## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### Respuesta


## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Respuesta


## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### Respuesta


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### Respuesta


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


### Respuesta


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Respuesta
