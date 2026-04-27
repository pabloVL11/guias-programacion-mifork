<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Polimorfismo". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones, Composición y Herencia.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

### Respuesta

El polimorfismo en programación orientada a objetos se define como la capacidad de utilizar una misma referencia para tratar objetos de distintas clases relacionadas por herencia, permitiendo que un mismo mensaje (llamada a método) produzca comportamientos diferentes según el objeto concreto que lo recibe. Este concepto se apoya en la existencia de una jerarquía de clases y permite trabajar con objetos de forma general, sin conocer su tipo exacto en tiempo de compilación. De este modo, se favorece la reutilización de código y se reduce el acoplamiento entre las distintas partes de un programa.

El polimorfismo sirve, principalmente, para escribir código más flexible y extensible. Por ejemplo, se puede definir una variable de un tipo base y asignarle objetos de distintas subclases, confiando en que cada uno responderá correctamente a las operaciones definidas. Esto resulta especialmente útil cuando se manejan colecciones de objetos relacionados o cuando se diseñan sistemas en los que se espera añadir nuevas clases sin modificar el código existente, siguiendo una idea similar al uso de funciones genéricas en C, pero con mayor seguridad y claridad.

La sobreescritura de métodos es el mecanismo que hace posible el polimorfismo en Java. Consiste en que una subclase proporcione su propia implementación de un método heredado de la clase base, manteniendo la misma firma (nombre, parámetros y tipo de retorno). Cuando un método sobrescrito se invoca a través de una referencia al tipo base, Java decide en tiempo de ejecución qué versión del método ejecutar, en función del tipo real del objeto. Este comportamiento dinámico es una diferencia clave respecto a C o al uso de funciones no virtuales en lenguajes procedimentales.

class Animal {
    void hacerSonido() {
        System.out.println("El animal hace un sonido");
    }
}

class Perro extends Animal {
    @Override
    void hacerSonido() {
        System.out.println("El perro ladra");
    }
}

// Uso polimórfico
Animal a = new Perro();
a.hacerSonido(); // Se ejecuta la versión de Perro




## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

### Respuesta

La ligadura dinámica o enlace tardío consiste en que la decisión de qué implementación de un método se va a ejecutar no se toma en tiempo de compilación, sino en tiempo de ejecución, en función del tipo real del objeto al que apunta una referencia. Frente a la ligadura estática (o enlace temprano), donde la llamada a una función o método queda fijada al compilar, la ligadura dinámica permite cambiar el comportamiento sin cambiar el código que realiza la llamada. Este mecanismo es fundamental en la programación orientada a objetos moderna.

La relación entre ligadura dinámica y polimorfismo es directa: el polimorfismo de subtipos solo es posible gracias al enlace tardío. Cuando se declara una variable de un tipo base y se le asigna un objeto de una subclase, es la ligadura dinámica la que garantiza que se ejecute el método sobrescrito correspondiente al objeto concreto. Sin ligadura dinámica, todas las llamadas se resolverían según el tipo de la referencia, perdiendo el comportamiento diferenciado que caracteriza al polimorfismo.

En C++, la ligadura dinámica no es automática: solo se produce si los métodos se declaran como virtual en la clase base. Si no se utiliza virtual, el enlace es estático, incluso aunque se use herencia. Por tanto, en C++ el programador debe indicar explícitamente qué métodos pueden resolverse dinámicamente, lo que ofrece mayor control pero también más responsabilidad. En cambio, en Java, la ligadura dinámica es el comportamiento por defecto para los métodos de instancia: cualquier método puede ser sobrescrito y se resolverá dinámicamente sin necesidad de indicarlo. Solo ciertos casos especiales, como métodos static, final o private, utilizan ligadura estática.

En Python, la ligadura dinámica es aún más flexible y forma parte del propio modelo del lenguaje. Python es dinámicamente tipado y utiliza lo que se conoce como duck typing, lo que implica que las llamadas a métodos siempre se resuelven en tiempo de ejecución según el objeto real. No es necesario declarar herencia explícita para aprovechar este comportamiento, y tampoco existen palabras clave equivalentes a virtual. Esto hace que el polimorfismo y el enlace tardío estén siempre presentes, aunque con menos comprobaciones en tiempo de compilación que en Java o C++.

## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

### Respuesta

