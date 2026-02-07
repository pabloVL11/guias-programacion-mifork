<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Encapsulación". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

### Respuesta
En la Programación Orientada a Objetos, la encapsulación y la ocultación de información buscan proteger el estado interno de los objetos evitando que otras partes del programa accedan o modifiquen directamente sus datos. Este enfoque pretende que cada clase controle cómo se manipulan sus atributos, obligando a que cualquier interacción se realice a través de métodos específicos. De esta manera, se establece una separación clara entre la interfaz pública (lo que se ofrece) y la implementación interna (cómo está hecho), proporcionando un mayor control sobre el uso correcto del objeto.

Además, estos mecanismos persiguen reducir la dependencia entre las distintas partes del programa, de modo que los detalles internos de una clase puedan modificarse sin que ello afecte al resto del sistema. Al evitar el acceso directo a los datos, se minimiza el riesgo de errores provocados por manipulaciones externas no previstas y se garantiza que el objeto mantenga siempre un estado válido. Esta aproximación difiere notablemente del estilo de C, donde los datos suelen estar expuestos y las funciones pueden modificarlos sin restricciones.

Entre las ventajas principales de la ocultación de información pueden enumerarse las siguientes:

Favorece un código más seguro al impedir modificaciones indebidas del estado interno.
Facilita el mantenimiento, ya que permite cambiar la implementación sin alterar el código que usa la clase.
Reduce la complejidad global del sistema al exponer únicamente lo esencial.
Mejora la reutilización y robustez del software, al ofrecer componentes más predecibles y controlados.


## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

### Respuesta
La interfaz pública de un objeto o clase en POO se entiende como el conjunto de métodos y atributos accesibles desde fuera de la propia clase. Representa aquello que un usuario del objeto necesita conocer para interactuar con él, sin necesidad de entender cómo está implementado internamente. En Java, esta interfaz suele estar formada por métodos públicos que permiten realizar operaciones controladas sobre el estado del objeto. Por tanto, la interfaz pública actúa como la “puerta de entrada” al comportamiento ofrecido por la clase.

Su relación con la ocultación de información es directa: la interfaz pública define lo que se puede usar, mientras que la ocultación determina lo que no debe conocerse ni modificarse desde fuera. Al mantener los atributos privados y exponer solo los métodos necesarios, se reduce la posibilidad de uso incorrecto del objeto y se garantiza que todas las interacciones sigan las reglas establecidas por la clase. Esto permite modificar la implementación interna sin afectar a quienes utilicen la clase, siempre que su interfaz pública permanezca estable.

Además, la existencia de una interfaz pública clara contribuye a un diseño más modular y comprensible. Al no depender de detalles internos, otras partes del programa pueden trabajar únicamente con aquello que la clase ofrece oficialmente, lo que simplifica la estructura del software. En comparación con C, donde structs y funciones pueden quedar más expuestos, la POO promueve una organización más segura y orientada a limitar el acceso a lo estrictamente necesario.

## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

### Respuesta
La interfaz pública de una clase debe diseñarse con cuidado porque define cómo otros componentes del programa podrán interactuar con dicha clase. Una vez que la interfaz se hace pública, cualquier cambio puede afectar a todas las partes del sistema que dependen de ella. Por este motivo, conviene exponer únicamente los métodos estrictamente necesarios, manteniendo el resto de la implementación oculta. Un diseño reflexivo evita dependencias innecesarias y favorece que la clase sea más fácil de mantener y utilizar correctamente.

Modificar la interfaz pública no suele ser fácil, especialmente en proyectos medianos o grandes. Un cambio en un método público —como su nombre, parámetros o comportamiento esperado— obliga a revisar y ajustar todas las partes del código que lo utilicen. Esto puede generar errores, incompatibilidades o la necesidad de reestructurar secciones completas del programa. Por el contrario, los cambios internos en los atributos privados o en la lógica interna no afectan a quienes usan la clase, siempre que la interfaz permanezca estable.

