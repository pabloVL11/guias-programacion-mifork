<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Aspectos funcionales". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia, polimorfismo y genericidad.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 7. Aspectos funcionales

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.

### Respuesta

Un puntero a función es una variable que almacena la dirección de una función, de forma análoga a como un puntero normal almacena la dirección de una variable. En C, las funciones no son objetos, pero sí tienen una dirección en memoria, lo que permite pasarlas como argumentos, almacenarlas en estructuras o invocarlas indirectamente. El tipo de un puntero a función viene determinado por la firma de la función: tipo de retorno y tipos de los parámetros. Para que un puntero pueda apuntar a una función concreta, ambos deben coincidir exactamente en esa firma.

Este mecanismo resulta especialmente útil para implementar comportamientos configurables, como callbacks o tablas de funciones, y puede considerarse un antecedente directo del uso de funciones como valores en paradigmas más funcionales. A diferencia de lenguajes más modernos, en C no existe verificación semántica adicional más allá del tipo, por lo que el programador debe garantizar que la llamada indirecta sea correcta.

En el ejemplo que sigue se define una función que recibe una cadena de caracteres, convierte sus letras a mayúsculas y devuelve la misma cadena modificada. La conversión se realiza carácter a carácter utilizando la función estándar toupper. A continuación, se declara una variable local aMayusculas que es un puntero a dicha función y se invoca la función a través de ese puntero.

#include <stdio.h>
#include <ctype.h>

/* Función que convierte una cadena a mayúsculas */
char *convertirAMayusculas(char *cadena) {
    char *p = cadena;
    while (*p != '\0') {
        *p = (char) toupper((unsigned char) *p);
        p++;
    }
    return cadena;
}

int main(void) {
    char texto[] = "Hola Mundo";

    /* Puntero a función */
    char *(*aMayusculas)(char *);
    aMayusculas = convertirAMayusculas;

    /* Invocación de la función mediante el puntero */
    char *resultado = aMayusculas(texto);

    printf("%s\n", resultado);
    return 0;
}

En este código, la llamada aMayusculas(texto) resulta equivalente a una llamada directa a la función, pero se realiza de forma indirecta mediante el puntero. Este estilo de programación permite desacoplar el código que realiza la llamada de la implementación concreta de la función, una idea que más adelante reaparece en la programación orientada a objetos y, con mayor peso conceptual, en la programación funcional.


## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

### Respuesta

Una función lambda es una función anónima que puede definirse directamente en una expresión y asignarse a una variable, pasarse como argumento o devolverse como resultado de otra función. A diferencia de las funciones tradicionales, las funciones lambda no tienen nombre explícito y suelen ser breves, centrándose en expresar qué se hace más que cómo se estructura. Su introducción en muchos lenguajes modernos facilita un estilo de programación más declarativo y es uno de los pilares de la programación funcional.

Desde el punto de vista conceptual, una función lambda cumple un papel similar al de un puntero a función en C, pero con una sintaxis más compacta y segura. Mientras que en C solo se trabaja con direcciones de funciones, en lenguajes como JavaScript o Java las funciones son ciudadanos de primera clase: pueden almacenarse en variables sin necesidad de una sintaxis especial y sin separar tan claramente la función del valor que la representa. Esto acerca el modelo mental al de “funciones como datos”.

En el siguiente ejemplo en JavaScript, se define una función lambda que recibe una cadena y devuelve una nueva cadena en mayúsculas. La función se asigna a una variable local llamada aMayusculas y se invoca posteriormente a través de dicha variable, de forma muy similar al uso de un puntero a función, pero con una sintaxis más directa.

// Función lambda asignada a una variable local
let aMayusculas = (cadena) => cadena.toUpperCase();

// Invocación de la función lambda
let resultado = aMayusculas("Hola Mundo");
console.log(resultado);

En Java, las funciones lambda se introducen para trabajar con interfaces funcionales, es decir, interfaces con un único método abstracto. En este caso se emplea Function<String, String>, que representa una función que recibe un String y devuelve un String. La lambda se asigna igualmente a una variable local aMayusculas, y la invocación se realiza mediante el método apply, lo que refleja de forma explícita la llamada indirecta a la función.

import java.util.function.Function;

public class EjemploLambda {
    public static void main(String[] args) {
        // Función lambda asignada a una variable local
        Function<String, String> aMayusculas =
            cadena -> cadena.toUpperCase();

        // Invocación de la función lambda
        String resultado = aMayusculas.apply("Hola Mundo");
        System.out.println(resultado);
    }
}

