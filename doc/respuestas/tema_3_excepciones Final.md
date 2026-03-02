<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Excepciones". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

### Respuesta
A falta de excepciones en C, lo más directo es separar el valor de salida del estado de error. Se puede diseñar raiz para que devuelva un código de estado (0 = OK, ≠0 = error) y escriba el resultado mediante parámetro de salida (puntero). Así, la función nunca “silencia” el error, y es el llamador quien decide cómo informar al usuario (por ejemplo, imprimiendo un mensaje). Este patrón es muy común en C porque evita ambigüedades con valores especiales y obliga a comprobar el retorno.

#include <stdio.h>
#include <math.h>

int raiz(double x, double *out_result) {
    if (x < 0.0) {
        return 1; // error: dominio inválido
    }
    *out_result = sqrt(x);
    return 0; // OK
}

int main(void) {
    double r;
    if (raiz(-9.0, &r) != 0) {
        // Se informa desde fuera de la función
        fprintf(stderr, "Error: raíz de número negativo.\n");
        return 1;
    }
    printf("Resultado: %f\n", r);
    return 0;
}

Otra alternativa idiomática en C es usar la variable global errno y establecer un código de error estándar (por ejemplo, EDOM para error de dominio matemático), dejando que el llamador lo consulte y actúe en consecuencia. Este enfoque encaja con funciones de la biblioteca como sqrt, que pueden indicar errores mediante errno. Se recomienda limpiar errno antes de la llamada y comprobarlo después, para evitar falsas detecciones.

#include <stdio.h>
#include <math.h>
#include <errno.h>

double raiz(double x) {
    if (x < 0.0) {
        errno = EDOM; // dominio inválido
        return 0.0;   // valor ignorado si hay error
    }
    errno = 0;
    double r = sqrt(x);
    return r;
}

int main(void) {
    errno = 0; // limpiar antes de usar
    double r = raiz(-9.0);
    if (errno == EDOM) {
        // Se informa desde fuera de la función
        fprintf(stderr, "Error: argumento fuera de dominio (negativo).\n");
        return 1;
    }
    printf("Resultado: %f\n", r);
    return 0;
}
``
El primer diseño (código de estado + parámetro de salida) evita depender de un global y obliga a comprobar errores; además, permite distinguir claramente entre “no hay resultado” y “hay resultado”, sin recurrir a sentinelas. El diseño con errno es cómodo y consistente con funciones de la libc, pero exige disciplina (limpiar y comprobar), y no es thread-local en C antiguo (sí en implementaciones modernas); además, si el valor de retorno pudiera confundirse con uno válido, conviene no confiar en él para detectar fallos, sino en errno.

## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

### Respuesta

Una excepción es un mecanismo que permite señalar que, durante la ejecución de una función, ha ocurrido una situación anómala o inesperada que impide continuar con el flujo normal del programa. A diferencia de los códigos de error tradicionales en C, una excepción permite interrumpir la ejecución del bloque actual y trasladar el control a un punto específico donde el error pueda ser tratado. Esto introduce una separación clara entre la lógica normal y la lógica de manejo de errores, lo que facilita la lectura y el mantenimiento del código.

Al implementar funciones, un programador utiliza excepciones para notificar fallos de forma estructurada, sin necesidad de verificar continuamente valores especiales o estados retornados por otras funciones. Esto permite que el código que realiza la tarea principal permanezca limpio, mientras que el manejo de errores queda relegado a bloques try/catch independientes. Gracias a ello, las funciones pueden concentrarse en su propósito principal y delegar la gestión de errores en quien las llame.

Al llamar funciones, el programador emplea las excepciones para capturar y gestionar problemas que no puede o no debe resolver la función interna. Esto ofrece control sobre cómo reaccionar ante situaciones anómalas, como validar datos introducidos por el usuario, registrar el error, reintentar una operación o finalizar la ejecución con un mensaje adecuado. En conjunto, las excepciones facilitan un modelo de error más robusto y menos propenso a pasar por alto condiciones problemáticas.


## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

### Respuesta

En Java resulta natural expresar el “fallo de dominio” (número negativo) mediante excepciones. El método raiz puede validar el argumento y, si es negativo, lanzar una IllegalArgumentException con un mensaje descriptivo. De este modo, se separa la lógica normal (calcular la raíz con Math.sqrt) del manejo de errores, que quedará fuera del método. Además, se evita devolver valores sentinela (como 0.0) que podrían confundirse con resultados válidos.

Desde el exterior (por ejemplo, en main), se puede controlar la situación anómala mediante un bloque try/catch. Si la llamada a Calculadora.raiz lanza la excepción, el catch la captura y decide qué hacer: informar al usuario, registrar el error, o recuperar la ejecución. Esto replica la idea planteada en C (informar “desde fuera”), pero con la sintaxis y semántica propias de las excepciones en Java.