En consecuencia, una interfaz pública bien diseñada actúa como un contrato claro y estable entre la clase y el resto del programa. Este contrato debe ser simple, coherente y duradero, lo que facilita la evolución del software sin penalizar su mantenimiento. Diseñar con esta perspectiva ayuda a obtener sistemas más robustos, modulares y resistentes a cambios futuros.

## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

### Respuesta
Las invariantes de clase son condiciones lógicas que deben cumplirse siempre para que un objeto se considere en un estado válido. Se establecen como reglas internas que describen cómo deben relacionarse los atributos de la clase y qué valores son aceptables en cualquier momento del ciclo de vida del objeto, excepto durante la ejecución temporal de un método. Estas invariantes permiten garantizar que la clase funcione de manera coherente y que no existan estados inconsistentes que puedan provocar errores o comportamientos impredecibles.

La ocultación de información ayuda de forma decisiva a mantener estas invariantes, porque evita que código externo pueda modificar los atributos directamente y romper esas reglas internas. Al obligar a que todas las modificaciones se realicen a través de métodos controlados, es posible comprobar y hacer cumplir las invariantes en cada operación. Esto no sería posible si los atributos fueran públicos, ya que cualquier parte del programa podría alterarlos sin ninguna validación.

Además, la ocultación refuerza la robustez del diseño, ya que permite modificar internamente cómo se aplican o verifican las invariantes sin cambiar la interfaz pública. Esto facilita el mantenimiento y reduce la aparición de errores relacionados con estados no válidos. En comparación con el estilo habitual en C, donde los datos suelen estar más expuestos, el modelo orientado a objetos ofrece un entorno mucho más adecuado para garantizar la coherencia y seguridad de los objetos.

## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

public class Punto {
    // Estado interno oculto (encapsulado)
    private double x;
    private double y;

    // Interfaz pública: constructor
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // Interfaz pública: getters (acceso controlado)
    public double getX() {
        return x;
    }

    public double getY() {
        return y;
    }

    // Interfaz pública: comportamiento
    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}

La interfaz pública de la clase Punto está formada por todo aquello accesible desde fuera de la clase: el/los constructores públicos, los métodos públicos getX(), getY() y calcularDistanciaAOrigen() (y, si se habilitaran, los setX() y setY()). Esa interfaz define qué operaciones pueden realizar los usuarios de la clase sin necesidad de conocer cómo se almacenan o validan internamente las coordenadas. El estado (x, y) queda oculto gracias a su visibilidad private, lo que impide modificaciones directas y garantiza que cualquier interacción pase por puntos de control bien definidos.

En cuanto a los modificadores de acceso, public significa que el miembro (clase, constructor o método) es accesible desde cualquier otro código que tenga visibilidad del tipo. Por su parte, private limita el acceso exclusivamente al interior de la propia clase, impidiendo el acceso directo desde otras clases. Este control de visibilidad es la base de la ocultación de información: se resguarda el estado interno y se expone únicamente una interfaz estable, clara y segura para interactuar con el objeto.

### Respuesta


## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### Respuesta

En Java, los modificadores public y private pueden aplicarse a clases, atributos, métodos y constructores, aunque con algunas restricciones. En el nivel superior (clases que no están dentro de otra clase), solo se permite que sean public o tener visibilidad por defecto (package‑private). En cambio, una clase interna sí puede declararse como public, private o protected, dado que forma parte del interior de otra clase y su visibilidad queda sujeta a la encapsulación de ese contexto.

En cuanto a los atributos y métodos, tanto public como private pueden aplicarse sin limitaciones dentro de una clase. Esto permite definir claramente qué elementos forman parte de la interfaz pública y cuáles deben permanecer ocultos como detalles internos. Del mismo modo, los constructores pueden llevar cualquiera de estos modificadores con el fin de controlar cómo y desde dónde se pueden crear instancias de la clase. Esta flexibilidad resulta esencial para implementar patrones de diseño o controlar el ciclo de vida de los objetos.

