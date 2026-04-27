<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Herencia". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones y Composición.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.2. Herencia

## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

### Respuesta

En orientación a objetos, la herencia es un mecanismo que permite definir una clase nueva a partir de otra ya existente, reutilizando su definición. Cuando se dice que “A es‑un B”, se está expresando precisamente esa relación: una instancia de la clase A puede ser considerada también como una instancia de la clase B. Esto no es solo una idea conceptual, sino una regla formal del lenguaje: si una clase Artillero hereda de Soldado, entonces todo artillero es un soldado. Esta relación es distinta de la composición (“tiene‑un”), ya conocida, donde un objeto contiene a otro pero no es intercambiable por él.

La primera implicación importante de la herencia es la compatibilidad de tipos. Gracias a esta regla, cualquier objeto de una subclase puede tratarse como si fuera de la clase base. Esto permite escribir código más general, por ejemplo, trabajar con colecciones de Soldado sin importar el tipo concreto de soldado que haya dentro. El compilador lo acepta porque desde el punto de vista del tipo, todos cumplen el contrato de Soldado. Esta idea es clave para el polimorfismo, que se apoya precisamente en esta compatibilidad.

La segunda implicación es la herencia de estado y comportamiento. Las subclases heredan los atributos y métodos accesibles de la clase base sin necesidad de redefinirlos. En este ejemplo, tanto Artillero como Zapador heredan el atributo nombre (que permanece encapsulado como private) y el método saludar(). Cada subtipo puede además añadir su propio estado y comportamiento específico, como el número de cohetes o de minas, sin afectar al código común del soldado.

A continuación se muestra un ejemplo sencillo en Java que refleja estas ideas y aprovecha la compatibilidad de tipos al recorrer un array de Soldado con objetos de distintos subtipos:

class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Soy el soldado " + nombre);
    }
}

class Artillero extends Soldado {
    private int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre);
        this.cohetes = cohetes;
    }

    public int getCohetes() {
        return cohetes;
    }
}

class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public int getMinas() {
        return minas;
    }
}

public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[3];
        ejercito[0] = new Soldado("Carlos");
        ejercito[1] = new Artillero("Luis", 5);
        ejercito[2] = new Zapador("Ana", 3);

        for (Soldado s : ejercito) {
            s.saludar();
        }
    }
}

Este ejemplo muestra cómo, pese a que los objetos son de tipos distintos, todos pueden almacenarse y utilizarse como Soldado, heredando estado y comportamiento comunes y permitiendo escribir código uniforme y reutilizable.

## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

### Respuesta

Al crear una instancia de una clase concreta que hereda de otra, se ejecutan siempre varios constructores, uno por cada nivel de la jerarquía de herencia. El orden es fijo: primero se construye la clase base y después la subclase, aunque en el código se esté invocando directamente el constructor de la clase hija. Esto garantiza que el objeto quede correctamente inicializado desde lo más general a lo más específico. Por ejemplo, al crear un Artillero, primero se ejecuta el constructor de Soldado y, a continuación, el de Artillero.

La palabra clave super dentro de un constructor sirve para invocar explícitamente un constructor de la clase base. Esta llamada debe ser la primera instrucción del constructor y permite indicar qué constructor del padre se quiere utilizar y con qué parámetros. Conceptualmente, super representa la parte del objeto que corresponde a la clase base. En el ejemplo de los soldados, super(nombre) se usa para inicializar el atributo nombre, que pertenece a Soldado y no a las clases hijas, respetando así la encapsulación.

Si el constructor de la clase base tiene un constructor sin parámetros visible, el compilador inserta automáticamente una llamada implícita a super() si no se escribe nada. En ese caso, no es obligatorio llamar a super de forma explícita. Sin embargo, esta inserción automática solo ocurre si dicho constructor sin parámetros existe y es accesible desde la subclase.

Cuando la clase base no dispone de un constructor sin parámetros visible (por ejemplo, solo tiene constructores con parámetros), es obligatorio llamar explícitamente a super desde cada constructor de la subclase. De lo contrario, el código no compila, porque Java no sabe cómo inicializar la parte heredada del objeto. Esto refuerza la idea de que una subclase no puede ignorar la inicialización necesaria de su clase base y debe cooperar activamente en ese proceso.

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