Estos ejemplos muestran cómo la idea básica de “apuntar a una función” evoluciona desde los punteros a función en C hacia un modelo más expresivo y seguro, en el que las funciones se tratan como valores normales del lenguaje, integrándose de forma natural con otros mecanismos como el tipado genérico y la programación orientada a objetos.


## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

### Respuesta

El paradigma funcional es un estilo de programación basado en la evaluación de funciones matemáticas, donde el énfasis se pone en el uso de expresiones y en la transformación de datos, en lugar de en la modificación de estados internos. En este paradigma se favorecen conceptos como la inmutabilidad de los datos, la ausencia de efectos secundarios y el uso de funciones puras, es decir, funciones cuyo resultado depende únicamente de sus parámetros de entrada. Esta forma de programar contrasta con la programación imperativa tradicional en C, donde es habitual modificar variables y estructuras de datos a lo largo del tiempo.

Se denomina a lenguajes como Java 8 multi‑paradigma porque permiten combinar varios estilos de programación dentro del mismo lenguaje. Java nació como un lenguaje puramente orientado a objetos, pero con la introducción de las funciones lambda, las interfaces funcionales y las APIs basadas en operaciones sobre colecciones (como Stream), se incorporaron elementos propios del paradigma funcional. Esto no implica que Java sea un lenguaje funcional puro, sino que ofrece herramientas para aplicar este paradigma cuando resulta ventajoso, conviviendo con clases, herencia y polimorfismo.

La expresión “funciones como ciudadanos de primera clase” indica que las funciones se tratan como cualquier otro valor del lenguaje. Esto significa que pueden almacenarse en variables, pasarse como parámetros a otras funciones y devolverse como resultado, sin necesidad de mecanismos especiales como punteros a función explícitos. En JavaScript esto ocurre de forma natural, y en Java se consigue mediante interfaces funcionales, lo que supone una evolución respecto a enfoques más rígidos.

Esta característica es fundamental para el paradigma funcional, ya que permite construir funciones de orden superior y expresar el comportamiento del programa de forma más declarativa. Para alguien con experiencia en C y Java orientado a objetos, puede interpretarse como una generalización del concepto de puntero a función, pero con mayor seguridad de tipos, mejor legibilidad y una integración más directa con el resto de las abstracciones del lenguaje.


## 4. Explica la sintaxis básica de una función lambda en Java.

### Respuesta

La sintaxis básica de una función lambda en Java fue introducida a partir de Java 8 y permite definir implementaciones de métodos de forma concisa, sin necesidad de crear clases anónimas. Una lambda siempre representa la implementación de un único método abstracto de una interfaz funcional. Su estructura general se compone de una lista de parámetros, el operador -> (denominado flecha) y un cuerpo que define el comportamiento de la función.

La forma más simple de una función lambda es (parámetros) -> expresión. Si la lambda contiene una sola expresión, su valor se devuelve implícitamente, sin necesidad de usar la palabra clave return ni llaves. El tipo de los parámetros puede omitirse cuando el compilador puede inferirlo a partir del contexto, como ocurre al asignarla a una variable de tipo Function<String, String>. Este mecanismo de inferencia reduce considerablemente la verbosidad respecto a enfoques anteriores en Java.

Function<String, String> aMayusculas =
    cadena -> cadena.toUpperCase();

Cuando la lambda requiere más de una instrucción, el cuerpo debe encerrarse entre llaves {} y utilizar return de forma explícita si existe un valor de retorno. En este caso, la sintaxis se asemeja más a la de un método tradicional, aunque sigue careciendo de nombre y modificadores de acceso. Esta flexibilidad permite usar lambdas tanto para expresiones simples como para comportamientos algo más elaborados.

Function<String, String> aMayusculas = cadena -> {
    String resultado = cadena.trim();
    return resultado.toUpperCase();
};

En resumen, la sintaxis de las funciones lambda en Java se caracteriza por ser breve, declarativa y estrechamente ligada al concepto de interfaz funcional. Para alguien con experiencia en Java orientado a objetos, puede interpretarse como una alternativa compacta a las clases anónimas, y para quien conoce los punteros a función en C, como una forma más segura y expresiva de referenciar comportamiento mediante variables.


## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

### Respuesta

Recibir una función como parámetro en un método es una consecuencia directa de tratar a las funciones como ciudadanos de primera clase. En este enfoque, un método no solo recibe datos, sino también comportamiento, que puede ejecutarse desde su interior. Esto permite desacoplar el algoritmo general (por ejemplo, transformar una cadena) del detalle concreto de la transformación, haciendo el código más reutilizable y flexible.

En lenguajes como JavaScript, donde las funciones son valores nativos, este mecanismo resulta especialmente natural. Un método puede recibir una cadena y una función transformadora, e invocar dicha función sin necesidad de ningún tipo adicional. De este modo, el método transformar no conoce cómo se realiza la transformación, solo sabe que recibe una función que acepta un texto y devuelve otro texto.