Por último, los modificadores no se aplican a variables locales dentro de métodos, ya que su ámbito está necesariamente limitado al propio método y no pueden ser accesibles desde fuera. Esta distinción refuerza la idea de que la visibilidad en Java se utiliza para controlar la accesibilidad entre clases y objetos, no dentro del flujo interno de un método.


## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### Respuesta
En Programación Orientada a Objetos existen más tipos de visibilidad además de la pública y la privada, aunque su disponibilidad depende del lenguaje. El objetivo general de disponer de varios niveles de visibilidad es ofrecer un control más preciso sobre quién puede acceder a los miembros de una clase, permitiendo modularidad y encapsulación más fina. Esto resulta especialmente útil cuando se trabaja con jerarquías de clases o con módulos compuestos por múltiples unidades de código.

En Java, además de public y private, existen otros dos niveles de visibilidad: protected y package‑private (la visibilidad “por defecto”, cuando no se indica ningún modificador). El nivel protected permite el acceso desde clases del mismo paquete y también desde subclases, incluso si están en otro paquete. Por su parte, la visibilidad package‑private permite el acceso únicamente dentro del mismo paquete, lo que facilita organizar componentes relacionados que colaboran entre sí sin exponer detalles al exterior. De este modo, Java ofrece cuatro niveles en total: private, package‑private, protected y public.

En otros lenguajes orientados a objetos también existen visibilidades adicionales o variaciones. Por ejemplo, C++ utiliza igualmente private, protected y public, pero su semántica es ligeramente distinta, especialmente en relación con la herencia y las clases amigas (friend). Lenguajes como C# amplían todavía más el conjunto, incluyendo niveles como internal o protected internal, que permiten controlar el acceso dentro del mismo ensamblado o en combinación con la herencia. Esto demuestra que la idea de disponer de distintos niveles de acceso es común en la POO, aunque cada lenguaje la adapta según sus propios objetivos y filosofía.

## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta
Los miembros de instancia privados están ocultos para otras clases, pero no para otras instancias de la misma clase. En Java, la restricción private actúa a nivel de clase, no de objeto: cualquier método definido dentro de la clase puede acceder a los campos privados de cualquier instancia de esa misma clase, incluido el objeto recibido como parámetro. Por tanto, la opción correcta es (a) otras clases; la opción (b) es falsa, porque dos instancias pueden “verse” sus privados a través del código de la clase que comparten.

A modo de ejemplo, puede añadirse a la clase Punto un método calcularDistanciaAPunto(Punto otro) que acceda directamente a otro.x y otro.y, aunque dichos atributos sean private. Esto es válido porque el acceso ocurre desde dentro de la clase Punto, que es precisamente el ámbito que autoriza private.

public class Punto {
    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    // Acceso válido a campos privados de "otro" porque se está dentro de la misma clase
    public double calcularDistanciaAPunto(Punto otro) {
        double dx = this.x - otro.x; // acceso legal a "otro.x"
        double dy = this.y - otro.y; // acceso legal a "otro.y"
        return Math.sqrt(dx * dx + dy * dy);
    }
}

En consecuencia, la encapsulación no se rompe al permitir este acceso, porque sigue sin ser posible que código externo a Punto lea o modifique x e y directamente. La ocultación de información se mantiene: solo los métodos de Punto deciden cómo se manipulan sus datos, tanto los propios como los de cualquier otra instancia de Punto recibida como argumento. Esta propiedad es clave para implementar operaciones coherentes entre objetos sin exponer el estado interno a terceros.


## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta
Los métodos getter y setter son procedimientos utilizados en lenguajes orientados a objetos para permitir un acceso controlado al estado interno de un objeto. Dado que los atributos suelen declararse como privados para proteger su integridad, los getters y setters actúan como una “puerta oficial” a través de la cual se leen o modifican esos valores. Esta forma de acceso evita exponer directamente los datos, manteniendo así la encapsulación como principio central del diseño orientado a objetos.