### Respuesta

Desde el punto de vista de la memoria, los atributos privados de la superclase sí forman parte de la instancia de la subclase. Un objeto en Java contiene toda la información definida en su clase y en todas las clases de las que hereda, independientemente del modificador de acceso de los atributos. Por tanto, cuando se crea un Artillero o un Zapador, la parte correspondiente a Soldado, incluido su atributo privado nombre, se reserva en memoria como parte del mismo objeto.

Ahora bien, que un atributo exista físicamente dentro del objeto no implica que pueda usarse desde cualquier punto del código. El modificador private controla el acceso desde el código, no la existencia en memoria. Esto significa que una subclase no puede acceder directamente a los atributos privados de su superclase, aunque estos estén presentes en la instancia. Esta restricción refuerza la encapsulación: solo la propia clase Soldado conoce y manipula directamente su atributo nombre.

En el ejemplo, el método saludar() puede mostrar el nombre porque está definido dentro de Soldado, que sí tiene acceso al atributo privado. Sin embargo, una subclase como Artillero no puede referirse directamente a nombre, aunque el objeto tenga ese dato internamente. Intentar hacerlo provocaría un error de compilación, no porque el atributo no exista, sino porque no es visible desde la subclase.

class Artillero extends Soldado {
    private int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre);
        this.cohetes = cohetes;
        // System.out.println(nombre); // ERROR: nombre no es accesible
    }
}

Por tanto, los atributos privados de la superclase siempre forman parte del objeto completo en memoria, pero solo pueden usarse a través de métodos públicos o protegidos definidos en la propia superclase. Esta separación entre estructura interna del objeto y visibilidad desde el código es una diferencia clave respecto a lenguajes procedimentales y un pilar fundamental de la orientación a objetos.

## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

### Respuesta

La compatibilidad de tipos entre una superclase y sus subclases tiene un impacto directo en la extensibilidad del código. Permite que el código que trabaja con el tipo base (Soldado) no necesite conocer ni modificarse cuando aparecen nuevos tipos concretos de soldados. En otras palabras, el sistema queda abierto a extensión pero cerrado a modificación: se pueden añadir nuevas clases sin tocar el código ya escrito que opera sobre la abstracción común.

Gracias a esta propiedad, el código que solicita el saludo a todos los soldados solo depende de que los objetos sean compatibles con el tipo Soldado. No importa cuántas subclases existan ni qué comportamiento adicional tengan, mientras hereden de Soldado, podrán utilizarse en los mismos contextos. Esto evita condicionales por tipo (if, instanceof) y reduce el acoplamiento, algo especialmente valioso cuando el sistema crece.

Para ilustrarlo, se añade un nuevo tipo de soldado, por ejemplo un Sanitario, que tiene un número de botiquines. Esta clase hereda de Soldado, reutiliza el atributo nombre y el método saludar(), y añade su propio estado específico. El código que recorre el array de Soldado no se modifica en absoluto, aunque ahora haya un nuevo subtipo.

class Sanitario extends Soldado {
    private int botiquines;

    public Sanitario(String nombre, int botiquines) {
        super(nombre);
        this.botiquines = botiquines;
    }

    public int getBotiquines() {
        return botiquines;
    }
}

public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[4];
        ejercito[0] = new Soldado("Carlos");
        ejercito[1] = new Artillero("Luis", 5);
        ejercito[2] = new Zapador("Ana", 3);
        ejercito[3] = new Sanitario("Marta", 2);

        for (Soldado s : ejercito) {
            s.saludar();
        }
    }
}

Este ejemplo muestra que la extensibilidad se logra añadiendo nuevas clases, no modificando código existente. La compatibilidad de tipos garantiza que todo nuevo subtipo de Soldado encaje automáticamente en los puntos del programa que trabajan con soldados en general, lo que simplifica el mantenimiento y la evolución del sistema.

## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

### Respuesta

En Java es perfectamente posible que una referencia del supertipo apunte a un objeto real de un subtipo. Esto es una consecuencia directa de la compatibilidad de tipos de la herencia: si Artillero hereda de Soldado, una referencia de tipo Soldado puede almacenar un objeto Artillero. Lo importante es distinguir entre el tipo estático de la referencia (el que ve el compilador) y el tipo dinámico u objeto real en memoria (el que existe realmente en ejecución). Esta separación es fundamental para entender el comportamiento del código con herencia.

Cuando se invocan métodos usando una referencia del supertipo, solo pueden llamarse los métodos que estén definidos en el supertipo (o heredados por él), aunque el objeto real sea de una subclase. No es posible invocar directamente métodos públicos exclusivos del subtipo usando la referencia del supertipo, porque el compilador no puede garantizar que dicho método exista para todos los posibles objetos compatibles. Para acceder a comportamiento específico del subtipo, es necesario realizar una conversión explícita de tipos.

El upcasting consiste en tratar un objeto de un subtipo como si fuera del supertipo. Es una conversión implícita y segura, ya que todo objeto del subtipo es, por definición, un objeto del supertipo. En cambio, el downcasting consiste en convertir una referencia del supertipo a una referencia del subtipo. Esta operación es explícita y potencialmente peligrosa, porque solo es válida si el objeto real es realmente de ese subtipo. Para evitar errores en tiempo de ejecución (ClassCastException), se utiliza el operador instanceof, que permite comprobar el tipo real del objeto antes de hacer la conversión.

El siguiente ejemplo muestra un recorrido de un array de Soldado en el que, al detectar que el objeto real es un Artillero, se realiza un downcasting seguro para acceder a su número de cohetes:

for (Soldado s : ejercito) {
    s.saludar(); // Método accesible desde Soldado

    if (s instanceof Artillero) {
        Artillero a = (Artillero) s; // Downcasting
        System.out.println("Tiene " + a.getCohetes() + " cohetes");
    }
}

Este patrón demuestra cómo la herencia permite escribir código general basado en el supertipo y, cuando es necesario, acceder de forma controlada al comportamiento específico de los subtipos, manteniendo la seguridad y claridad del diseño.

## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

### Respuesta

El acceso protegido forma parte de los mecanismos de ocultación de información que ofrece la orientación a objetos. Su objetivo es encontrar un punto intermedio entre private y public: los miembros protegidos no son accesibles desde cualquier clase, pero sí pueden ser utilizados por las subclases. De esta forma, se permite que las clases hijas reutilicen ciertos detalles internos de la superclase sin exponerlos completamente al exterior.

En Java, el acceso protegido se implementa mediante la palabra clave protected. Un atributo o método declarado como protegido es accesible desde la propia clase, desde todas sus subclases (aunque estén en otro paquete) y desde otras clases del mismo paquete. Desde el punto de vista del diseño, protected indica que ese miembro forma parte de la extensión prevista de la clase, aunque no del uso público general. Esto lo diferencia claramente de private, que impide cualquier acceso directo desde las subclases.

Aplicado al ejemplo del Soldado, declarar el atributo nombre como protegido permite que una subclase como Zapador pueda utilizarlo directamente en uno de sus métodos, por ejemplo al colocar bombas, sin necesidad de un método “getter”. El atributo sigue sin ser accesible desde código externo, pero forma parte de la información que una subclase necesita para su comportamiento específico.

class Soldado {
    protected String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Soy el soldado " + nombre);
    }
}

class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public void ponerMina() {
        System.out.println("El zapador " + nombre + " ha puesto una mina");
        minas--;
    }

    public int getMinas() {
        return minas;
    }
}

Este uso de protected muestra cómo la herencia no solo permite reutilizar métodos, sino también compartir información interna de forma controlada, manteniendo la encapsulación y facilitando la extensión coherente de la clase base.

## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### Respuesta

En los lenguajes orientados a objetos no siempre existe una clase base universal para todos los objetos; esto depende del lenguaje y de su diseño. El concepto de una “raíz común” suele introducirse para garantizar que todos los objetos compartan un conjunto mínimo de características, como identidad, comparación o conversión a texto. Sin embargo, algunos lenguajes orientados a objetos optan por no imponer esta jerarquía única o lo hacen solo de manera conceptual y no explícita.