// Función transformadora
let aMayusculas = (cadena) => cadena.toUpperCase();

// Método que recibe una función como parámetro
function transformar(texto, transformador) {
    return transformador(texto);
}

// Uso del método transformar
let resultado = transformar("Hola Mundo", aMayusculas);
console.log(resultado);

En Java, el mismo concepto se expresa utilizando interfaces funcionales. El método transformar recibe un String y una referencia a una función del tipo Function<String, String>. Desde el interior del método, la función se invoca mediante apply. Aunque la sintaxis resulta algo más explícita que en JavaScript, el principio es el mismo: el comportamiento se pasa como argumento.

import java.util.function.Function;

public class EjemploTransformar {

    static String transformar(String texto, Function<String, String> transformador) {
        return transformador.apply(texto);
    }

    public static void main(String[] args) {
        Function<String, String> aMayusculas =
            cadena -> cadena.toUpperCase();

        String resultado = transformar("Hola Mundo", aMayusculas);
        System.out.println(resultado);
    }
}

Este patrón es fundamental en la programación funcional y aparece con frecuencia en las APIs modernas de Java, como Stream. Para alguien con experiencia en C, puede verse como una evolución del paso de punteros a funciones; para alguien con base en Java orientado a objetos, como una generalización del uso de estrategias o comportamientos intercambiables, expresados ahora de forma más concisa y declarativa.


## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

### Respuesta

Invocar un método pasando una función lambda definida directamente en la llamada es una práctica habitual en el paradigma funcional, ya que permite expresar el comportamiento justo en el punto donde se necesita. De esta forma se evita definir una variable previa cuando la función solo va a utilizarse una vez, reduciendo código incidental y mejorando la legibilidad. Esta técnica refuerza la idea de que las funciones no son entidades especiales, sino valores que pueden construirse “al vuelo”.

En JavaScript, esta capacidad es especialmente natural, ya que las funciones son objetos de primera clase desde el diseño original del lenguaje. El método transformar puede recibir directamente una función lambda que invierta la cadena, sin necesidad de asignarla previamente a una variable. La lógica de inversión se expresa en el momento de la llamada, haciendo evidente qué transformación se está aplicando.

function transformar(texto, transformador) {
    return transformador(texto);
}

let resultado = transformar("Hola Mundo", (cadena) =>
    cadena.split("").reverse().join("")
);

console.log(resultado);

En Java, aunque la sintaxis es algo más explícita, el concepto es exactamente el mismo. Se pasa una función lambda que implementa la interfaz funcional Function<String, String> directamente como argumento del método transformar. El compilador se encarga de inferir el tipo de la lambda a partir de la firma del método, siempre que no exista ambigüedad.

import java.util.function.Function;

public class EjemploTransformar {

    static String transformar(String texto, Function<String, String> transformador) {
        return transformador.apply(texto);
    }

    public static void main(String[] args) {
        String resultado = transformar(
            "Hola Mundo",
            cadena -> new StringBuilder(cadena).reverse().toString()
        );

        System.out.println(resultado);
    }
}

Este estilo de invocación destaca una de las principales ventajas del paradigma funcional: la posibilidad de pasar comportamiento de forma directa y localizada. Para alguien con experiencia en C, puede verse como una llamada con un puntero a función definido en línea; para quien conoce Java orientado a objetos, supone una alternativa más compacta y expresiva a patrones clásicos como Strategy, sin necesidad de clases adicionales.


## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

### Respuesta

Un cierre o closure se entiende como una función que, además de su propio código, captura y mantiene acceso a variables del contexto en el que fue definida, incluso aunque ese contexto ya no esté activo cuando la función se ejecuta. En otras palabras, la función “recuerda” el entorno léxico en el que nació. Este concepto es fundamental en el paradigma funcional, ya que permite escribir funciones que dependen de datos externos sin necesidad de almacenarlos en estructuras globales ni pasarlos explícitamente como parámetros.

En Java, las funciones lambda pueden formar cierres, pero con una restricción importante: solo pueden capturar variables locales que sean efectivamente finales (effectively final). Esto significa que la variable no necesita declararse con final, pero su valor no debe modificarse después de su inicialización. Esta restricción garantiza seguridad y evita problemas de concurrencia o ambigüedad en la gestión de memoria, algo especialmente relevante en un lenguaje orientado a objetos con gestión automática de memoria.

A partir del ejemplo anterior, se puede definir una variable local fuera de la función lambda que contenga un texto adicional. La lambda, al acceder a dicha variable, forma un cierre. Aunque la variable no forme parte de los parámetros de la función, puede utilizarse dentro de ella como si fuera propia de su implementación.