public class Calculadora {

    // Método "puro": valida y lanza excepción si el argumento no es válido
    public static double raiz(double x) {
        if (x < 0.0) {
            // Excepción de argumento ilegal (dominio negativo)
            throw new IllegalArgumentException("La raíz requiere un número no negativo.");
        }
        return Math.sqrt(x);
    }

    public static void main(String[] args) {
        // Ejemplo 1: caso correcto
        try {
            double r1 = Calculadora.raiz(9.0);
            System.out.println("Resultado (9.0): " + r1);
        } catch (IllegalArgumentException ex) {
            // Control “desde fuera”: informar al usuario
            System.err.println("Error: " + ex.getMessage());
        }

        // Ejemplo 2: caso que provoca la excepción
        try {
            double r2 = Calculadora.raiz(-4.0); // lanzará IllegalArgumentException
            System.out.println("Resultado (-4.0): " + r2); // no se ejecuta
        } catch (IllegalArgumentException ex) {
            // Control “desde fuera”: decidir qué hacer con el error
            System.err.println("No se pudo calcular la raíz: " + ex.getMessage());
            // Aquí podría registrarse el error, pedir otro dato, etc.
        }
    }
}

Se usa IllegalArgumentException por ser una excepción no verificada (unchecked) adecuada cuando un parámetro viola un precondición. Alternativamente, podría definirse una excepción propia (por ejemplo, DominioMatematicoInvalidoException) si se desea una jerarquía de errores más expresiva. En cualquier caso, el patrón es el mismo: el método lanza al detectar un uso inválido y el código cliente captura para informar o decidir la recuperación.


## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

### Respuesta

“Lanzar” una excepción (en Java, con throw) significa señalar que ha ocurrido una condición anómala que impide continuar con el flujo normal. La función que detecta la situación inválida no devuelve un valor de forma convencional; en su lugar, interrumpe su ejecución y crea/lanza un objeto-excepción que describe el problema. En el ejemplo de la raíz, Calculadora.raiz(x) lanza IllegalArgumentException si x < 0, porque el argumento viola la precondición del dominio.

“Controlar” o “capturar” una excepción consiste en envolver la llamada potencialmente problemática en un bloque try y proporcionar uno o varios catch compatibles con el tipo de excepción lanzada. Si la excepción se captura, se ejecuta el bloque catch, donde se decide cómo reaccionar (informar, reintentar, transformar la excepción, etc.). Si no se captura en ese nivel, la excepción se propaga hacia el llamador: el control “salta” hacia arriba en la pila de llamadas buscando el primer try/catch que sepa manejar ese tipo. Este proceso se denomina stack unwinding: cada función intermedia se aborta (no continúa) y se ejecutan automáticamente sus finalizadores/finally y liberaciones que corresponda.

Cuando una excepción se propaga, las funciones por las que pasa no se reanudan en el punto donde se lanzó ni después; su ejecución termina abruptamente. Sólo el bloque catch (en el primer nivel que la captura) continúa, y desde ahí el programa sigue después del try/catch (o ejecuta un finally si lo hay). Si nadie la captura hasta llegar a main, el runtime terminará el programa mostrando el rastro de pila. Este comportamiento contrasta con C procedural, donde se suelen encadenar códigos de error y cada función debe comprobar y devolver explícitamente estados.

public class Calculadora {
    public static double raiz(double x) {
        if (x < 0) {
            throw new IllegalArgumentException("La raíz requiere un número no negativo.");
        }
        return Math.sqrt(x);
    }

    static double b(double x) {         // no controla aquí
        // Si x<0, la excepción saldrá de b y seguirá subiendo
        return raiz(x);
    }

    static double a(double x) {         // podría controlar o dejar propagar
        return b(x);
    }

    public static void main(String[] args) {
        try {
            double r = a(-4.0);          // aquí se lanzará desde raiz -> b -> a -> main
            System.out.println("Resultado: " + r); // no se ejecuta
        } catch (IllegalArgumentException ex) {
            // Primer punto que captura: se informa "desde fuera"
            System.err.println("No se pudo calcular la raíz: " + ex.getMessage());
        } finally {
            // Este bloque sí se ejecuta, haya o no excepción
            System.out.println("Fin del intento.");
        }
        // La ejecución continúa aquí tras el catch/finally
    }
}

En el flujo anterior, raiz(-4) lanza; b y a no capturan, por lo que la excepción se propaga y ambas funciones terminan sin reanudarse; main la captura e informa al usuario. Tras el catch (y finally), el programa prosigue con normalidad en el siguiente punto, manteniendo separadas la lógica “normal” y la de gestión de errores.

## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