A continuación se muestra un ejemplo sencillo que ilustra el polimorfismo mediante sobreescritura de métodos en Java. Se define una clase base Soldado con un método saludar, y dos subclases, Zapador y Artillero. En este caso, la subclase Zapador sobrescribe completamente el comportamiento del método saludar, mientras que Artillero mantiene una versión propia diferenciada. La relación de herencia permite que ambas subclases sean tratadas como Soldado.

El ejemplo permite observar que, aunque se empleen referencias del tipo Soldado, el método que se ejecuta depende del tipo real del objeto al que se hace referencia. Esto demuestra el funcionamiento del polimorfismo junto con la ligadura dinámica, ya que la decisión sobre qué versión del método se ejecuta se toma en tiempo de ejecución y no en compilación. El código que recorre los objetos no necesita conocer el tipo concreto de cada soldado.

Este enfoque facilita la extensibilidad del programa, ya que podrían añadirse nuevos tipos de soldados sin modificar el código que los utiliza. Basta con que las nuevas clases hereden de Soldado y sobrescriban el método saludar si se desea un comportamiento distinto. De este modo, el polimorfismo contribuye a un diseño más modular y mantenible.

class Soldado {
    void saludar() {
        System.out.println("El soldado saluda de forma general.");
    }
}

class Zapador extends Soldado {
    @Override
    void saludar() {
        System.out.println("El zapador saluda mientras prepara explosivos.");
    }
}

class Artillero extends Soldado {
    @Override
    void saludar() {
        System.out.println("El artillero saluda desde su posición.");
    }
}

public class PruebaPolimorfismo {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[2];
        ejercito[0] = new Zapador();
        ejercito[1] = new Artillero();

        for (Soldado s : ejercito) {
            s.saludar();
        }
    }
}



## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

### Respuesta

Sí, al sobreescribir un método es posible reutilizar el comportamiento de la clase base e invocar el método original para ampliar o modificar ligeramente su resultado, en lugar de sustituirlo por completo. Esto resulta útil cuando se desea mantener la lógica común definida en la superclase y añadir solo una pequeña variación en la subclase. De este modo, se evita duplicar código y se respeta el diseño jerárquico de las clases.

En Java, esta posibilidad se ofrece mediante la palabra clave super, que permite acceder explícitamente a los miembros (métodos o atributos) de la clase base. Al llamar a super.saludar() dentro del método sobrescrito, se fuerza la ejecución de la versión definida en Soldado, incluso aunque exista una versión redefinida en Zapador. Posteriormente, se puede añadir comportamiento adicional antes o después de dicha llamada.

Este mecanismo no rompe el polimorfismo, ya que la llamada al método sigue resolviéndose dinámicamente: es la versión de Zapador la que se ejecuta, pero esta decide apoyarse en la implementación base. Esto permite construir comportamientos más complejos a partir de otros más generales y claros, manteniendo la coherencia del diseño orientado a objetos.

class Soldado {
    void saludar() {
        System.out.println("El soldado saluda de forma general.");
    }
}

class Zapador extends Soldado {
    @Override
    void saludar() {
        super.saludar(); // Invoca el método de la clase base
        System.out.println("ZAPADOR A SUS ÓRDENES");
    }
}

La palabra clave utilizada para invocar el método de la clase base es super.


## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

### Respuesta

Al sobreescribir un método en Java existen varias restricciones destinadas a mantener la coherencia del sistema de herencia. Los parámetros deben coincidir exactamente en número, orden y tipo con los del método de la clase base; no es posible cambiarlos para considerarlo sobreescritura. El tipo de retorno debe ser el mismo o uno covariante, es decir, un subtipo del tipo de retorno original. Además, el método sobrescrito no puede tener un nivel de acceso más restrictivo que el del método heredado (por ejemplo, no se puede pasar de public a protected), y las excepciones comprobadas lanzadas deben ser iguales o más específicas.

La sobreescritura (overriding) y la sobrecarga (overloading) son conceptos distintos aunque a menudo se confunden. La sobreescritura ocurre entre una clase base y una subclase, con el mismo método redefinido para cambiar o extender su comportamiento, y depende del tipo real del objeto en tiempo de ejecución (ligadura dinámica). En cambio, la sobrecarga consiste en definir varios métodos con el mismo nombre pero con distintos parámetros dentro de una misma clase (o jerarquía), y la elección del método se hace en tiempo de compilación. Por tanto, la sobrecarga no está relacionada con el polimorfismo dinámico.