Un getter (o accesor) es un método que devuelve el valor de un atributo privado. Su función es permitir leer el estado de forma segura, sin otorgar permiso para cambiarlo directamente. Por su parte, un setter (o mutador) permite modificar el valor de un atributo, pero lo hace de forma controlada: puede validar, restringir, transformar o aplicar reglas antes de aceptar la modificación. Esto evita que el objeto adopte estados no válidos o inconsistentes, algo que podría romper las invariantes de la clase.

Además, el uso de getters y setters favorece la mantenibilidad, ya que permite cambiar la implementación interna sin alterar la interfaz pública. Por ejemplo, aunque el atributo deje de almacenarse directamente y pase a calcularse, el getter podría seguir funcionando del mismo modo hacia el exterior. De esta manera, los objetos continúan ofreciendo una interfaz coherente y estable, incluso si su estructura interna evoluciona con el tiempo.


## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta
No, cuando en POO se afirma que la ocultación de información mejora la “seguridad”, no se hace referencia a seguridad frente a hackeos, ataques externos o vulnerabilidades de ciberseguridad. El término “seguridad” se entiende en un sentido interno al diseño del software, y tiene que ver con evitar errores lógicos, inconsistencias y usos indebidos de los objetos por parte de otras partes del propio programa. De esta forma, la ocultación garantiza que el estado de un objeto no pueda ser alterado de manera inapropiada, y que todas las modificaciones pasen por métodos controlados que mantienen las invariantes de la clase.

Este tipo de seguridad implica que el objeto se protege frente a manipulaciones accidentales o incorrectas generadas por desarrolladores u otros módulos del programa. Al restringirse el acceso directo a los datos mediante atributos privados y métodos públicos controlados, se limita la posibilidad de romper las reglas internas que definen un estado válido. Esto es especialmente útil en sistemas grandes, donde las interacciones entre componentes pueden ser complejas y propensas a errores si no existen estos mecanismos de protección.

En cambio, la seguridad frente a hackeos o ataques externos —la denominada seguridad informática o ciberseguridad— requiere medidas completamente distintas, relacionadas con criptografía, control de acceso, validación de entradas externas, políticas de autenticación, etc. La encapsulación no protege frente a este tipo de amenazas externas, ya que su ámbito es puramente estructural y pertenece al diseño del código. Por tanto, la “seguridad” que proporciona la ocultación es organizativa y lógica, no de defensa contra atacantes.


## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta
Un miembro de instancia es aquel que pertenece a cada objeto individual creado a partir de una clase. Cada instancia mantiene su propia copia independiente de esos atributos y métodos no estáticos, de modo que el estado varía entre objetos. En la clase Punto, por ejemplo, las coordenadas x e y son miembros de instancia: cada punto concreto almacena sus propios valores, y los métodos no estáticos operan sobre ese estado particular. Esta característica permite que distintos objetos representen información distinta dentro del mismo programa, aunque provengan de la misma definición de clase.

Un miembro de clase, en cambio, es compartido por todas las instancias de la misma clase. En Java se declaran con la palabra clave static y existen incluso aunque no se cree ningún objeto. Esto incluye tanto atributos como métodos. Por ejemplo, un contador de cuántos objetos se han creado es típicamente un atributo estático. Dado que el valor es común, una modificación afecta a todas las instancias, y su uso responde a necesidades globales más que a características particulares de cada objeto.

Los miembros de clase también pueden ocultarse, utilizando los mismos modificadores de acceso que para los miembros de instancia. Un atributo estático puede declararse private para impedir que código externo lo modifique o lo lea directamente. De este modo, se mantiene la encapsulación incluso para la parte compartida de la clase, permitiendo exponer solo métodos de acceso controlado (por ejemplo, un getter público que devuelva el valor del atributo estático). Esta posibilidad refuerza la idea de que la ocultación de información actúa a nivel de clase, con independencia de que el miembro sea de instancia o estático.


## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta
Sí, tiene sentido que los constructores sean privados, aunque solo en situaciones concretas. Un constructor privado impide que el código externo cree instancias libremente, lo que permite a la clase controlar completamente cuándo y cómo se generan sus objetos. Esto resulta útil cuando la creación de instancias debe seguir reglas estrictas o cuando se desea que solo exista un número limitado de objetos posibles. Un ejemplo típico es el patrón Singleton, donde solo puede existir una única instancia global y el constructor privado evita que se creen más desde fuera de la clase.

También es habitual usar constructores privados en clases que proporcionan únicamente métodos estáticos y no necesitan ser instanciadas, como ocurre con ciertas utilidades matemáticas o bibliotecas auxiliares. En estos casos, marcar el constructor como privado deja claro que la clase no representa objetos y que su uso previsto es puramente funcional. Además, puede emplearse esta técnica para obligar a utilizar métodos de fábrica (factory methods), permitiendo ocultar detalles de creación o devolver subtipos sin que el usuario de la clase lo note.

Por tanto, aunque no es lo más común en clases orientadas a representar entidades, el uso de constructores privados tiene pleno sentido cuando se busca modularidad, control en la creación de objetos o un diseño más claro en los patrones arquitectónicos.

## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta
En Java, los miembros de clase se indican con la palabra clave static. Esto hace que el miembro pertenezca a la clase y no a cada instancia concreta: existe una única copia compartida por todos los objetos (o incluso sin haber creado ninguno). Pueden ser tanto atributos como métodos estáticos, y se les puede aplicar igualmente la ocultación de información (private) y exponer una interfaz controlada mediante métodos estáticos públicos.

A continuación, se amplía la clase Punto para incluir dos atributos estáticos privados que registran los valores máximos globales de x e y vistos hasta el momento. Se actualizan en el constructor (y también en los setters si se activan), y se exponen mediante getters estáticos públicos que forman parte de la interfaz de clase. De este modo, se mantiene la encapsulación y se ofrece una manera coherente de consultar estos agregados globales.

public class Punto {
    // Estado de instancia (oculto)
    private double x;
    private double y;

    // Miembros de clase (compartidos por todas las instancias)
    private static double maxXGlobal = Double.NEGATIVE_INFINITY;
    private static double maxYGlobal = Double.NEGATIVE_INFINITY;

    // Constructor: actualiza máximos globales
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
        actualizarMaximos(x, y);
    }

    // Getters de instancia
    public double getX() { return x; }
    public double getY() { return y; }

    // (Opcional) Setters de instancia: mantienen los máximos si se permite mutación
    public void setX(double x) {
        this.x = x;
        actualizarMaximos(this.x, this.y);
    }

    public void setY(double y) {
        this.y = y;
        actualizarMaximos(this.x, this.y);
    }

    // Comportamiento de instancia
    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    public double calcularDistanciaAPunto(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }

    // Métodos de clase (interfaz pública para consultar agregados globales)
    public static double getMaxXGlobal() { return maxXGlobal; }
    public static double getMaxYGlobal() { return maxYGlobal; }

    // Detalle interno (oculto) para mantener los máximos globales
    private static void actualizarMaximos(double x, double y) {
        if (x > maxXGlobal) maxXGlobal = x;
        if (y > maxYGlobal) maxYGlobal = y;
    }
}

En esta versión, maxXGlobal y maxYGlobal son miembros de clase (static) y están ocultos (private); solo pueden consultarse mediante getMaxXGlobal() y getMaxYGlobal(). De este modo, se separa claramente la interfaz de instancia (operaciones sobre un punto concreto) de la interfaz de clase (consultas agregadas sobre todos los puntos). Si se necesitara concurrencia real, podría considerarse sincronización o tipos atómicos; para un contexto introductorio, esta solución captura adecuadamente la idea de miembros de clase y su encapsulación.


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta
// Método factoría estático: redondea al entero más cercano y crea un Punto
public static Punto crearRedondeando(double x, double y) {
    double rx = (double) Math.round(x); // half-up: 2.5 -> 3, -2.5 -> -2
    double ry = (double) Math.round(y);
    return new Punto(rx, ry);
}