### Respuesta

La propagación natural de excepciones en Java evita la necesidad de que cada función compruebe manualmente códigos de retorno, algo muy habitual en C. En un programa en C, cada función que llame a otra debe verificar explícitamente si hubo error y, en tal caso, devolver su propio código de error al siguiente nivel. Esto conduce fácilmente a código repetitivo, difícil de leer y donde un único olvido de comprobación puede ocultar fallos importantes. Con las excepciones, en cambio, el error viaja automáticamente hacia arriba por la pila hasta encontrar un catch adecuado, sin necesidad de comprobaciones intermedias.

Otra ventaja clara es que el flujo “normal” del programa queda más limpio. En C, la lógica principal suele quedar entremezclada con condiciones del tipo if (error) { ... }, lo que dispersa la atención y obliga a repartir la gestión de errores por múltiples puntos. Con las excepciones, cada función se centra en realizar su tarea y sólo en el punto donde realmente interesa se decide cómo actuar ante fallos. Esto mejora la claridad y reduce la duplicación de código, especialmente en programas con varias capas de llamadas.

Además, la propagación automática garantiza que los errores no se silencian accidentalmente, algo común cuando en C un programador olvida comprobar un return o cuando un valor especial se confunde con uno válido. En Java, si nadie captura la excepción, el programa termina mostrando un rastro de pila, lo que facilita identificar el origen del problema. Este comportamiento predecible minimiza la probabilidad de que el sistema continúe en un estado inconsistente, ventaja significativa respecto al enfoque manual de C.

Finalmente, al estructurar el manejo de errores fuera del camino principal, la propagación de excepciones permite diseñar funciones y módulos que cooperan mejor. Cada nivel de la aplicación puede decidir si atrapa y gestiona el error o si lo deja subir a un nivel superior mejor preparado. En contraste, en C las decisiones de propagación deben implementarse manualmente en cada paso, lo que aumenta la complejidad y la probabilidad de errores. De este modo, la gestión de fallos se vuelve más robusta, legible y coherente en aplicaciones medianas o grandes.

## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

### Respuesta

En la mayoría de lenguajes orientados a objetos, incluidas las versiones modernas de Java, las excepciones sí suelen ser objetos. Al ser instancias de clases que heredan de Throwable, pueden transportar información estructurada sobre el error: un mensaje, un tipo específico, la causa interna o incluso datos adicionales. Esto las integra de forma natural en el modelo de objetos, permitiendo que se manipulen, se pasen como parámetros o se analicen mediante su tipo exacto, igual que cualquier otro objeto del lenguaje.

Desde el punto de vista de la encapsulación, que es uno de los principios fundamentales que ya conoces, las excepciones-objeto permiten ocultar detalles internos del fallo detrás de una interfaz clara y coherente. En lugar de exponer códigos numéricos o estados ambiguos, cada tipo de excepción encapsula su propio significado y ofrece métodos para obtener información relevante. Además, esto ayuda a que distintas partes del programa reaccionen de forma diferente según el tipo de la excepción, sin necesidad de inspeccionar manualmente valores externos. Esta separación limpia mejora la mantenibilidad y coherencia del código.

El hecho de que las excepciones sean clases implica que se pueden crear excepciones personalizadas. Basta con definir una clase que extienda de Exception o RuntimeException, añadiendo constructores o campos según las necesidades. Esto resulta útil cuando se quiere representar errores muy específicos del dominio de la aplicación, como SaldoInsuficienteException en un banco, o DominioMatematicoInvalidoException para casos como la raíz de un número negativo. Estos tipos propios permiten que el código que captura la excepción distinga claramente entre errores distintos y actúe de forma más precisa.
Ejemplo simplificado de una excepción personalizada relacionada con el caso de la raíz:

class DominioMatematicoInvalidoException extends RuntimeException {
    public DominioMatematicoInvalidoException(String msg) {
        super(msg);
    }
}

public static double raiz(double x) {
    if (x < 0) {
        throw new DominioMatematicoInvalidoException(
            "La raíz requiere un número no negativo."
        );
    }
    return Math.sqrt(x);
}

Este enfoque encapsula el error en un objeto con identidad propia, facilita su detección mediante catch (DominioMatematicoInvalidoException ex) y refuerza la claridad del programa, todo ello siguiendo los principios de orientación a objetos.

## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### Respuesta

En Java, cualquier objeto excepción transporta siempre una traza de pila (stack trace) completa, es decir, la secuencia exacta de métodos que estaban en ejecución cuando se produjo el fallo. Esta información incluye el nombre de cada función, la clase a la que pertenece y, normalmente, la línea de código donde ocurrió el lanzamiento. Gracias a ello, cuando la excepción llega a un manejador, se dispone de un contexto muy preciso que permite reconstruir el camino recorrido por el programa hasta el error, algo imposible de obtener de forma automática en C usando códigos de error tradicionales.

Además de la traza de pila, cada excepción encapsula al menos un mensaje descriptivo que explica la causa del fallo en términos legibles por el programador. También puede incluir una causa interna (otra excepción que originó el error), lo que permite encadenar fallos y entender mejor problemas más complejos. Esto facilita muchísimo el diagnóstico, porque el manejador recibe no sólo un aviso de que algo fue mal, sino también una descripción rica del problema y su contexto.

Comparado con C, donde una función suele devolver únicamente un código numérico o un valor especial, la excepción objeto en Java lleva consigo información detallada, coherente y estructurada. Esto evita depender de comentarios externos, documentación o convenciones frágiles para interpretar un error. En cambio, el estado del fallo está correctamente encapsulado dentro del propio objeto, siguiendo los principios de la orientación a objetos y dejando muy claro qué ocurrió, dónde ocurrió y por qué.

En resumen, lo esencial que aporta una excepción—y que resulta extremadamente útil al llegar al manejador—es: la traza de pila, el tipo concreto de excepción, el mensaje explicativo y, opcionalmente, la causa encapsulada. Este conjunto de información proporciona un nivel de diagnóstico muy superior al de los mecanismos clásicos de error en C.

## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Respuesta

Sí, en Java se pueden colocar varios bloques catch a continuación de un mismo try. Esta posibilidad permite manejar de forma diferenciada distintos tipos de excepciones, ya que cada catch indica el tipo concreto que sabe gestionar. Dado que las excepciones son objetos con jerarquía de clases, Java selecciona el primer catch cuyo tipo es compatible con el objeto lanzado, siguiendo el orden en que aparecen en el código. Por ello, es importante ordenar los catch desde los tipos más específicos a los más generales.

Respecto a cuántos bloques catch se ejecutan, solo se ejecuta uno, exactamente el primero cuyo tipo coincide con la excepción lanzada. Una vez encontrado y ejecutado ese bloque, los demás catch se ignoran, y el flujo continúa después de todo el try-catch (o pasa por un finally si existe). Esta regla asegura que el manejo de errores sea claro y no ambiguo, evitando que múltiples bloques intenten gestionar la misma excepción.

Este comportamiento contrasta con estructuras como los if-else en los que puede evaluarse más de una condición. En el caso de las excepciones, Java garantiza que solo un manejador intervendrá, lo que simplifica la lógica y mejora la legibilidad o separación de responsabilidades. De este modo, cada tipo de error tiene su propio tratamiento y la decisión sobre cuál aplicar queda completamente determinada por el tipo del objeto excepción.

## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Respuesta

En Java, para garantizar la liberación de recursos (cerrar ficheros, soltar conexiones, desbloquear mutex, etc.) incluso cuando se lanza una excepción, se utiliza el bloque finally. El código dentro de finally siempre se ejecuta tras el try (y después del catch si existe), tanto si hubo excepción como si no, salvo en casos extremos como un System.exit() o fallo de la JVM. Esto asegura que el recurso se libere antes de que la excepción siga propagándose por la pila o antes de retornar normalmente.

Se puede usar finally sin catch cuando no se desea manejar la excepción en ese nivel, pero sí garantizar la limpieza. El patrón resulta útil cuando el cierre del recurso es obligatorio, pero la decisión de qué hacer ante el error se delega a capas superiores.

import java.io.*;

public class EjemploFinallySinCatch {
    public static void main(String[] args) throws IOException { // se deja propagar
        BufferedReader br = null;
        try {
            br = new BufferedReader(new FileReader("datos.txt"));
            String linea = br.readLine();           // puede lanzar IOException
            System.out.println("Primera línea: " + linea);
        } finally {
            // Se ejecuta siempre: con o sin excepción
            if (br != null) {
                try { br.close(); } catch (IOException e) {
                    // Opcional: registro del fallo al cerrar
                    System.err.println("Error al cerrar: " + e.getMessage());
                }
            }
        }
        // Si hubo excepción, no se llega aquí: se propagó después del finally
    }
}

Si se desea capturar la excepción en el mismo nivel para informar o transformar el error, se añade un catch antes del finally. El finally se ejecutará igualmente, garantizando el cierre del recurso sin duplicar código de limpieza en cada rama del manejo.

import java.io.*;