La anotación @Override se utiliza para indicar explícitamente que un método pretende sobrescribir otro de la clase base. Su función principal es ayudar al compilador a detectar errores: si la firma no coincide exactamente o el método no existe en la superclase, el compilador generará un error. Esto evita fallos sutiles, como errores tipográficos o confusiones con la sobrecarga, que de otro modo pasarían desapercibidos.

El uso de @Override es altamente recomendable porque mejora la legibilidad del código, documenta claramente la intención del programador y aumenta la seguridad durante el mantenimiento y la evolución del software. En proyectos reales, esta anotación actúa como una forma de autocontrol que reduce errores y facilita la comprensión de las jerarquías de herencia.


## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

### Respuesta

En Java, el polimorfismo se utiliza prácticamente desde el principio, incluso antes de que se estudie de forma explícita como concepto teórico. Esto se debe a que todas las clases en Java heredan, directa o indirectamente, de la clase Object. Por tanto, cuando se sobrescriben métodos como toString o equals, ya se está modificando el comportamiento de métodos heredados y confiando en que se invoque la versión correcta en función del objeto real, lo cual es precisamente polimorfismo con ligadura dinámica.

Al sobrescribir toString, se define cómo debe representarse un objeto como cadena. Cuando ese objeto se utiliza en un contexto genérico (por ejemplo, al imprimirlo con System.out.println), Java llama internamente a toString usando una referencia de tipo Object. Es el tipo real del objeto el que determina qué versión del método se ejecuta. Aunque en ese momento no se hable explícitamente de polimorfismo, el mecanismo ya está actuando de forma transparente.

Algo similar ocurre con equals. Al sobrescribir este método, se adapta la forma en que los objetos se comparan por igualdad lógica en lugar de por identidad. De nuevo, el uso habitual de equals se realiza a través de referencias de tipo Object, y es la implementación concreta la que se ejecuta en tiempo de ejecución. Esto implica que ya se está aprovechando el polimorfismo para personalizar el comportamiento de clases propias sin modificar el código que las utiliza.

Por tanto, puede afirmarse que el aprendizaje de Java introduce el polimorfismo de manera implícita y progresiva. Primero se usa sin nombrarlo, mediante la sobreescritura de métodos heredados, y más adelante se formaliza el concepto al estudiar herencia, referencias a la clase base y colecciones de objetos heterogéneos. Esto facilita una transición natural desde una programación más estructurada hacia un enfoque plenamente orientado a objetos.


## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

### Respuesta

Una clase abstracta es una clase que no está pensada para ser instanciada directamente, sino para servir como base común de otras clases. Se utiliza cuando se quiere definir un comportamiento general, compartido por varias subclases, pero dejando ciertos aspectos sin concretar. Una clase abstracta puede contener atributos, métodos implementados normalmente y también métodos abstractos. Su función principal es marcar una estructura y un contrato que las subclases deben respetar.

Un método abstracto es un método que se declara sin implementación, es decir, solo se especifica su firma pero no su código. La responsabilidad de implementar ese método recae obligatoriamente en las subclases concretas. Si una clase contiene al menos un método abstracto, la clase debe declararse como abstracta. No es posible crear instancias de una clase abstracta, ya que no tiene un comportamiento completamente definido.

En este contexto, Soldado puede redefinirse como una clase abstracta que define qué significa ser un soldado, pero sin concretar cómo ataca cada uno. El método saludar puede seguir implementado en la clase base, mientras que atacar se declara como abstracto. Cada tipo de soldado (Zapador, Artillero, etc.) está obligado a implementar su propia forma de atacar, lo que encaja directamente con el uso del polimorfismo.

La palabra clave abstract debe colocarse delante de la definición de la clase abstracta y delante de cada método abstracto. En las subclases concretas, no se usa abstract si se implementan todos los métodos heredados.

abstract class Soldado {
    void saludar() {
        System.out.println("El soldado saluda de forma general.");
    }

    abstract void atacar();
}

class Zapador extends Soldado {
    @Override
    void atacar() {
        System.out.println("El zapador coloca y detona explosivos.");
    }
}

class Artillero extends Soldado {
    @Override
    void atacar() {
        System.out.println("El artillero dispara la artillería.");
    }
}