Sí: se usa `static` (es un método de clase/factoría).


## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta
A continuación se muestra una posible reimplementación de Punto que sustituye los dos double por un array interno de longitud 2 (coords[0] = x, coords[1] = y) sin modificar la interfaz pública ya usada hasta ahora (constructor, getters, setters opcionales, métodos de instancia y los miembros de clase para máximos). De este modo, el contrato público se mantiene estable y cualquier código cliente no necesita cambios.

Esta variación ilustra cómo la ocultación de información permite alterar la representación interna sin afectar a los usuarios de la clase. El cambio queda confinado a la implementación privada: métodos públicos siguen funcionando igual, y las invariantes y agregados globales se conservan. Como consideración adicional, se evita exponer el array y se copian valores cuando procede, para mantener la encapsulación.

public class Punto {
    // Representación interna: array de dos posiciones (0 -> x, 1 -> y)
    private final double[] coords = new double[2];

    // Miembros de clase (idénticos a la versión previa)
    private static double maxXGlobal = Double.NEGATIVE_INFINITY;
    private static double maxYGlobal = Double.NEGATIVE_INFINITY;

    // Constructor público (misma firma)
    public Punto(double x, double y) {
        coords[0] = x;
        coords[1] = y;
        actualizarMaximos(coords[0], coords[1]);
    }

    // Getters públicos (misma interfaz)
    public double getX() { return coords[0]; }
    public double getY() { return coords[1]; }

    // (Opcional) Setters públicos (misma interfaz, si se permiten mutaciones)
    public void setX(double x) {
        coords[0] = x;
        actualizarMaximos(coords[0], coords[1]);
    }

    public void setY(double y) {
        coords[1] = y;
        actualizarMaximos(coords[0], coords[1]);
    }

    // Comportamiento público (misma interfaz)
    public double calcularDistanciaAOrigen() {
        return Math.sqrt(coords[0] * coords[0] + coords[1] * coords[1]);
    }

    public double calcularDistanciaAPunto(Punto otro) {
        double dx = this.coords[0] - otro.coords[0]; // acceso válido: misma clase
        double dy = this.coords[1] - otro.coords[1];
        return Math.sqrt(dx * dx + dy * dy);
    }

    // Método factoría ilustrativo (mantenido si ya se ofrecía en la interfaz pública)
    public static Punto crearRedondeando(double x, double y) {
        double rx = (double) Math.round(x);
        double ry = (double) Math.round(y);
        return new Punto(rx, ry);
    }

    // Interfaz de clase para consultar agregados globales (sin cambios)
    public static double getMaxXGlobal() { return maxXGlobal; }
    public static double getMaxYGlobal() { return maxYGlobal; }

    // Detalle interno (oculto)
    private static void actualizarMaximos(double x, double y) {
        if (x > maxXGlobal) maxXGlobal = x;
        if (y > maxYGlobal) maxYGlobal = y;
    }
}
``
Con esta versión se mantiene exactamente el mismo conjunto de miembros públicos: el constructor, getX(), getY(), setX(), setY() (si se desean), calcularDistanciaAOrigen(), calcularDistanciaAPunto(Punto), el método factoría crearRedondeando(...) y los métodos estáticos getMaxXGlobal() y getMaxYGlobal(). La única diferencia es la representación privada del estado, que ahora se apoya en un array, demostrando que la encapsulación permite cambiar el “cómo” sin tocar el “qué” de la interfaz pública.

