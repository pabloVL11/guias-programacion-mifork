<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Composición". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación y Excepciones.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.1. Composición


## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

### Respuesta

La composición en C se basa en la capacidad de definir estructuras que contienen otras estructuras, lo que permite representar relaciones del tipo “A tiene‑un B”. Esta técnica resulta útil para modelar elementos complejos construidos a partir de otros más simples. En el caso de una línea compuesta por dos puntos, cada punto puede representarse mediante una estructura con sus coordenadas, mientras que la línea contendrá dos instancias de esa estructura. Este enfoque facilita la organización del código y la reutilización de componentes.

Además, es habitual acompañar estas estructuras de funciones que operan sobre ellas. Por ejemplo, una función para calcular la distancia entre dos puntos aplica la fórmula derivada del teorema de Pitágoras, mientras que otra función puede usar dicha distancia para obtener la longitud de una línea. Separar estas operaciones mejora la claridad del programa y favorece la modularidad típica de la programación estructurada.

Por último, este uso de funciones y estructuras permite introducir conceptos que posteriormente se trasladarán de forma natural a Java, donde la composición aparece también mediante atributos dentro de clases. Aunque en C no exista orientación a objetos, el diseño por composición sigue siendo una herramienta clave para crear modelos más abstractos y manejables.

#include <stdio.h>
#include <math.h>

typedef struct {
    double x;
    double y;
} Punto;

typedef struct {
    Punto inicio;
    Punto fin;
} Linea;

double distancia(Punto a, Punto b) {
    double dx = b.x - a.x;
    double dy = b.y - a.y;
    return sqrt(dx*dx + dy*dy);
}

double longitudLinea(Linea l) {
    return distancia(l.inicio, l.fin);
}

int main() {
    Punto p1 = {0.0, 0.0};
    Punto p2 = {3.0, 4.0};
    Linea l = {p1, p2};

    printf("Distancia entre puntos: %.2f\n", distancia(p1, p2));
    printf("Longitud de la línea: %.2f\n", longitudLinea(l));

    return 0;
}




## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

### Respuesta

La composición en orientación a objetos permite modelar estructuras complejas definiendo clases cuyos atributos son instancias de otras clases. En este caso, una línea puede representarse como un objeto que contiene dos puntos, del mismo modo que ocurría con struct en C, pero con mayor capacidad de control. Gracias a la encapsulación, cada clase puede ocultar sus detalles internos, exponiendo solo los métodos necesarios para interactuar con sus datos. Esto permite diseñar modelos más seguros y coherentes que los logrados mediante simples estructuras.

En Java, la inmutabilidad se consigue declarando los atributos como private y final, de forma que solo puedan inicializarse en el constructor. Esta estrategia impide que los valores internos se modifiquen posteriormente, reforzando la idea de que un punto representa coordenadas fijas y una línea permanece unida siempre a los mismos puntos. Se trata de una mejora notable frente al enfoque de C, donde no existe un mecanismo nativo para restringir la modificación de campos salvo disciplina por parte del programador.

Además, al asociar a cada clase comportamientos propios, como el cálculo de distancias o de longitudes, se consigue un diseño más modular. Cada objeto sabe operar sobre sí mismo sin que el código externo tenga que manipular directamente los datos internos. De esta forma, la composición se integra de forma natural con los principios de la orientación a objetos, favoreciendo una organización más clara, extensible y robusta del programa.

public final class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distanciaA(Punto otro) {
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        return Math.sqrt(dx * dx + dy * dy);
    }

    public double getX() {
        return x;
    }

    public double getY() {
        return y;
    }
}

public final class Linea {
    private final Punto inicio;
    private final Punto fin;

    public Linea(Punto inicio, Punto fin) {
        this.inicio = inicio;
        this.fin = fin;
    }

    public double longitud() {
        return inicio.distanciaA(fin);
    }

    public Punto getInicio() {
        return inicio;
    }

    public Punto getFin() {
        return fin;
    }
}
``


## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

### Respuesta

La multiplicidad en composición describe cuántas instancias de un tipo pueden estar asociadas a una instancia de otro tipo dentro de una relación de “todo–parte”. Este concepto se emplea para precisar si un objeto está compuesto por uno, varios o muchos elementos internos, y también si esos elementos pueden pertenecer a más de un objeto compuesto. En modelos orientados a objetos, esta información permite entender mejor la estructura del sistema y documentar de forma clara sus restricciones internas.

En el ejemplo anterior, una Linea está formada exactamente por dos objetos de tipo Punto, y dicha cantidad no puede variar porque la línea es inmutable. Esto implica que la multiplicidad desde Linea hacia Punto es 2, o expresado de forma UML: Linea → Punto: 2. En otras palabras, toda línea tiene dos puntos obligatorios y no puede tener ni más ni menos.

En dirección contraria, cada Punto individual no está asociado necesariamente a ninguna línea en concreto. Un punto puede no formar parte de ninguna línea, puede ser compartido por varias, o incluso utilizarse para cálculos independientes. Por lo tanto, la multiplicidad desde Punto hacia Linea es 0.., indicando que un punto puede pertenecer a cero, una o múltiples líneas. Expresado en notación UML: Punto → Linea: 0...


## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

### Respuesta

La composición fuerte y la débil describen distintos grados de dependencia entre un objeto “todo” y los objetos que lo componen. En una composición débil, también llamada agregación, las partes pueden existir de forma independiente del todo. Esto significa que el ciclo de vida de los objetos agregados no está ligado al ciclo de vida del objeto que los contiene: pueden crearse antes, usarse por separado, o seguir existiendo incluso después de que el objeto contenedor desaparezca. Esta relación se utiliza cuando los elementos mantienen autonomía y pueden compartirse entre distintas estructuras.

En contraste, la composición fuerte, llamada simplemente composición en sentido estricto, establece una relación mucho más estrecha. En este caso, los objetos que forman parte del todo dependen completamente de él, de manera que su ciclo de vida está ligado al de la entidad compuesta. Si el objeto “todo” se destruye o deja de existir, sus partes también deben considerarse destruidas. Esto refleja la idea de que las partes no tienen una identidad útil separada fuera del conjunto que las contiene.

La consecuencia principal de esta distinción recae, por tanto, en la gestión del ciclo de vida. En una agregación, las partes poseen independencia y pueden sobrevivir más allá del objeto que las agrupa. En una composición fuerte, las partes nacen y mueren junto al objeto que las contiene, reforzando la idea de unidad inseparable. Por ello, en terminología habitual, la composición débil es lo que suele llamarse asociación o agregación, mientras que la composición fuerte es la que se denomina composición propiamente dicha. ¿Seguimos con la siguiente?


## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

### Respuesta

Cuando una clase utiliza otra únicamente de manera puntual dentro de un método, se está ante una relación de dependencia, no de composición. La dependencia indica que una clase necesita a otra para llevar a cabo una operación concreta, pero no mantiene una relación estructural ni de posesión sobre ella. Esta situación aparece cuando un método recibe objetos como parámetros, devuelve objetos de otro tipo o crea instancias temporales mediante new dentro de la propia operación.

En una relación de dependencia, los objetos usados no forman parte del estado interno del objeto que los emplea. Su ciclo de vida es independiente: pueden existir antes o después de la llamada al método, y nada obliga a que queden asociados de manera permanente a la clase que los utiliza. Se trata, por tanto, de una relación más débil y circunstancial, que describe simplemente que una clase depende de otra para realizar cierta funcionalidad.

Esto contrasta con la composición, donde la clase que compone posee internamente a los objetos componentes, conservándolos como parte de su estructura y gestionando su ciclo de vida. En dependencia, las demás clases actúan solamente como colaboradores momentáneos, sin integrarse en el estado ni en la identidad del objeto que los usa.
Si lo deseas, puedo preparar una tabla comparativa entre dependencia, agregación y composición, o pasamos directamente a la siguiente pregunta del cuestionario


## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

### Respuesta

En una composición fuerte, los objetos que forman parte de otro se crean dentro del propio objeto contenedor y no existen fuera de él. Esto implica que su ciclo de vida está completamente ligado al del objeto que los contiene: si la línea deja de existir, sus puntos también dejan de existir. Este diseño se emplea cuando las partes no tienen sentido por sí solas, o cuando resulta deseable que estén totalmente controladas por el objeto compuesto. La inmutabilidad refuerza este vínculo, ya que impide modificar la estructura interna después de su construcción.

En cambio, una composición débil (habitualmente llamada agregación) permite que las partes existan independientemente. En este modelo, la línea no crea los puntos, sino que los recibe desde fuera y simplemente los utiliza como elementos componentes. Los puntos pueden existir antes, después y fuera de la línea, pudiendo incluso ser compartidos con otras líneas u otras estructuras. Esta relación es adecuada cuando los objetos poseen identidad propia y no dependen estrictamente de un contenedor para tener sentido.

La consecuencia práctica principal es que, en composición fuerte, el programador controla al 100% la existencia de las partes, mientras que en composición débil se facilita la reutilización de objetos externos. Ambas relaciones son útiles dependiendo del modelo conceptual: si los puntos pertenecen en exclusiva a la línea, se opta por composición fuerte; si los puntos son reutilizados, la opción correcta es la composición débil.

Composición fuerte (los puntos nacen y mueren con la línea):

public final class LineaFuerte {
    private final Punto inicio;
    private final Punto fin;

    public LineaFuerte(double x1, double y1, double x2, double y2) {
        this.inicio = new Punto(x1, y1);   // Se crean dentro
        this.fin = new Punto(x2, y2);      // No existen fuera
    }

    public double longitud() {
        return inicio.distanciaA(fin);
    }

    public Punto getInicio() {
        return inicio; // Se pueden exponer, pero su vida depende de LineaFuerte
    }

    public Punto getFin() {
        return fin;
    }
}

Composición débil (la línea recibe puntos ya existentes):

public final class LineaDebil {
    private final Punto inicio;
    private final Punto fin;

    public LineaDebil(Punto inicio, Punto fin) {
        this.inicio = inicio;  // No se crean, solo se usan
        this.fin = fin;        // Existen fuera de la línea
    }

    public double longitud() {
        return inicio.distanciaA(fin);
    }

    public Punto getInicio() {
        return inicio;
    }

    public Punto getFin() {
        return fin;
    }
}


## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

### Respuesta

En una composición fuerte dentro de Java, el contenedor no destruye explícitamente los objetos que contiene porque Java no ofrece un mecanismo de destrucción manual como sucede en lenguajes con gestión manual de memoria, por ejemplo C o C++. En su lugar, la plataforma utiliza un recolector de basura (Garbage Collector) encargado de liberar memoria cuando detecta que un objeto ya no es accesible desde ninguna parte del programa. Esto significa que, cuando una instancia de Linea deja de usarse y ya no hay referencias apuntando a ella, también dejan de existir referencias hacia los puntos creados exclusivamente para esa línea.

El motivo por el que no se observa ningún método que destruya los Punto en la clase Linea es que Java no permite al programador decidir explícitamente cuándo liberar memoria. La destrucción ocurre de manera automática y diferida, en el momento en que el recolector de basura determina que un objeto es inalcanzable. En una composición fuerte, los objetos internos solo son accesibles a través del contenedor; por ello, cuando el contenedor deja de tener referencias, sus componentes también se vuelven inalcanzables y, por tanto, elegibles para ser recolectados.

Esta gestión automatizada encaja bien con el concepto de composición fuerte, porque asegura que las partes “mueren” junto con el todo sin necesidad de un procedimiento manual. Al mismo tiempo, este enfoque evita errores comunes de lenguajes con gestión manual, como fugas de memoria o dobles liberaciones. El programador se limita a garantizar que los objetos no queden referenciados innecesariamente, y el sistema se ocupa de liberar los recursos cuando corresponde.


## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

### Respuesta

Una composición débil entre un departamento y sus profesores implica que los profesores existen de manera independiente y pueden vivir fuera del departamento. El departamento simplemente mantiene referencias a ellos y los utiliza como parte de su estructura lógica, pero sin ser responsable de su ciclo de vida. En este modelo, los profesores pueden añadirse, eliminarse o ser compartidos entre distintos departamentos hipotéticos. Esta relación se modela mediante un contenedor interno, pero sin que el departamento cree ni destruya a los profesores.

La particularidad del ejercicio consiste en mantener dos relaciones simultáneas: el departamento debe tener una lista de profesores y, además, debe tener un director, que también es un profesor del departamento. Esto introduce una invariante clave de clase: el director debe formar siempre parte de la lista. Para preservarla, se requiere comprobar esta condición tanto al cambiar el director como al eliminar un profesor. El uso de un array interno de tamaño máximo 50 reflejará composición débil, ya que únicamente se almacenan referencias proporcionadas desde fuera.

El reto consiste también en no romper la encapsulación: no debe revelarse que internamente se utiliza un array. Para ello, se exponen métodos de acceso controlados que permitan añadir profesores, eliminarlos por posición, consultar cuántos hay y obtener uno por índice. Estos métodos deben garantizar siempre la validez de las invariantes y lanzar excepciones cuando alguna operación pretenda violarlas.

Implementación en Java (composición débil + invariantes):

public final class Profesor {
    private final String nombre;

    public Profesor(String nombre) {
        if (nombre == null || nombre.isBlank()) {
            throw new IllegalArgumentException("Nombre inválido");
        }
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}

public final class Departamento {
    private static final int MAX_PROFESORES = 50;

    private final Profesor[] profesores = new Profesor[MAX_PROFESORES];
    private int numProfesores = 0;

    private Profesor director;  // Debe ser siempre un profesor del departamento

    public Departamento(Profesor directorInicial) {
        if (directorInicial == null) {
            throw new IllegalArgumentException("Debe haber director inicial");
        }
        // Se añade el director como primer profesor
        profesores[0] = directorInicial;
        numProfesores = 1;
        director = directorInicial;
    }

    // Añadir profesor al final
    public void añadirProfesor(Profesor p) {
        if (p == null) {
            throw new IllegalArgumentException("Profesor inválido");
        }
        if (numProfesores >= MAX_PROFESORES) {
            throw new IllegalStateException("Departamento lleno");
        }
        profesores[numProfesores++] = p;
    }

    // Eliminar profesor por posición
    public void eliminarProfesor(int pos) {
        if (pos < 0 || pos >= numProfesores) {
            throw new IndexOutOfBoundsException("Posición inválida");
        }

        Profesor eliminado = profesores[pos];

        // Invariante: no se puede eliminar al director
        if (eliminado == director) {
            throw new IllegalStateException("No se puede eliminar al director del departamento");
        }

        // Compactar el array para no dejar huecos
        for (int i = pos; i < numProfesores - 1; i++) {
            profesores[i] = profesores[i + 1];
        }
        profesores[--numProfesores] = null;
    }

    // Cambiar director: debe ser profesor existente
    public void cambiarDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) {
            throw new IllegalArgumentException("Director inválido");
        }
        if (!contieneProfesor(nuevoDirector)) {
            throw new IllegalStateException("El director debe formar parte del departamento");
        }
        director = nuevoDirector;
    }

    // Comprobación interna sin romper encapsulación
    private boolean contieneProfesor(Profesor p) {
        for (int i = 0; i < numProfesores; i++) {
            if (profesores[i] == p) {
                return true;
            }
        }
        return false;
    }

    public int getNumeroProfesores() {
        return numProfesores;
    }

    public Profesor getProfesor(int pos) {
        if (pos < 0 || pos >= numProfesores) {
            throw new IndexOutOfBoundsException("Posición inválida");
        }
        return profesores[pos];
    }

    public Profesor getDirector() {
        return director;
    }
}



## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

### Respuesta

El uso de List en Java simplifica notablemente la gestión de colecciones, ya que evita gestionar manualmente el tamaño, el desplazamiento de elementos al eliminar, y las comprobaciones asociadas al array primitivo. En el ejemplo del departamento, el cambio principal consiste en sustituir el array fijo por una lista dinámica, delegando en esta estructura las operaciones de añadido, eliminación y desplazamiento interno. Gracias a ello, desaparecen partes enteras del código original, como la compactación manual del array o la gestión explícita del límite máximo, lo que mejora la claridad y reduce la posibilidad de errores.

Sin embargo, al trabajar con List, debe mantenerse la encapsulación. Exponer directamente la lista interna permitiría que código externo añadiera, eliminara o modificara profesores sin control, rompiendo invariantes fundamentales, como la obligatoriedad del director o la coherencia interna de la colección. Si existiera un método que devolviera todos los profesores a la vez, no podría devolverse la lista interna tal cual, porque se estaría entregando el control total de la estructura al exterior. La solución adecuada consiste en devolver una copia defensiva o bien una vista inmodificable, garantizando así que la estructura interna permanezca protegida.

Este enfoque mantiene la separación entre la representación interna del departamento y la interfaz pública que ofrece, respetando la encapsulación y asegurando que las invariantes sigan siendo válidas. De este modo, la lista se usa como un detalle de implementación, mientras que la clase conserva el control sobre las operaciones permitidas y el orden en que deben verificarse las condiciones lógicas de la relación entre profesores y departamento.

Implementación con List:

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public final class Profesor {
    private final String nombre;

    public Profesor(String nombre) {
        if (nombre == null || nombre.isBlank()) {
            throw new IllegalArgumentException("Nombre inválido");
        }
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}

public final class Departamento {
    private final List<Profesor> profesores =(Profesor p) {    private final List<Profesor> profesores = new ArrayList<>();
        if (p == null) {
            throw new IllegalArgumentException("Profesor inválido");
        }
        profesores.add(p);
    }

    public void eliminarProfesor(int pos) {
        if (pos < 0 || pos >= profesores.size()) {
            throw new IndexOutOfBoundsException("Posición inválida");
        }

        Profesor eliminado = profesores.get(pos);

        if (eliminado == director) {
            throw new IllegalStateException("No se puede eliminar al director");
        }

        profesores.remove(pos);
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) {
            throw new IllegalArgumentException("Director inválido");
        }
        if (!profesores.contains(nuevoDirector)) {
            throw new IllegalStateException("El director debe formar parte del departamento");
        }
        director = nuevoDirector;
    }

    public int getNumeroProfesores() {
        return profesores.size();
    }

    public Profesor getProfesor(int pos) {
        if (pos < 0 || pos >= profesores.size()) {
            throw new IndexOutOfBoundsException("Posición inválida");
        }
        return profesores.get(pos);
    }

    // Devolver copia defensiva
    public List<Profesor> getProfesores() {
        return Collections.unmodifiableList(new ArrayList<>(profesores));
    }

    public Profesor getDirector() {
        return director;
    }
}
    private Profesor director;

    public Departamento(Profesor directorInicial) {
        if (directorInicial == null) {
            throw new IllegalArgumentException("Debe haber director inicial");
        }
        profesores.add(directorInicial);
        director = directorInicial;
    }

¿Qué parte del código original se ha ahorrado?

-La gestión del tamaño máximo del array.
-El desplazamiento manual de elementos al eliminar uno.
-El control explícito de índices internos salvo verificaciones básicas.
-La inicialización del array y su mantenimiento.

¿Por qué no se puede devolver directamente la lista interna?

Porque el código externo podría modificarla libremente: añadir profesores sin control, eliminar al director, o romper cualquier invariante. Esto violaría la encapsulación y comprometería la integridad de la clase.

¿Cómo se soluciona?

Con una copia defensiva o una vista inmodificable:

-Copia defensiva: new ArrayList<>(profesores)
-Vista inmodificable: Collections.unmodifiableList(...)

Así, el exterior puede consultar los datos, pero no modificarlos.

## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

### Respuesta

La composición recursiva aparece cuando un objeto contiene otro objeto del mismo tipo, creando una estructura potencialmente encadenada. En este caso, una Persona puede tener como atributo a su madre, que es también otra Persona, y esta a su vez puede tener otra madre, formando una cadena genealógica. Este patrón es similar al que utilizan las excepciones en Java, donde una excepción puede encapsular otra como causa, generando una estructura jerárquica recursiva para describir errores encadenados. La clave es que cada objeto contiene un enlace a otra instancia del mismo tipo, permitiendo representar relaciones naturales que se repiten en distintos niveles.

En un diseño inmutable, los atributos se establecen en el constructor y nunca cambian después. Aplicado al caso de una persona y su madre, esto significa que la relación genealógica queda fijada al crear el objeto y no se modifica posteriormente. Esta aproximación facilita razonar sobre la estructura, evita inconsistencias y hace que la composición recursiva sea más segura. En este tipo de modelos, es habitual permitir que el último elemento de la cadena tenga un valor null para representar la ausencia de antecesores adicionales, como en el caso de la abuela sin madre conocida.

Este tipo de composición se presenta en muchos otros contextos. Un ejemplo clásico es el de nodos en estructuras de datos como listas enlazadas, árboles o grafos, donde cada nodo contiene referencias a otros nodos del mismo tipo. También aparece en expresiones aritméticas o lógicas representadas como árboles sintácticos, donde cada nodo puede contener subexpresiones igualmente representadas como nodos del mismo tipo. En todos estos casos, la recursividad del modelo permite representar estructuras complejas de forma natural y escalable.

Ejemplo en Java: composición recursiva con Persona inmutable:

public final class Persona {
    private final String nombre;
    private final Persona madre; // Composición recursiva

    public Persona(String nombre, Persona madre) {
        if (nombre == null || nombre.isBlank()) {
            throw new IllegalArgumentException("Nombre inválido");
        }
        this.nombre = nombre;
        this.madre = madre; // Puede ser null en la raíz de la cadena
    }

    public String getNombre() {
        return nombre;
    }

    public Persona getMadre() {
        return madre;
    }
}
``

Ejemplo de uso (main):

public class Main {
    public static void main(String[] args) {
        Persona abuela = new Persona("Isabel", null);
        Persona madre = new Persona("Laura", abuela);
        Persona hijo  = new Persona("Carlos", madre);

        System.out.println("Hijo: " + hijo.getNombre());
        System.out.println("Madre: " + hijo.getMadre().getNombre());
        System.out.println("Abuela: " + hijo.getMadre().getMadre().getNombre());
    }
}

Salida esperada:

Hijo: Carlos
Madre: Laura
Abuela: Isabel

Otros ejemplos clásicos de composiciones recursivas:

-Listas enlazadas: cada nodo contiene un dato y un enlace a otro nodo del mismo tipo.
-Árboles (como árboles binarios o de expresión): cada nodo contiene referencias a otros nodos hijos.
-Estructuras de directorios y ficheros: una carpeta contiene una lista de elementos, que pueden ser archivos o más carpetas.
-Expresiones matemáticas: una expresión contiene subexpresiones que también son expresiones.
-Excepciones encadenadas en Java, como comentabas en la pregunta.

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

### Respuesta

Las relaciones de composición bidireccional aparecen cuando dos clases mantienen referencias entre sí de forma simultánea: cada objeto del “todo” conoce a sus partes, pero cada parte también mantiene una referencia hacia el todo al que pertenece. Esta situación crea un vínculo más estrecho, ya que ambas instancias participan en la relación y deben mantenerse coherentes entre sí. La bidireccionalidad implica, por tanto, que las modificaciones efectuadas en un lado deben reflejarse adecuadamente en el otro para preservar las invariantes. En modelos reales, este patrón se utiliza cuando la relación necesita ser navegable en ambas direcciones.

Aplicado al ejemplo de Profesor y Departamento, implementar una composición bidireccional requeriría que cada Profesor almacenase una referencia al Departamento al que pertenece. De este modo, el departamento seguiría manteniendo una colección de profesores, pero cada profesor también conocería a su departamento. Esto obligaría a reforzar la gestión interna para garantizar que al añadir o eliminar un profesor, o al cambiar a otro departamento, se actualicen correctamente ambas direcciones de la relación. La coherencia de la relación sería responsabilidad tanto del departamento como de las operaciones que gestionan su lista interna.

Además, habría que decidir cómo controlar la mutabilidad de los profesores. Si los profesores son objetos inmutables, la referencia al departamento no podría cambiar, y por tanto no podrían moverse entre departamentos. En cambio, si se permite mutabilidad controlada en los profesores, sería necesario garantizar que el método que añade un profesor actualice también la referencia interna del profesor al departamento, y que el método que elimina un profesor la elimine o la sustituya por otra. En la práctica, este tipo de relación requiere encapsular cuidadosamente las operaciones que la gestionan para evitar incoherencias.

¿Qué habría que añadir para implementar la bidireccionalidad?

Para hacer la relación bidireccional en el ejemplo:

1.Añadir en Profesor un atributo:

private

2.Dar un método de acceso controlado (probablemente package‑private o privado):

void setDepartamento(Departamento d) {
    this.departamento = d;
}

3.Modificar Departamento.añadirProfesor:

añadir el profesor a la lista
llamar a p.setDepartamento(this);

4.Modificar Departamento.eliminarProfesor:

eliminar al profesor
hacer p.setDepartamento(null);


5.Asegurar en cambiarDirector que el director forme parte del mismo departamento, lo cual ya se hacía, pero ahora también respetando la referencia interna.

De esta forma, el profesor sabe a qué departamento pertenece y el departamento conoce a sus profesores, manteniendo una composición bidireccional sin romper las invariantes.