En este ejemplo no se pueden crear objetos de tipo Soldado, pero sí utilizar referencias de ese tipo para apuntar a objetos concretos, aprovechando así el polimorfismo.

## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

### Respuesta

La palabra clave final en Java tiene el efecto de impedir la modificación futura de un elemento del programa. Cuando se aplica a un método, indica que ese método no puede ser sobrescrito por las subclases. Cuando se aplica a una clase, significa que dicha clase no puede ser heredada. Su utilización expresa una decisión de diseño: el comportamiento definido se considera completo y no debe alterarse mediante herencia.

La relación entre final y el polimorfismo es directa, ya que final limita o anula su uso. El polimorfismo basado en herencia se apoya en la sobreescritura de métodos y en la posibilidad de tratar objetos de distintas subclases de forma uniforme. Si un método es final, no puede participar en la sobreescritura, por lo que siempre se ejecutará la misma versión, independientemente del tipo concreto del objeto. Del mismo modo, si una clase es final, no puede haber subclases, y por tanto no puede existir polimorfismo de subtipos a partir de ella.

Aun así, el uso de final no es negativo; se emplea cuando se quiere garantizar seguridad, coherencia o eficiencia. Impedir la herencia evita modificaciones no deseadas y facilita ciertas optimizaciones por parte del compilador. En este sentido, final se utiliza para cerrar jerarquías de clases cuando no tiene sentido ampliarlas o cuando hacerlo podría introducir errores.

Un ejemplo muy conocido en la API estándar de Java es la clase String, que está declarada como final. Esto impide que se creen subclases de String, garantizando que su comportamiento sea siempre predecible e inmutable. También existen métodos final en otras clases de la biblioteca estándar cuando se desea evitar su redefinición en subclases.

public final class EjemploFinal {
    public final void metodoNoSobrescribible() {
        System.out.println("Este método no se puede sobrescribir");
    }
}




## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

### Respuesta

En Java, las interfaces son un tipo especial de estructura que permite definir qué métodos debe tener una clase, pero no cómo se implementan. Una interfaz establece un contrato: cualquier clase que la implemente se compromete a proporcionar una implementación concreta de todos sus métodos (salvo los que tengan implementación por defecto). A diferencia de una clase, una interfaz no representa un objeto ni contiene estado propio; su papel es definir capacidades o comportamientos comunes.

Las interfaces se parecen a las clases abstractas, pero no son lo mismo. Ambas se utilizan para definir comportamientos comunes y facilitan el polimorfismo, pero una clase abstracta puede contener atributos, métodos implementados y constructores, mientras que una interfaz se centra casi exclusivamente en la definición de métodos públicos. Tradicionalmente, las interfaces no contenían implementación alguna, aunque desde Java 8 pueden incluir métodos default y static. Conceptualmente, una clase abstracta responde más a una relación “es un”, mientras que una interfaz expresa “puede hacer”.

Una diferencia clave es que una clase puede implementar más de una interfaz, algo que no ocurre con las clases abstractas, ya que Java no permite herencia múltiple de clases. Esto resuelve el problema de necesitar combinar comportamientos provenientes de distintas jerarquías sin introducir ambigüedades. Gracias a las interfaces, una clase puede verse como distintos tipos al mismo tiempo, lo que refuerza el polimorfismo y la flexibilidad del diseño.

interface Atacante {
    void atacar();
}

interface Saludador {
    void saludar();
}

class Soldado implements Atacante, Saludador {
    @Override
    public void atacar() {
        System.out.println("El soldado ataca.");
    }

    @Override
    public void saludar() {
        System.out.println("El soldado saluda.");
    }
}
``

En este ejemplo, Soldado implementa dos interfaces, lo que no sería posible con clases abstractas. Esto permite tratar al objeto como Atacante, como Saludador o como Soldado, según el contexto, utilizando plenamente el polimorfismo.

## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

### Respuesta

Se propone un diseño basado en polimorfismo donde la clase Punto actúa como tipo abstracto común para distintos tipos de puntos, sin fijar de antemano la dimensión del espacio. El método calcularDistanciaA se declara como abstracto, ya que el cálculo de la distancia depende de si se trabaja en dos o en tres dimensiones. De este modo, se obliga a que cada subtipo (Punto2D y Punto3D) proporcione su propia implementación, manteniendo una interfaz común para el uso general.