public class EjemploTryCatchFinally {
    public static void main(String[] args) {
        BufferedReader br = null;
        try {
            br = new BufferedReader(new FileReader("datos.txt"));
            String linea = br.readLine();
            System.out.println("Primera línea: " + linea);
        } catch (IOException ex) {
            // Manejo local: informar, registrar, decidir recuperación
            System.err.println("No se pudo leer el fichero: " + ex.getMessage());
            // Aquí podría reintentarse, devolver un valor por defecto, etc.
        } finally {
            // Se ejecuta siempre, haya o no excepción
            if (br != null) {
                try { br.close(); } catch (IOException e) {
                    System.err.println("Error al cerrar: " + e.getMessage());
                }
            }
        }
        // La ejecución continúa aquí tras catch/finally
    }
}

En Java moderno, para recursos que implementan AutoCloseable, suele preferirse try-with-resources porque cierra automáticamente y es menos propenso a errores. Sin embargo, el objetivo aquí es mostrar explícitamente el papel de finally tanto sin catch (dejando propagar) como con catch (manejando localmente) y garantizando siempre la liberación de recursos.

## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Respuesta

Sí, en Java un bloque finally puede ir sin catch. Es legal escribir try { ... } finally { ... } cuando no se quiere manejar la excepción en ese nivel, pero sí se desea garantizar que cierto código (normalmente de limpieza de recursos) se ejecute siempre. En este caso, si dentro del try ocurre una excepción, la ejecución salta igualmente al finally, y después de ejecutarlo la excepción se sigue propagando hacia el llamador.

El bloque finally se ejecuta siempre, tanto si ocurre una excepción como si no, porque su finalidad es asegurar acciones que deben cumplirse independientemente del flujo. Esto resulta especialmente útil para liberar recursos (ficheros, sockets, memoria nativa…) o dejar el sistema en un estado coherente incluso ante errores. Por ello, el programador puede confiar en que el código del finally se ejecutará incluso en situaciones de ruptura del flujo habitual.

Incluso si aparece un return dentro del try, el finally sigue ejecutándose antes de completar el retorno. Es decir, aunque return intente terminar la función, Java obliga a ejecutar el finally en primer lugar. Esto garantiza que las acciones de limpieza no puedan saltarse por accidente. Sin embargo, si dentro del finally aparece otro return, este anula el retorno del try (aunque no es recomendable hacerlo porque confunde el flujo).

Ejemplo sin catch:

static int ejemploSinCatch() {
    try {
        return 10;        // Antes de devolver 10...
    } finally {
        System.out.println("Ejecutando finally (sin catch).");
        // ...se ejecuta este código.
    }
}

Ejemplo con catch:

static int ejemploConCatch() {
    try {
        int x = 5 / 0;    // Lanza ArithmeticException
        return 1;         // No se ejecuta
    } catch (ArithmeticException ex) {
        System.out.println("Excepción capturada.");
        return 2;         // Antes de devolver 2...
    } finally {
        System.out.println("Ejecutando finally (con catch).");
        // ...se ejecuta siempre.
    }
}

En ambos ejemplos, el finally se ejecuta con total independencia de si hubo error o de si se usó return. Este comportamiento hace que el finally sea un mecanismo fiable para asegurar tareas críticas de limpieza en el flujo de un programa.

## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta

En Java, las excepciones controladas (checked exceptions) son aquellas que el compilador exige declarar con throws o capturar con un catch. Representan situaciones anómalas que se espera que el programador gestione porque son previsibles en condiciones normales de funcionamiento (errores de E/S, ficheros inexistentes, problemas de red…). Por el contrario, las excepciones no controladas (unchecked exceptions) derivan de RuntimeException y no requieren declaración ni captura obligatoria. Suelen indicar errores de programación o violaciones de precondiciones, que deben corregirse en el código más que tratarse de forma rutinaria.

La clase RuntimeException es la raíz de todas las excepciones no controladas. Su papel es representar errores que normalmente implican fallos lógicos: índices fuera de rango, referencias nulas, argumentos ilegales, conversiones incorrectas, etc. Dado que no requieren declaración obligatoria, permiten que el código sea más limpio cuando la situación errónea debería evitarse cumpliendo las precondiciones y no mediante un manejo exhaustivo. También es habitual que nuestras excepciones personalizadas hereden de RuntimeException cuando queremos indicar un uso indebido de un método sin obligar a capturar en todos los niveles.

Ejemplos típicos de excepciones controladas que nosotros mismos podríamos usar:

- IOException (operaciones de E/S que pueden fallar).
- SQLException (errores al acceder a una base de datos).  
- FileNotFoundException (acceder a un fichero inexistente).  
- ParseException (fallo al interpretar un dato externo).

Ejemplos típicos de excepciones no controladas:

- NullPointerException (uso indebido de referencias nulas).
- IllegalArgumentException (argumento inválido, como raíz de negativo).  
- ArrayIndexOutOfBoundsException (índice fuera de rango).  
- ArithmeticException (división entre cero).

Situaciones donde se prefiere una excepción controlada (3–4 ejemplos):

1. Operaciones con ficheros que pueden no existir.
2. Lectura de datos externos cuyo formato puede ser incorrecto.  
3. Conexiones de red que pueden fallar por causas externas.  
4. Acceso a bases de datos donde puede haber errores en tiempo de ejecución.

Situaciones donde se prefiere una excepción no controlada:

1. Violaciones de precondiciones (argumentos ilegales).
2. Fallos de programación (índices fuera de rango, nulos inesperados).  
3. Lógica interna donde la corrección debe garantizarse, no manejarse.  
4. Métodos cuyo uso indebido indica un bug, no una situación excepcional.

En conjunto, la distinción permite que Java separe los errores esperables y gestionables (controlados) de los errores lógicos o de programación (no controlados), equilibrando robustez y claridad en el código.


## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta

En Java, la cláusula throws se utiliza en la cabecera de un método para declarar que dicho método puede lanzar una o varias excepciones controladas (checked). Esta declaración obliga a cualquier código que llame al método a decidir cómo tratar ese posible fallo: o bien capturarlo con un bloque try-catch, o bien volver a declararlo en su propia firma con otro throws. Así, throws actúa como una forma explícita de documentar y propagar errores que forman parte del comportamiento normal del método, como problemas de E/S o de formato externo.

La razón de que throws sea una alternativa a capturar la excepción es que el compilador exige elegir entre capturar (catch) o propagar (throws). En ciertos niveles de la aplicación, puede no tener sentido manejar el error allí mismo, bien porque el método no dispone de información suficiente para solucionarlo, o bien porque se prefiere delegar la responsabilidad a una capa superior (por ejemplo, la interfaz de usuario o una clase especializada en manejar fallos). Declarar throws permite evitar un manejo artificial o incorrecto del error, y posponer la decisión a quien realmente pueda resolverlo.

Este mecanismo también favorece la claridad y la separación de responsabilidades. Un método puede concentrarse en su tarea principal —por ejemplo, leer un fichero o interpretar un dato— y dejar que otro componente decida cómo informar al usuario, registrar el evento o intentar una recuperación. En este sentido, throws no evita el error, pero sí garantiza que no pueda ser ignorado accidentalmente, ya que el compilador obliga a tratarlo de algún modo concreto.

## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta

Una forma típica es declarar en la firma que el método puede lanzar la excepción controlada y no manejarla localmente. En este caso, abrir un fichero puede lanzar FileNotFoundException/IOException. Se usará throws para propagar el error y un finally para cerrar el recurso si llegó a abrirse, garantizando la liberación incluso cuando ocurra una excepción. Obsérvese que el método no imprime ni decide cómo reaccionar: delega esa responsabilidad al llamador.

En el llamador se puede elegir capturar la excepción o volver a declararla con throws. Si se captura, el código del finally del método se ejecutará igualmente antes de que la excepción llegue al catch del llamador. Si no se captura, la excepción seguirá propagándose tras ejecutar el finally. Este patrón mantiene separadas la lógica de negocio (abrir/leerse el fichero) y la política de manejo de errores (informar, reintentar, abortar).

import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class LectorFichero {

    // Firma que declara que NO maneja la excepción: la propaga al llamador
    public static String leerPrimeraLinea(String ruta) throws IOException {
        BufferedReader br = null;
        try {
            br = new BufferedReader(new FileReader(ruta)); // puede lanzar FileNotFoundException
            return br.readLine();                           // puede lanzar IOException
        } finally {
            // Se ejecuta siempre, haya o no excepción y aunque haya return arriba
            if (br != null) {
                try {
                    br.close();
                } catch (IOException e) {
                    // Opcional: registrar el fallo de cierre, pero no se re-lanza aquí
                }
            }
        }
        // Si ocurrió una excepción en el try, tras este finally se propagará automáticamente
    }

    public static void main(String[] args) {
        // Opción A: capturar aquí
        try {
            String linea = leerPrimeraLinea("datos.txt");
            System.out.println("Primera línea: " + linea);
        } catch (IOException ex) {
            System.err.println("No se pudo leer el fichero: " + ex.getMessage());
        }

        // Opción B: o bien propagar desde main (alternativa)
        // public static void main(String[] args) throws IOException {
        //     String linea = leerPrimeraLinea("datos.txt");
        //     System.out.println("Primera línea: " + linea);
        // }
    }
}

Se ha usado throws IOException en vez de solo FileNotFoundException porque tanto la apertura como la lectura/cierre pueden fallar con distintos subtipos de IOException. Se mantiene el finally explícito para mostrar la garantía de cierre aun con return o error en el try. (En Java moderno, también sería válido un try-with-resources, pero aquí se prioriza ilustrar finally tal y como se solicita.)


## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta

Sí, en Java se puede listar excepciones no controladas (subclases de RuntimeException) en la cláusula throws, aunque no es obligatorio. El compilador no exige declararlas ni capturarlas, por lo que incluir throws RuntimeException (o una subclase) es informativo/documental: comunica a quien llama que, en condiciones de uso incorrecto o fallo lógico, el método puede fallar con ese tipo concreto. Esto resulta útil en APIs públicas para dejar claras las precondiciones y para que herramientas de análisis estático o la propia lectura del código anticipen posibles fallos.

El método llamador no está obligado a poner try-catch en ese caso. Con excepciones no controladas, la política habitual es no capturarlas en puntos intermedios, salvo en límites de la aplicación (por ejemplo, en la capa de UI/servicio) donde se quiera registrar, transformar o devolver una respuesta de error controlada. Aun así, sí puede tener sentido capturarlas puntualmente para: (1) traducirlas a una excepción controlada o a un error de dominio más expresivo; (2) enriquecer el contexto (añadir datos de entrada, identificadores, etc.); o (3) proteger un bucle/worker para que no se caiga todo el proceso por un bug aislado.

En resumen: poner RuntimeException (o similares) en throws sirve como documentación contractual y hace explícito que el método confía en que el llamador cumpla las precondiciones. No obliga a capturar. Se recomienda capturar sólo en lugares donde tenga sentido recuperar o informar adecuadamente (fronteras del sistema), o donde convenga envolver y re-lanzar con más contexto; en el resto, se suele permitir la propagación natural para que el error no se oculte y se detecte temprano.

Ejemplo ilustrativo (documentar una IllegalArgumentException “unchecked” sin obligar a capturarla):

public class Calculadora {
    // Documenta que puede lanzar IllegalArgumentException (unchecked), pero no obliga a catch
    public static double raiz(double x) throws IllegalArgumentException {
        if (x < 0) {
            throw new IllegalArgumentException("La raíz requiere un número no negativo.");
        }
        return Math.sqrt(x);
    }
}

Y un punto “frontera” donde sí puede tener sentido capturar para informar al usuario o registrar:

public static void main(String[] args) {
    try {
        System.out.println(Calculadora.raiz(Double.parseDouble(args[0])));
    } catch (IllegalArgumentException ex) {
        System.err.println("Entrada inválida: " + ex.getMessage());
        // Aquí tendría sentido: se informa al usuario y no se cae el programa.
    }
}




## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta

Las excepciones controladas se recomiendan cuando el fallo forma parte del funcionamiento normal del programa y el llamador tiene posibilidad razonable de recuperarse. Son situaciones externas, previsibles y ajenas a un error de programación, como problemas de entrada/salida, ausencia de un fichero, errores de red o interrupciones. En estos casos, obligar al llamador a decidir entre capturar o propagar mediante throws ayuda a no ignorar fallos que requieren tratamiento explícito. Así, excepciones como IOException, SQLException o ParseException encajan en este modelo, ya que son parte del contrato del método y pueden resolverse según la lógica de negocio.

Por el contrario, las excepciones no controladas se usan para indicar errores de programación o violaciones de precondiciones. Situaciones como pasar un valor negativo a la raíz cuadrada, acceder fuera del tamaño de un array o dividir entre cero representan fallos en la lógica interna y no algo que se pretenda “gestionar” rutinariamente. En estos casos, excepciones como IllegalArgumentException, NullPointerException o ArithmeticException permiten que el error aflore rápidamente, sin obligar a añadir bloques try-catch en cada llamada. El objetivo principal es evitar silencios accidentales y asegurar que el programa falle pronto durante el desarrollo.

No todos los lenguajes distinguen entre controladas y no controladas como Java. De hecho, Java es uno de los pocos lenguajes populares que impone esta separación en el compilador. En lenguajes como C++, Python, C#, JavaScript o Rust, todas las excepciones funcionan como no controladas: el compilador no obliga a capturarlas ni a declararlas. En estos entornos, lo más habitual es usar excepciones no controladas para todos los casos, y el estilo general consiste en capturar únicamente en los niveles donde tenga sentido recuperarse o en las fronteras del programa.

En los lenguajes donde sólo existe una opción, la práctica más común consiste en tratar las excepciones como errores que pueden ocurrir, pero sin imponer la obligación de capturarlos en cada paso. No se fuerza la declaración en la firma del método, y las capas superiores deciden si deben manejar o dejar propagarse el error. Esto favorece un código más limpio, aunque requiere disciplina para no ignorar errores que sí deberían gestionarse explícitamente. En resumen, los lenguajes sin excepciones controladas siguen un modelo más cercano al de las excepciones no controladas de Java, que resulta más flexible y menos intrusivo en el flujo normal del programa.

## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta

Sí, tiene sentido lanzar excepciones dentro de un catch cuando se desea traducir el error a un tipo más significativo para la capa superior, o enriquecerlo con contexto adicional (parámetros, identificadores, pista de qué operación falló). Este patrón se conoce como exception translation o wrapping: se captura la excepción original y se lanza una nueva, estableciendo la causa (cause) para no perder la traza original. También es útil para convertir una checked (IOException, SQLException) en una unchecked (RuntimeException) en fronteras donde no se pretende obligar a todos los clientes a tratarla.

public Usuario cargarUsuario(String id) {
    try {
        return dao.cargar(id); // puede lanzar SQLException (checked)
    } catch (java.sql.SQLException ex) {
        // Se aporta contexto del dominio y se preserva la causa
        throw new DatosInaccesiblesException("No se pudo cargar usuario id=" + id, ex);
        // class DatosInaccesiblesException extends RuntimeException { public DatosInaccesiblesException(String m, Throwable c){ super(m,c); } }
    }
}

También se puede relanzar la misma excepción capturada: catch (E e) { /*log*/ throw e; }. Esto tiene sentido cuando la capa actual no puede recuperarse, pero sí quiere registrar o medir el fallo antes de dejar que se propague naturalmente; o cuando se desea ejecutar lógica compensatoria (p. ej., métricas, tracing) sin cambiar el tipo de error. Al relanzar la misma instancia se conserva íntegra la traza original; si se lanzara un new sin causa, se perdería información relevante.

public void procesarPedido(Pedido p) {
    try {
        validar(p);              // puede lanzar IllegalArgumentException (unchecked)
        servicioPago.cobrar(p);  // puede lanzar PagoRechazadoException (unchecked)
    } catch (RuntimeException e) {
        auditor.logError("Fallo al procesar pedido " + p.id(), e);
        throw e; // se RELANZA la MISMA excepción, no se cambia el tipo
    } finally {
        recursos.liberar(); // se ejecuta siempre
    }
}

Cuándo usar cada enfoque: (a) Traducir/ envolver cuando se quiere elevar el nivel semántico del error (pasar de error técnico a uno de dominio) o cambiar la checked/unckecked en los límites de la API; (b) Relanzar igual cuando sólo se pretende añadir observabilidad (log, métricas, tracing) o ejecutar limpieza, dejando que la política de manejo quede en una capa superior. Lo que conviene evitar es capturar y silenciar (“swallowing”) sin acción ni relanzado, porque oculta fallos y deja el sistema en estados difíciles de diagnosticar.

## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta

Ser la “causa” de otra excepción significa que una excepción de bajo nivel (por ejemplo, técnica como IOException) se encapsula dentro de una excepción de alto nivel (de dominio), preservando el origen real del fallo. En Java, esto se hace pasando la excepción original como cause al constructor de la nueva excepción (new MiExcepcion("mensaje", cause)), o bien llamando a initCause. Este patrón, conocido como exception chaining, permite elevar el nivel semántico del error sin perder la traza original, facilitando el diagnóstico posterior.

Ejemplo: se captura una IOException al leer un fichero y se traduce a una excepción de dominio DatosInaccesiblesException, que hereda de RuntimeException y preserva la causa. El llamador puede capturar la de alto nivel, y si se imprime la traza (printStackTrace() o logs), se verá la cadena con “Caused by:” mostrando la excepción original y su traza completa.

// Excepción de alto nivel, con soporte de causa
class DatosInaccesiblesException extends RuntimeException {
    public DatosInaccesiblesException(String mensaje, Throwable causa) {
        super(mensaje, causa);
    }
}

import java.io.*;
public class Repositorio {
    public String cargarConfiguracion(String ruta) {
        try (BufferedReader br = new BufferedReader(new FileReader(ruta))) {
            return br.readLine();
        } catch (IOException e) {
            // Se añade contexto de dominio y se preserva la causa
            throw new DatosInaccesiblesException(
                "No se pudo cargar la configuración desde: " + ruta, e
            );
        }
    }

    public static void main(String[] args) {
        new Repositorio().cargarConfiguracion("inexistente.conf");
    }
}

Cuando una excepción sale por pantalla (por ejemplo, porque no se capturó y la JVM imprime el stack trace o porque se llamó a ex.printStackTrace()), si tiene causa se muestra explícitamente con el formato estándar: primero la traza de la excepción de alto nivel y, a continuación, una sección Caused by: <tipo>: <mensaje> con la traza de la excepción original. Si hay una cadena de varias causas, cada una aparecerá anidada, lo que facilita reconstruir el recorrido desde el error técnico inicial hasta el error de dominio expuesto. Esta información encapsulada es una ventaja clara respecto a códigos de error planos.