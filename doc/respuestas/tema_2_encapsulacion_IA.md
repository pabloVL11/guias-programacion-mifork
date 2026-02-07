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


## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

### Respuesta


## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### Respuesta


## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### Respuesta


## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta


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