En determinados lenguajes, especialmente los más cercanos al hardware o con modelos híbridos, no existe una clase base única obligatoria. Por ejemplo, en C++ la herencia es completamente explícita: una clase solo hereda de otra si así se declara. No hay una clase raíz automática para todas las clases definidas por el programador. Esto da más libertad, pero también implica que no existe un comportamiento común garantizado para todos los objetos.

En Java sí ocurre que todas las clases heredan, directa o indirectamente, de una clase base común, que es java.lang.Object. Incluso cuando no se indica explícitamente extends Object, el compilador lo añade de forma implícita. Esto significa que cualquier objeto en Java dispone al menos de los métodos definidos en Object, como toString(), equals(), hashCode() o getClass(). Esta decisión facilita escribir código genérico que puede trabajar con cualquier objeto del sistema.

Como consecuencia práctica, cualquier clase como Soldado, Artillero o Zapador es también un Object, y puede utilizarse allí donde se espere un objeto genérico. Esta raíz común refuerza la coherencia del lenguaje y simplifica mecanismos como las colecciones, la reflexión o el manejo uniforme de objetos, a costa de imponer una jerarquía base que no todos los lenguajes orientados a objetos consideran obligatoria.


## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### Respuesta

La herencia múltiple es un mecanismo mediante el cual una clase puede heredar directamente de más de una clase base. Esto implicaría que la subclase adquiere estado y comportamiento de varios padres al mismo tiempo. Aunque este enfoque puede parecer potente, introduce problemas conceptuales y técnicos, como el conocido problema del diamante, donde surge ambigüedad sobre de qué clase base procede un atributo o método concreto cuando ambas lo definen.

No todos los lenguajes orientados a objetos permiten herencia múltiple de clases. Algunos, como C++, la admiten con reglas adicionales para resolver conflictos, mientras que otros la prohíben deliberadamente para mantener el modelo más simple y seguro. La experiencia ha demostrado que la herencia múltiple de implementación puede complicar el diseño, el mantenimiento y la comprensión del código, especialmente en jerarquías grandes.

En Java no existe herencia múltiple de clases. Una clase solo puede extender (extends) una única clase base. Esta restricción es una decisión de diseño del lenguaje para evitar ambigüedades y simplificar el modelo de herencia. Por tanto, una clase como Soldado solo podría tener una superclase directa, y cualquier subtipo (Artillero, Zapador) seguiría una jerarquía lineal de herencia.

No obstante, Java ofrece una alternativa controlada mediante las interfaces, que permiten una forma de herencia múltiple de comportamiento (pero no de estado). Una clase puede implementar múltiples interfaces, comprometiéndose a proporcionar ciertos métodos sin heredar atributos. De este modo, Java combina un modelo de herencia simple de clases con múltiples contratos de comportamiento, logrando un equilibrio entre flexibilidad y seguridad en el diseño orientado a objetos.

## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

### Respuesta

En los lenguajes orientados a objetos, las excepciones son también objetos, lo que permite aplicar sobre ellas los mismos principios que al resto de clases: encapsulación, herencia y composición. Crear excepciones personalizadas resulta útil para expresar errores del dominio del problema con mayor claridad que usando excepciones genéricas. En Java, una excepción personalizada se define creando una nueva clase que herede de Exception o de RuntimeException.

Cuando una excepción hereda de RuntimeException, se considera una excepción no controlada (unchecked exception). Esto significa que el compilador no obliga a capturarla ni a declararla en la cláusula throws. Este tipo de excepciones se utiliza normalmente para errores de programación o situaciones anómalas que no se espera que el llamador maneje de forma inmediata, como puede ser que un usuario solicitado no exista.

Además, una excepción puede estar compuesta con otros objetos, almacenando información adicional sobre el error. En este caso, incluir un objeto Usuario dentro de la excepción permite conocer qué usuario concreto provocó el problema. Java también permite encadenar excepciones mediante una causa subyacente (cause), lo que resulta muy útil para no perder información cuando una excepción es consecuencia de otra.

A continuación se muestra un ejemplo de excepción personalizada no controlada (UsuarioNoEncontradoException), compuesta con un Usuario y con constructores sobrecargados que permiten incluir o no la causa original del error:

class Usuario {
    private String nombre;

    public Usuario(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}

class UsuarioNoEncontradoException extends RuntimeException {
    private Usuario usuario;

    public UsuarioNoEncontradoException(Usuario usuario) {
        super("Usuario no encontrado: " + usuario.getNombre());
        this.usuario = usuario;
    }

    public UsuarioNoEncontradoException(Usuario usuario, Throwable cause) {
        super("Usuario no encontrado: " + usuario.getNombre(), cause);
        this.usuario = usuario;
    }

    public Usuario getUsuario() {
        return usuario;
    }
}

Este diseño permite lanzar la excepción con información rica sobre el error, acceder al usuario problemático si es necesario y, al mismo tiempo, encadenar excepciones para facilitar el diagnóstico y la depuración del sistema.


## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

### Respuesta

La herencia no debe usarse únicamente como un mecanismo de reutilización de código porque introduce una relación semántica fuerte entre clases: la relación “es‑un”. Cuando se hereda, se está afirmando que la subclase es un tipo específico de la superclase y que puede sustituirla en cualquier contexto. Si esta relación no es conceptualmente correcta y se usa solo para “aprovechar código ya hecho”, el diseño queda forzado y puede resultar confuso o incorrecto desde el punto de vista del dominio del problema.

Uno de los principales problemas de usar herencia solo para reutilizar código es el acoplamiento fuerte que genera. La subclase queda ligada a la implementación interna de la superclase, incluso a detalles que no necesita. Cualquier cambio en la clase base puede afectar a todas las subclases, aunque esas subclases no tengan una relación conceptual sólida con ella. Esto reduce la flexibilidad del sistema y dificulta su mantenimiento y evolución.

La composición, en cambio, permite reutilizar código sin establecer una relación de tipos. En lugar de “ser” algo, una clase “tiene” otra y delega en ella parte de su comportamiento. Este enfoque favorece un diseño más modular, donde los cambios quedan más localizados y los componentes pueden sustituirse con más facilidad. Además, la composición encaja mejor con la encapsulación, ya que no expone automáticamente la interfaz completa del objeto reutilizado.

Por este motivo, se suele recomendar favorecer la composición frente a la herencia cuando el objetivo principal es reutilizar funcionalidad. La herencia debe reservarse para situaciones en las que exista una relación clara “es‑un” y en las que la compatibilidad de tipos y el comportamiento heredado tengan sentido en el modelo conceptual. De lo contrario, la reutilización mediante composición suele producir diseños más robustos, flexibles y fáciles de mantener.



## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

### Respuesta

Se dice que se debe favorecer la composición frente a la herencia porque la composición suele producir diseños más flexibles, menos acoplados y más fáciles de mantener. La herencia crea una relación muy fuerte entre clases, ya que la subclase queda vinculada de forma permanente a la superclase y a su implementación. Cualquier cambio en la clase base puede tener efectos colaterales en todas sus subclases, incluso en aquellas que solo reutilizaban una pequeña parte de su funcionalidad.

La composición, en cambio, permite reutilizar comportamiento sin imponer una relación de tipos. Una clase puede contener a otra y delegar en ella ciertas responsabilidades, pero sin afirmar que “es‑un” caso particular de esa otra clase. Esto facilita cambiar la implementación interna, sustituir componentes o combinar comportamientos distintos sin afectar al resto del sistema. Desde el punto de vista del diseño, la composición reduce el impacto de los cambios y mejora la capacidad de evolución del código.

Otro motivo importante es que la herencia tiende a ser estática, ya que la relación entre clases se define en tiempo de compilación y no puede cambiarse en ejecución. La composición permite una mayor variabilidad dinámica, ya que los objetos que se componen pueden decidirse en tiempo de ejecución. Esto resulta especialmente valioso cuando el comportamiento puede cambiar o configurarse según el contexto.

Por estas razones, la herencia se considera una herramienta potente pero que debe usarse con cuidado, solo cuando exista una relación conceptual clara y estable. La recomendación de favorecer la composición no implica evitar la herencia, sino reservarla para los casos en que realmente modele correctamente el dominio, utilizando la composición como primera opción para compartir funcionalidad de forma segura y flexible.

## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### Respuesta

Cuando se dice que la herencia rompe la encapsulación, se hace referencia a que una subclase puede acabar dependiendo de detalles internos de la superclase, incluso de aquellos que no formaban parte de su interfaz pública original. La encapsulación persigue ocultar la implementación interna de una clase y exponer solo lo necesario a través de una interfaz estable. Sin embargo, la herencia introduce una relación estrecha en la que la subclase conoce y utiliza información interna del padre, especialmente cuando se emplean miembros protected.

Esta ruptura no significa que la encapsulación desaparezca por completo, sino que se debilita. Una subclase no solo depende de qué hace la superclase (su comportamiento observable), sino también de cómo lo hace. Si la superclase cambia su implementación interna —por ejemplo, modifica la forma de mantener su estado o la lógica de ciertos métodos—, la subclase puede verse afectada aunque la interfaz pública no haya cambiado. Esto contradice el objetivo de la encapsulación, que busca minimizar el impacto de los cambios internos.

El problema se agrava cuando las subclases utilizan atributos o métodos protected de la superclase. Aunque estos elementos no sean públicos, pasan a formar parte de un contrato implícito entre la superclase y sus hijas. Cualquier modificación en esos miembros protegidos puede obligar a revisar todas las subclases, lo que aumenta el acoplamiento y dificulta el mantenimiento del sistema a largo plazo.

Por este motivo se afirma que la herencia tiende a romper la encapsulación, en comparación con la composición. La composición permite interactuar con otro objeto exclusivamente a través de su interfaz pública, manteniendo intacto el principio de ocultación de información. En cambio, la herencia expone internamente la superclase a sus subclases, lo que hace que el diseño sea más frágil frente a cambios y requiera un uso cuidadoso y justificado.


## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### Respuesta

Se puede modelar una misma realidad de distintas formas en orientación a objetos, y la elección entre herencia o composición tiene implicaciones importantes en el diseño. En este caso, Estudiante y Trabajador comparten datos comunes (DNI y nombre), lo que permite ilustrar claramente ambas alternativas. Cada enfoque es válido, pero expresa relaciones conceptuales distintas y ofrece diferentes niveles de flexibilidad.

En el modelo basado en herencia, se introduce una superclase Persona que contiene los datos comunes. Tanto Estudiante como Trabajador heredan de ella, estableciendo una relación clara de tipo “es‑una”: un estudiante es una persona y un trabajador es una persona. Esta solución es sencilla y directa cuando la jerarquía es estable y tiene sentido desde el punto de vista del dominio. Los atributos comunes se centralizan en la superclase y se reutilizan automáticamente.

class Persona {
    protected String dni;
    protected String nombre;

    public Persona(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }
}

class Estudiante extends Persona {
    public Estudiante(String dni, String nombre) {
        super(dni, nombre);
    }
}

class Trabajador extends Persona {
    public Trabajador(String dni, String nombre) {
        super(dni, nombre);
    }
}

En el modelo basado en composición, no se introduce una relación de herencia entre Estudiante y Trabajador. En su lugar, ambos tienen unos DatosPersonales, que se representan mediante una clase independiente. Esto evita afirmar que ambos sean un tipo concreto de una clase base común y permite reutilizar los mismos datos personales en otros contextos si fuese necesario. Además, reduce el acoplamiento y mantiene una encapsulación más estricta.

class DatosPersonales {
    private String dni;
    private String nombre;

    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getDni() {
        return dni;
    }

    public String getNombre() {
        return nombre;
    }
}

class Estudiante {
    private DatosPersonales datos;

    public Estudiante(DatosPersonales datos) {
        this.datos = datos;
    }
}

class Trabajador {
    private DatosPersonales datos;

    public Trabajador(DatosPersonales datos) {
        this.datos = datos;
    }
}

Este ejemplo pone de manifiesto que la herencia expresa una relación conceptual fuerte, mientras que la composición favorece la reutilización sin imponer jerarquías rígidas. Por ello, la elección entre ambos enfoques debe basarse en el significado del dominio y en la evolución esperada del sistema, no únicamente en la eliminación de código duplicado.