Para garantizar que la distancia solo se calcule entre puntos compatibles, se utiliza instanceof junto con downcasting. Antes de realizar el cálculo, se comprueba que el punto recibido pertenece al mismo subtipo; en caso contrario, se lanza una excepción. Este enfoque no es el más elegante desde el punto de vista del diseño, pero permite mostrar explícitamente cómo comprobar tipos en tiempo de ejecución, un concepto cercano al uso manual de estructuras y comprobaciones en C, ahora integrado en un contexto orientado a objetos.

Este diseño permite crear la clase Linea, que trabaja únicamente con referencias de tipo Punto, sin conocer si los puntos son 2D o 3D. Gracias al polimorfismo, la línea puede calcular su longitud delegando el cálculo de la distancia en el propio objeto Punto, funcionando correctamente independientemente de la dimensión del espacio. Así, la clase Linea queda desacoplada de los detalles concretos y puede reutilizarse sin modificaciones.

abstract class Punto {
    abstract double calcularDistanciaA(Punto otro);
}

class Punto2D extends Punto {
    double x, y;

    Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto2D)) {
            throw new IllegalArgumentException("Puntos incompatibles");
        }
        Punto2D p = (Punto2D) otro;
        return Math.sqrt(Math.pow(x - p.x, 2) + Math.pow(y - p.y, 2));
    }
}

class Punto3D extends Punto {
    double x, y, z;

    Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto3D)) {
            throw new IllegalArgumentException("Puntos incompatibles");
        }
        Punto3D p = (Punto3D) otro;
        return Math.sqrt(
            Math.pow(x - p.x, 2) +
            Math.pow(y - p.y, 2) +
            Math.pow(z - p.z, 2)
        );
    }
}

class Linea {
    private Punto a;
    private Punto b;

    Linea(Punto a, Punto b) {
        this.a = a;
        this.b = b;
    }

    double longitud() {
        return a.calcularDistanciaA(b);
    }
}

En este ejemplo, Linea utiliza el método polimórfico calcularDistanciaA sin conocer el subtipo concreto de los puntos, demostrando cómo el polimorfismo permite escribir código genérico que funciona correctamente para distintas implementaciones concretas.


## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

### Respuesta

La herencia de interfaces en Java consiste en que una interfaz puede extender a otra interfaz, heredando la declaración de sus métodos. De este modo, una interfaz más especializada puede ampliar las capacidades definidas por otra más general, manteniendo una relación jerárquica similar a la herencia entre clases, pero limitada a la definición de comportamientos. Esta técnica se utiliza para construir contratos progresivamente más completos sin duplicar definiciones.

En Java sí existe herencia múltiple de interfaces. Una interfaz puede extender una o varias interfaces a la vez, separándolas por comas. Esto es posible porque las interfaces no aportan estado ni implementación obligatoria (salvo métodos default), lo que evita los problemas clásicos de la herencia múltiple de clases. Gracias a ello, una clase puede implementar una interfaz que, a su vez, hereda de varias otras, comprometiéndose a cumplir todos los contratos definidos.

En el ejemplo propuesto, se puede definir una interfaz Fichero que represente la capacidad de leer el contenido de un fichero. A partir de ella, se define una interfaz más específica, FicheroEscribible, que extiende a Fichero y añade nuevas operaciones relacionadas con la escritura y eliminación. De este modo, cualquier clase que implemente FicheroEscribible será, automáticamente, también un Fichero.

Este diseño refuerza el polimorfismo, ya que el mismo objeto puede ser tratado como Fichero o como FicheroEscribible según el contexto. Además, permite construir sistemas flexibles en los que las capacidades se combinan mediante interfaces, sin depender de una única jerarquía rígida de clases.

interface Fichero {
    String leerContenido();
}

interface FicheroEscribible extends Fichero {
    void escribirContenido(String contenido);
    void eliminar();
}

class FicheroTexto implements FicheroEscribible {
    private String contenido = "";

    @Override
    public String leerContenido() {
        return contenido;
    }

    @Override
    public void escribirContenido(String contenido) {
        this.contenido = contenido;
    }

    @Override
    public void eliminar() {
        contenido = "";
        System.out.println("Fichero eliminado");
    }
}

En este ejemplo, FicheroEscribible hereda de Fichero, y la clase FicheroTexto implementa la interfaz más específica, quedando obligada a implementar todos los métodos heredados y definidos.