import java.util.function.Function;

public class EjemploClosure {

    static String transformar(String texto, Function<String, String> transformador) {
        return transformador.apply(texto);
    }

    public static void main(String[] args) {
        String sufijo = " - procesado";

        String resultado = transformar(
            "Hola Mundo",
            cadena -> cadena + sufijo
        );

        System.out.println(resultado);
    }
}

En este caso, la función lambda captura la variable local sufijo, definida fuera de su cuerpo, y la utiliza para construir el resultado. Este comportamiento ilustra claramente el concepto de cierre: la lambda no solo representa una función, sino también el contexto en el que fue creada. Para alguien con experiencia en C, este mecanismo no tiene un equivalente directo, y en Java orientado a objetos supone una forma más expresiva y controlada de compartir estado entre funciones sin recurrir a atributos de clase o variables globales.


## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

### Respuesta

Una diferencia fundamental entre una función lambda y un puntero a función en C es el nivel de abstracción y el modelo conceptual que representan. Un puntero a función en C es, estrictamente, una dirección de memoria que apunta al código de una función concreta, y su uso se limita a invocar ese código de forma indirecta. No existe ninguna noción asociada de contexto, ni de comportamiento autosuficiente: el puntero solo referencia una función previamente definida, separada de los datos que pudiera necesitar.

Las funciones lambda, en cambio, son expresiones que crean funciones como valores completos del lenguaje. No solo encapsulan código, sino que también pueden capturar variables del entorno en el que se definen, formando cierres. Esto significa que una lambda puede llevar consigo parte de su contexto, algo que no es posible con los punteros a funciones en C sin recurrir a estructuras adicionales y a una gestión manual del estado. Además, las lambdas están integradas en el sistema de tipos del lenguaje, lo que permite una comprobación más segura y explícita de su uso.

Otra diferencia importante reside en la expresividad y seguridad. En C, el programador es responsable de asegurar que la firma del puntero coincide exactamente con la de la función apuntada, ya que los errores pueden derivar en comportamiento indefinido. En lenguajes como Java, las funciones lambda solo pueden asignarse a interfaces funcionales compatibles, y el compilador verifica esta correspondencia. Asimismo, las lambdas fomentan un estilo declarativo y conciso, mientras que los punteros a función suelen requerir más código auxiliar y una mayor disciplina por parte del programador.

En conjunto, puede interpretarse que las funciones lambda generalizan y superan la idea de los punteros a función. Mientras que estos últimos proporcionan un mecanismo básico para referenciar funciones, las lambdas ofrecen una abstracción más rica, segura e integrada con el lenguaje, alineada con los principios del paradigma funcional y fácilmente combinable con la orientación a objetos.


## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

### Respuesta

Devolver funciones es una característica clave del paradigma funcional y permite construir funciones dinámicamente, en función de ciertos parámetros. En este caso, se propone una función crearDescuento(porcentaje) que no aplica directamente el descuento, sino que devuelve otra función, denominada función descuento. Esta función devuelta recibirá una cantidad y aplicará sobre ella el porcentaje especificado previamente. El tipo Function<Double, Double> se utiliza para representar tanto la función devuelta como las funciones de descuento resultantes.

Desde el punto de vista conceptual, crearDescuento actúa como una fábrica de funciones. A partir de un porcentaje concreto, se construye una función lambda que encapsula ese porcentaje y lo usa posteriormente para calcular el precio con descuento. Esta técnica es un ejemplo claro de programación funcional: las funciones se generan, se devuelven como valores y se almacenan en variables, de forma análoga a cualquier otro dato.

import java.util.function.Function;

public class EjemploDescuentos {

    static Function<Double, Double> crearDescuento(double porcentaje) {
        return cantidad -> cantidad * (1 - porcentaje / 100);
    }

    public static void main(String[] args) {
        Function<Double, Double> descuento10 = crearDescuento(10.0);
        Function<Double, Double> descuento25 = crearDescuento(25.0);

        double precio = 200.0;

        System.out.println(descuento10.apply(precio)); // 180.0
        System.out.println(descuento25.apply(precio)); // 150.0
    }
}

En este ejemplo se crean dos funciones de descuento distintas a partir de la misma función generadora. Cada una conserva su propio porcentaje, aun cuando dicho porcentaje ya no forma parte del ámbito activo del método crearDescuento. Esto es posible gracias a que la lambda forma una closure: captura la variable local porcentaje y la mantiene asociada a la función devuelta. Así, cada función descuento “recuerda” el valor con el que fue creada, demostrando cómo las lambdas combinan código y contexto en una única entidad funcional.


## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

### Respuesta

En Java, una interfaz funcional es una interfaz que define exactamente un método abstracto, y sirve como tipo objetivo (target type) para una función lambda. Dado que Java es un lenguaje con comprobación estática de tipos, toda función lambda debe asociarse a un tipo concreto en tiempo de compilación, y ese tipo es siempre una interfaz funcional. De este modo, una lambda no existe de forma aislada, sino que representa la implementación de un único método definido por dicha interfaz.

El requisito fundamental de una interfaz funcional es tener un solo método abstracto. Este método define la firma que debe cumplir la función lambda: número y tipo de parámetros, así como el tipo de retorno. Si existiera más de un método abstracto, el compilador no podría determinar qué método está implementando la lambda, por lo que dejaría de ser válida como tipo de una función lambda. Ejemplos habituales de interfaces funcionales en la biblioteca estándar son Runnable, Comparator<T> o Function<T, R>.

Una interfaz funcional puede, no obstante, contener métodos default y métodos static, sin que esto viole su condición de funcional. Estos métodos no cuentan como abstractos, ya que tienen implementación o no pertenecen a la instancia. Además, los métodos heredados de Object, como toString o equals, tampoco se consideran métodos abstractos a efectos de este cómputo, incluso aunque se redefinan en la interfaz.

Opcionalmente, una interfaz funcional puede anotarse con @FunctionalInterface. Esta anotación no es obligatoria, pero resulta muy recomendable, ya que fuerza al compilador a verificar que la interfaz cumple realmente los requisitos. En conjunto, las interfaces funcionales actúan como el puente de tipado que permite integrar las funciones lambda dentro del sistema de tipos de Java, manteniendo la seguridad estática propia del lenguaje y habilitando, al mismo tiempo, un estilo de programación funcional.


## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

### Respuesta

Una interfaz funcional definida por el programador sirve para describir explícitamente el tipo de una función lambda que se desea utilizar en un determinado contexto. En este caso, se pretende modelar una función cuyo objetivo sea transformar una cadena de texto en otra, por lo que la interfaz debe declarar un único método abstracto que reciba un String y devuelva un String. Esta interfaz actuará como contrato, indicando qué tipo de funciones pueden utilizarse como transformadoras.

Para que una interfaz sea considerada funcional, debe cumplir el requisito esencial de tener exactamente un método abstracto. Puede incluir, si se desea, métodos default o static, pero solo uno puede carecer de implementación. La anotación @FunctionalInterface no es obligatoria, pero se recomienda, ya que permite al compilador comprobar que la interfaz mantiene correctamente su naturaleza funcional y evita errores si se añaden métodos abstractos accidentalmente.

A continuación se muestra la definición de la interfaz funcional Transformador, adaptada al ejemplo trabajado previamente, donde una cadena se convierte en otra. Esta interfaz puede utilizarse como tipo para funciones lambda, referencias a métodos o implementaciones mediante clases tradicionales.

@FunctionalInterface
public interface Transformador {
    String transformar(String texto);
}

Una vez definida esta interfaz, cualquier función lambda que reciba un String y devuelva un String podrá asignarse a una referencia de tipo Transformador. De este modo, se obtiene una alternativa más semántica y específica que el uso de Function<String, String>, mejorando la legibilidad del código y reforzando la idea de que las funciones también pueden formar parte explícita del diseño del dominio del programa.


## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

### Respuesta

Para hacer la interfaz funcional más genérica, se puede recurrir a genéricos, de forma que no quede limitada a transformar únicamente cadenas. En lugar de fijar los tipos de entrada y salida a String, se definen parámetros de tipo que representen el tipo de entrada y el tipo de salida. Así, la interfaz Transformador pasa a describir una transformación de un tipo T en otro tipo R, manteniendo la misma idea conceptual pero con mayor reutilización y flexibilidad.

Desde el punto de vista de Java, esta generalización encaja de manera natural con el sistema de tipos genéricos ya conocido por colecciones como List<T> o Map<K, V>. El requisito clave sigue siendo el mismo: la interfaz debe tener un único método abstracto, cuya firma ahora utiliza los tipos genéricos. La anotación @FunctionalInterface sigue siendo válida y recomendable, ya que garantiza que la interfaz pueda usarse como tipo objetivo de una función lambda.

La interfaz funcional genérica Transformador<T, R> puede definirse de la siguiente manera, indicando que transforma un valor de tipo T en otro de tipo R:

@FunctionalInterface
public interface Transformador<T, R> {
    R transformar(T valor);
}

Una vez definida, se puede crear fácilmente un transformador concreto mediante una función lambda. Por ejemplo, para transformar un Double en un Integer redondeando el valor, se define una lambda que implemente esa conversión explícitamente. En el ejemplo siguiente, la lógica de redondeo se expresa de forma directa y concisa:

public class EjemploTransformador {

    public static void main(String[] args) {
        Transformador<Double, Integer> redondear =
            valor -> (int) Math.round(valor);

        Integer resultado = redondear.transformar(3.6);
        System.out.println(resultado); // 4
    }
}
``

Este ejemplo muestra cómo las interfaces funcionales genéricas permiten modelar transformaciones de distinto tipo sin perder seguridad estática. Además, refuerza el enfoque funcional en Java, donde el comportamiento se describe mediante funciones tipadas, combinables y reutilizables, sin necesidad de crear múltiples interfaces específicas para cada combinación concreta de tipos.


## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

### Respuesta

En Java, a partir de Java 8, se introdujo un conjunto de interfaces funcionales predefinidas en el paquete java.util.function, con el objetivo de cubrir los casos de uso más habituales al trabajar con funciones lambda. Estas interfaces evitan la necesidad de crear nuevas interfaces funcionales para transformaciones, predicados o acciones comunes, favoreciendo la reutilización, la consistencia del código y la interoperabilidad con las APIs estándar, como Stream.

La interfaz más general es Function<T, R>, que representa una función que transforma un valor de tipo T en otro de tipo R, exactamente como el Transformador<T, R> definido anteriormente. A partir de ella derivan variantes especializadas, como UnaryOperator<T> (cuando entrada y salida son del mismo tipo) y BinaryOperator<T> (cuando se combinan dos valores del mismo tipo para producir otro del mismo tipo). Estas interfaces cubren gran parte de los escenarios típicos de transformación de datos.

Otro grupo importante lo forman las interfaces relacionadas con condiciones lógicas y acciones sin valor de retorno. Predicate<T> representa una función que recibe un valor y devuelve un boolean, siendo habitual en filtrados. Consumer<T> describe una operación que recibe un valor y no devuelve resultado, utilizándose para acciones con efectos secundarios, como mostrar datos. En el caso opuesto, Supplier<T> representa una función que no recibe parámetros y devuelve un valor, útil para generación diferida de datos.

Además de estas interfaces genéricas, Java proporciona versiones especializadas para tipos primitivos, como IntFunction<R>, DoublePredicate o LongConsumer, con el fin de evitar el coste del boxing y unboxing. En conjunto, este conjunto de interfaces funcionales predefinidas constituye la base tipada que permite a Java integrar el paradigma funcional dentro de un lenguaje con orientación a objetos y comprobación estática de tipos, haciendo innecesaria la creación de interfaces funcionales propias en la mayoría de los casos comunes.


## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

### Respuesta

El método forEach de List representa una forma funcional de recorrer colecciones en Java y puede considerarse una alternativa al bucle for tradicional. Introducido en Java 8, permite expresar qué se quiere hacer con cada elemento de la colección, delegando el control de la iteración a la propia estructura de datos. Esto encaja con el enfoque declarativo del paradigma funcional y reduce el código repetitivo asociado a los bucles explícitos con índices o iteradores.

Desde el punto de vista conceptual, forEach recibe una función que se aplica a cada elemento de la lista. Dicha función suele expresarse mediante una función lambda que implementa la interfaz funcional Consumer<T>, ya que no devuelve ningún valor y actúa sobre cada elemento. En comparación con un for clásico, el código resultante suele ser más legible y deja claro que la intención es procesar cada elemento, no controlar manualmente el recorrido.

En el siguiente ejemplo se recorre una lista de enteros utilizando forEach. Para cada número, la función lambda comprueba si es positivo y, en ese caso, muestra un mensaje por pantalla. La condición lógica forma parte del comportamiento pasado como parámetro, no del mecanismo de iteración.

import java.util.List;

public class EjemploForEach {

    public static void main(String[] args) {
        List<Integer> numeros = List.of(-2, 0, 5, 10, -7);

        numeros.forEach(numero -> {
            if (numero > 0) {
                System.out.println(numero + " es positivo");
            }
        });
    }
}

Este ejemplo ilustra cómo forEach permite combinar estructuras orientadas a objetos con funciones lambda de estilo funcional. Para alguien con experiencia previa en Java o C, puede verse como una evolución del bucle for, donde la iteración se abstrae y el foco se sitúa en la acción que se desea realizar sobre cada elemento.

## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### Respuesta

La firma de forEach en Java es void forEach(Consumer<? super T> action) y no Consumer<T> por una razón relacionada con la varianza de los genéricos. En Java, los genéricos son invariantes, lo que significa que Consumer<Number> no es subtipo de Consumer<Integer>, aunque Number sea supertipo de Integer. El uso de ? super T permite pasar consumidores que acepten no solo exactamente T, sino también cualquier supertipo de T, haciendo la API más flexible sin perder seguridad de tipos.

Esta decisión se explica mediante el principio conocido como PECS, acrónimo de Producer Extends, Consumer Super. Este principio indica que, cuando una estructura o parámetro produce valores de tipo T, se debe usar ? extends T, y cuando consume valores de tipo T, se debe usar ? super T. En el caso de forEach, la lista produce elementos de tipo T y la función recibida los consume, por lo que tiene sentido permitir consumidores de T o de cualquier supertipo de T, como Object.

Aplicando PECS al ejemplo del método transformar, se puede mejorar su flexibilidad ajustando el tipo de la función transformadora. En lugar de usar Function<T, R>, resulta más general emplear Function<? super T, ? extends R>. De este modo, la función puede aceptar como entrada un supertipo de T (porque consume el valor) y devolver un subtipo de R (porque produce el resultado), ampliando los casos compatibles sin comprometer el tipado estático.

static <T, R> R transformar(T valor, Function<? super T, ? extends R> transformador) {
    return transformador.apply(valor);
}

Este uso de PECS permite diseñar APIs genéricas más reutilizables y expresivas, alineadas con el enfoque funcional introducido en Java 8. Para alguien con experiencia previa en genéricos, este principio resulta clave para entender por qué muchas firmas estándar de Java no utilizan tipos genéricos “directos”, sino comodines acotados que capturan correctamente el rol de productor o consumidor de cada componente.

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

### Respuesta

Las referencias a métodos permiten tratar un método existente como un valor, de forma que pueda almacenarse en una variable, pasarse como parámetro o invocarse posteriormente sin llamarlo de forma inmediata. Conceptualmente, están muy relacionadas con las funciones lambda, ya que ambas representan comportamiento. La diferencia principal es que una referencia a método no define un comportamiento nuevo, sino que reutiliza directamente un método ya existente de una clase o de un objeto concreto.

En lenguajes donde las funciones son ciudadanos de primera clase, como JavaScript, obtener una referencia a un método de un objeto es directo. Un método es, en realidad, una función asociada a un objeto, y puede asignarse a una variable. Es importante tener en cuenta el valor de this, ya que al separar el método del objeto puede perderse el contexto original si no se gestiona correctamente.

class Persona {
    constructor(nombre) {
        this.nombre = nombre;
    }

    saludar() {
        console.log("Hola, soy " + this.nombre);
    }
}

// Código principal
let persona = new Persona("Ana");

// Referencia al método saludar
let saludarRef = persona.saludar.bind(persona);

// Invocación mediante la referencia
saludarRef();
``

En Java, las referencias a métodos se introducen junto con las funciones lambda en Java 8 y están estrechamente ligadas a las interfaces funcionales. En este caso, se puede obtener una referencia a un método de instancia usando la sintaxis objeto::metodo. Dicha referencia se almacena en una variable cuyo tipo sea una interfaz funcional compatible, como Runnable cuando el método no recibe parámetros ni devuelve valor.

public class Persona {
    private String nombre;

    public Persona(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

// Código principal
public class EjemploReferenciaMetodo {

    public static void main(String[] args) {
        Persona persona = new Persona("Ana");

        // Referencia al método saludar
        Runnable saludarRef = persona::saludar;

        // Invocación mediante la referencia
        saludarRef.run();
    }
}

Estos ejemplos muestran cómo las referencias a métodos permiten reutilizar comportamiento existente de forma elegante y expresiva. Para alguien con experiencia en punteros a funciones en C, pueden verse como una generalización más segura y rica; para quien conoce Java orientado a objetos, representan una integración natural entre métodos y programación funcional, evitando código redundante y mejorando la claridad del programa.


## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### Respuesta

En Java existen cuatro tipos principales de referencias a métodos, todas introducidas en Java 8 como parte de la integración del paradigma funcional. Una referencia a método es una forma compacta de expresar una función lambda cuyo cuerpo se limita a llamar a un método existente. Su sintaxis general utiliza el operador :: y siempre debe ser compatible con una interfaz funcional, ya que la referencia representa la implementación de su único método abstracto.

El primer tipo es la referencia a un método estático, que toma la forma Clase::metodoEstatico. Se utiliza cuando el comportamiento está definido directamente en la clase y no depende de ninguna instancia concreta. El compilador asocia la firma del método estático con la de la interfaz funcional correspondiente.

import java.util.function.Function;

public class Utilidades {
    public static String aMayusculas(String texto) {
        return texto.toUpperCase();
    }

    public static void main(String[] args) {
        Function<String, String> ref = Utilidades::aMayusculas;
        System.out.println(ref.apply("hola"));
    }
}

El segundo tipo es la referencia a un constructor, que se expresa como Clase::new. En este caso, la referencia no apunta a un método existente, sino a la operación de creación de objetos. La firma del constructor debe coincidir con la del método abstracto de la interfaz funcional.

import java.util.function.Function;

class Persona {
    private String nombre;

    public Persona(String nombre) {
        this.nombre = nombre;
    }
}

public class EjemploConstructor {
    public static void main(String[] args) {
        Function<String, Persona> ref = Persona::new;
        Persona p = ref.apply("Ana");
    }
}

El tercer tipo es la referencia a un método de instancia de una instancia concreta, con la forma objeto::metodo. Aquí, la instancia ya existe y queda ligada a la referencia. Cuando se invoca la función, el método se ejecuta sobre ese mismo objeto.

class Persona {
    private String nombre;

    Persona(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

public class EjemploInstanciaConcreta {
    public static void main(String[] args) {
        Persona persona = new Persona("Ana");
        Runnable ref = persona::saludar;
        ref.run();
    }
}

El cuarto tipo es la referencia a un método de instancia sobre cualquier instancia de una clase, con la forma Clase::metodo. En este caso, el objeto sobre el que se invoca el método no está fijado de antemano, sino que se pasa implícitamente como primer parámetro en la invocación funcional. Este tipo es habitual en operaciones sobre colecciones.

import java.util.function.Function;

public class EjemploCualquierInstancia {
    public static void main(String[] args) {
        Function<String, Integer> ref = String::length;
        System.out.println(ref.apply("Hola"));
    }
}

En conjunto, estos cuatro tipos de referencias a método permiten reutilizar código existente de forma clara y expresiva, reduciendo la necesidad de escribir funciones lambda explícitas. Representan una integración natural entre el modelo orientado a objetos de Java y el enfoque funcional, facilitando un estilo de programación más declarativo y legible.


## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

### Respuesta

Ordenar una colección de objetos en Java es un ejemplo muy representativo del uso de programación funcional integrada con la orientación a objetos. En este caso, Collections.sort recibe como segundo parámetro un comparador, es decir, una función que define el criterio de ordenación entre dos elementos. Con la llegada de las funciones lambda, este comparador puede expresarse de forma directa y concisa, sin necesidad de crear clases adicionales.

En la primera versión, el criterio de comparación se define manualmente dentro de una expresión lambda que implementa la interfaz funcional Comparator<Persona>. La lambda recibe dos objetos Persona, compara primero la edad y, solo si esta es igual, compara los nombres en orden alfabético. La lógica queda completamente explícita y cercana a la definición formal del orden.

import java.util.*;

class Persona {
    String nombre;
    int edad;

    Persona(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }

    @Override
    public String toString() {
        return nombre + " (" + edad + ")";
    }
}

public class EjemploOrdenacionManual {

    public static void main(String[] args) {
        List<Persona> personas = new ArrayList<>(List.of(
            new Persona("Ana", 30),
            new Persona("Luis", 25),
            new Persona("Pedro", 30),
            new Persona("Marta", 25)
        ));

        Collections.sort(personas, (p1, p2) -> {
            int cmpEdad = Integer.compare(p1.edad, p2.edad);
            if (cmpEdad != 0) {
                return cmpEdad;
            }
            return p1.nombre.compareTo(p2.nombre);
        });

        System.out.println(personas);
    }
}
``

En la segunda versión, se utiliza la clase auxiliar Comparator, que ofrece métodos estáticos y por defecto para construir comparadores de forma más declarativa. Mediante Comparator.comparing se define el primer criterio (edad) y con thenComparing se encadena el segundo criterio (nombre). El resultado es un código más compacto y expresivo, que describe qué se compara en lugar de cómo se implementa la comparación paso a paso.

import java.util.*;

public class EjemploOrdenacionComparator {

    public static void main(String[] args) {
        List<Persona> personas = new ArrayList<>(List.of(
            new Persona("Ana", 30),
            new Persona("Luis", 25),
            new Persona("Pedro", 30),
            new Persona("Marta", 25)
        ));

        Collections.sort(
            personas,
            Comparator
                .comparing((Persona p) -> p.edad)
                .thenComparing(p -> p.nombre)
        );

        System.out.println(personas);
    }
}

Ambas soluciones son correctas, pero la segunda refleja mejor el estilo funcional moderno de Java, al reutilizar utilidades estándar y reducir el código imperativo. Este tipo de expresividad es uno de los principales beneficios de la introducción de lambdas y comparadores funcionales en Java 8, especialmente en operaciones habituales como la ordenación de colecciones.