Como nota de diseño, utilizar un array puede ser útil para extensiones (p. ej., pasar a 3D) o para operar de forma más uniforme sobre coordenadas, aunque se pierde algo de auto-documentación frente a campos con nombre (x, y). Esto se compensa con getters y setters claros, y con constantes o comentarios que expliciten el significado de cada posición del array.


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta
En primer lugar, no es mejor declarar un atributo público solo porque tenga un getter y un setter. Aunque parezca que exponerlos públicamente es equivalente, en realidad no lo es. Declarar el atributo como público significaría renunciar totalmente al control sobre su acceso y modificación, dejando abierta la posibilidad de que cualquier parte del programa pueda alterarlo sin restricciones. En cambio, mantenerlo privado permite interponer lógica dentro de los métodos setter y getter, como validaciones, restricciones, conversiones o comprobaciones de invariantes. El atributo público impediría añadir esta lógica sin romper la compatibilidad con código existente.

La convención más habitual en la mayoría de lenguajes orientados a objetos, incluyendo Java, es declarar todos los atributos como privados y exponer únicamente los métodos necesarios (getters, setters, o ningún método si no se desea permitir acceso externo). Esta práctica forma parte del principio de encapsulación, que busca aislar la representación interna para poder modificarla sin afectar a la interfaz pública. Incluso en clases muy simples, se prefiere usar atributos privados, ya que evita dependencias externas hacia detalles que podrían cambiar en el futuro.

Este tema tiene una relación directa con las invariantes de clase. Una invariante es una condición que debe cumplirse siempre para que el objeto esté en un estado válido. Los setters permiten verificar estas condiciones cada vez que se actualiza un atributo, lo que garantiza la coherencia interna del objeto. Si los atributos fueran públicos, no habría manera de controlar estas modificaciones ni de asegurarse de que se respetan las invariantes, lo que podría llevar a estados inconsistentes o errores lógicos en el programa. Así, la ocultación de información es el mecanismo que permite proteger y salvaguardar esas invariantes.

En conjunto, la práctica recomendada es clara: atributos privados, interfaz pública mínima y controlada, y uso de getters y setters solo cuando realmente se necesitan. Esto no solo mejora la robustez, sino que también permite diseñar objetos coherentes, flexibles y resistentes a cambios futuros.


## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta
Una clase inmutable es aquella cuyo estado no puede modificarse una vez creada una instancia. Esto implica que todos sus atributos representan un valor fijo desde el momento de la construcción y no existen métodos que alteren ese estado interno. Para lograrlo, los atributos suelen declararse private y final, y no se ofrecen setters ni métodos que cambien los valores. La inmutabilidad facilita razonar sobre el comportamiento del objeto, ya que se garantiza que, tras construirse, permanecerá siempre en un estado válido y consistente.

Un método modificador es cualquier método que cambie el estado interno del objeto. Su función es alterar el valor de uno o varios atributos, ya sea directamente o a través de operaciones más complejas. Un método modificador no es necesariamente un setter: un setter es un caso particular y muy simple de método modificador, pero existen muchos otros métodos que también modifican el estado aunque no se llamen setAlgo ni actúen sobre un único atributo. Por ejemplo, un método que traslada un punto sumando un desplazamiento a sus coordenadas, o uno que normaliza los valores de un objeto, serían igualmente métodos modificadores.

La inmutabilidad ofrece varias ventajas importantes. En primer lugar, hace más fácil garantizar las invariantes de clase, ya que ninguna operación posterior puede romperlas: si el constructor crea un objeto válido, ese objeto seguirá siendo válido siempre. Además, las clases inmutables son intrínsecamente más seguras en contextos de concurrencia, porque varios hilos pueden compartirlas sin riesgo de interferencias o condiciones de carrera. También simplifican el diseño y la depuración, al evitar efectos colaterales indeseados provocados por modificaciones del estado.

Otra ventaja destacada es la posibilidad de reutilizar instancias sin temor a que cambien inesperadamente, lo que mejora el rendimiento en ciertos escenarios y permite aplicar técnicas como el caching interno. Sin embargo, la inmutabilidad no siempre es necesaria ni conveniente: en objetos que representan entidades que cambian con el tiempo (como posiciones, contadores o estados de juego), puede ser más práctico permitir modificaciones controladas. En todo caso, conocer la diferencia entre objetos mutables e inmutables resulta clave para diseñar clases robustas, claras y coherentes.

## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta
