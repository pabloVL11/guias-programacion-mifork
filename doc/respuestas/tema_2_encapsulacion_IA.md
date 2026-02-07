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


## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta


## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta


## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta


## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta


## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta


## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta


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
