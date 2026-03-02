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
La **encapsulación** persigue agrupar en una misma unidad (la clase) el **estado**
(datos) y el **comportamiento** (métodos) que opera sobre ese estado. Frente a un
estilo más procedural (típico en C), se pasa a interactuar con objetos que ofrecen
operaciones y se responsabilizan de mantener sus propios datos consistentes.
La **ocultación de información** (information hiding) limita qué partes del estado y de
 la implementación quedan accesibles desde fuera. En lugar de permitir modificar campos
 directamente, se obliga a usar métodos diseñados para ello, donde se pueden aplicar
reglas y validaciones.
Ventajas típicas: se reduce el acoplamiento (menos dependencia de detalles internos),
se mejora la **robustez** evitando estados inválidos, se facilita mantener
**invariantes** de clase y se puede cambiar la implementación interna sin romper el
código cliente, siempre que se conserve la interfaz pública.


## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

### Respuesta
La **interfaz pública** es el conjunto de miembros accesibles desde fuera de la clase
(principalmente métodos y constructores). Actúa como un **contrato**: define qué
operaciones se pueden realizar con un objeto y qué resultados o efectos deben
esperarse.
La ocultación de información se apoya en esa interfaz para que el exterior no dependa
de cómo se representa o se calcula internamente. El usuario de la clase conoce *qué*
hace la clase mediante sus métodos públicos, pero no necesita conocer *cómo* lo hace.
Gracias a ello, la implementación interna puede evolucionar (cambiar estructuras,
optimizar cálculos) sin obligar a modificar el código que usa la clase, siempre que la
interfaz pública y su comportamiento observado se mantengan.


## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

### Respuesta
La interfaz pública debe diseñarse con cuidado porque todo lo expuesto pasa a ser una
dependencia del resto del programa. Si se publica un método “de más” o con una firma
poco pensada, es fácil que otras partes lo usen y queden acopladas a esa decisión.
Cambiar la interfaz pública suele ser **costoso**: renombrar métodos, cambiar
parámetros o tipos de retorno puede romper compilación o comportamiento en muchos
puntos. En proyectos grandes o librerías, además, puede romper compatibilidad con
versiones anteriores.
Por ese motivo se recomienda exponer lo mínimo necesario, usar nombres claros y3
preferir operaciones de alto nivel (orientadas a la intención) en lugar de exponer
detalles internos

Interza pubicla--> Los miembros q se ven desde fuera, es decir los q no están en la parte oculta. 


## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

### Respuesta
Las **invariantes de clase** son condiciones que deben cumplirse para que el estado de
un objeto sea válido. Por ejemplo, “un radio no puede ser negativo” o “la coordenada
debe ser finita”. Aunque dentro de un método el estado pueda pasar por estados
intermedios, al finalizar cualquier operación pública el objeto debe volver a cumplir
sus invariantes.
La ocultación de información ayuda porque impide que código externo modifique
directamente el estado sin controles. Si los atributos fueran públicos, cualquier parte
 del programa podría asignar valores inválidos y romper la consistencia.
Al mantener el estado privado, la clase centraliza la validación y garantiza que cada
cambio pase por métodos que pueden comprobar precondiciones y preservar las
invariantes.


## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

### Respuesta
Para aplicar ocultación, las coordenadas se guardan como atributos `private`, de modo
que no se puedan modificar desde fuera directamente. La clase ofrece métodos públicos
para construir el objeto y para operar con él, sin exponer cómo se almacena
internamente el estado.
La **interfaz pública** está formada por los miembros `public`: el constructor y los
métodos públicos (por ejemplo, `calcularDistanciaAOrigen()` y, si se desea, getters).
Lo `private` pertenece a la implementación interna y no debería usarse desde el
exterior.
`public` significa que el miembro es accesible desde cualquier clase. `private`
significa que sólo el código dentro de la propia clase puede acceder al miembro, lo que
 permite controlar el estado y mantener invariantes.
public class Punto {
 private final double x;
 private final double y;
 public Punto(double x, double y) {
 this.x = x;
 this.y = y;
 }
 public double calcularDistanciaAOrigen() {
 return Math.sqrt(x * x + y * y);
 }
 public double getX() { return x; }
 public double getY() { return y; }
}
""""


## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### Respuesta
 `public` y `private` se pueden aplicar a **atributos (campos)**, **métodos** y
 **constructores**. También se aplican a **tipos anidados** (clases, interfaces o enums
 declarados dentro de otra clase), lo que permite ocultar detalles internos de
implementación.
Una clase de nivel superior (top-level) no puede ser `private`: sólo puede ser `public`
 o tener visibilidad por defecto (package-private). En cambio, una clase interna sí
puede ser `private`, porque su alcance queda dentro de la clase contenedora.
El objetivo de estos modificadores es separar con claridad la interfaz pública de lo
que es interno, reduciendo dependencias externas y reforzando la encapsulación.




## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### Respuesta
Además de pública y privada, muchos lenguajes incluyen niveles intermedios de
visibilidad para dar acceso controlado. Java ofrece cuatro niveles: `public`,
`protected`, `private` y la visibilidad por defecto (sin modificador), conocida como
**package-private**.
`protected` permite acceso desde subclases y desde clases del mismo paquete. La
visibilidad por defecto permite acceso sólo dentro del paquete, lo que es útil para
ocultar componentes internos sin hacerlos totalmente privados.
Otros lenguajes orientados a objetos implementan conceptos similares: C++ tiene
`public/protected/private`, y C# añade `internal` para limitar el acceso al ensamblado.
 La idea común es equilibrar reutilización y encapsulación.




## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta
En Java, los miembros `private` están ocultos para **otras clases** (a), pero no están
ocultos para otras instancias de la **misma clase** cuando el acceso se realiza desde
código dentro de esa clase. El permiso lo tiene el *código de la clase*, no “cada
objeto por separado”.
Esto permite que un método de `Punto` acceda a `otro.x` y `otro.y` aunque sean
privados, sin necesidad de exponer getters sólo para uso interno. Se mantiene la
ocultación hacia el exterior y se facilita la implementación de operaciones naturales
entre instancias.
public class Punto {
 private final double x;
 private final double y;
 public Punto(double x, double y) {
 this.x = x;
 this.y = y;
 }
 public double calcularDistanciaAPunto(Punto otro) {
 double dx = this.x - otro.x;
 double dy = this.y - otro.y;
 return Math.sqrt(dx * dx + dy * dy);
 }
}
""""


## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta
Un **getter** es un método que permite leer el valor de un atributo (normalmente
`private`) de forma controlada, por ejemplo `getX()`. Un **setter** permite modificar
ese atributo, por ejemplo `setX(double x)`, y suele ser el lugar adecuado para validar
entradas y preservar invariantes.
La finalidad principal es mantener la **ocultación de información**: en vez de exponer
campos, se exponen operaciones. Esto permite cambiar cómo se guarda internamente un
dato sin obligar a cambiar el código que usa la clase.
No siempre conviene crear getters y setters para todo. A menudo es mejor exponer
métodos con significado (p. ej., `mover(dx, dy)`), evitando que el resto del programa
manipule el estado sin contexto.



## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta
En este contexto, “seguridad” suele significar **seguridad del diseño** (*safety*), no
ciberseguridad. Se refiere a evitar usos incorrectos del estado interno y a reducir la
probabilidad de errores lógicos por modificaciones no controladas.
Al ocultar el estado, las modificaciones pasan por métodos que pueden validar,
normalizar y mantener invariantes. Esto aumenta la robustez y hace el programa más
predecible y mantenible.
La protección frente a ataques (hacking) depende de otras medidas (validación de
entradas, autenticación, autorización, etc.). La encapsulación no sustituye esas
prácticas.


## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta
Un miembro de **instancia** pertenece a cada objeto: cada instancia tiene su propia
copia del atributo. Un miembro de **clase** pertenece a la clase como entidad única y
se comparte entre todas las instancias; en Java se declara con `static`.
Los miembros de clase también pueden ocultarse usando `private static`. Es habitual
encapsular contadores, máximos, cachés o constantes internas, exponiendo sólo métodos
públicos para consultar o modificar de forma controlada.
Así, la ocultación se aplica tanto al estado individual de cada objeto como al estado
compartido por toda la clase.


## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta
Sí, un constructor `private` tiene sentido cuando se desea **controlar** cómo se crean
las instancias. Esto obliga a usar métodos factoría (`static`) u otros mecanismos
internos para construir objetos, lo que permite aplicar validaciones, cacheo o
políticas de creación.
También se usa en clases de utilidades (solo métodos `static`) para impedir
instanciación accidental. Otro caso típico es el patrón Singleton, donde se restringe
el número de instancias.
En general, un constructor privado refuerza la encapsulación del proceso de
construcción y puede ayudar a garantizar invariantes desde el inicio.


## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta
Los miembros de clase se indican con `static`. Si se desean máximos globales, se
guardan en campos `static` compartidos y se actualizan cuando se crean instancias. Para
 mantener ocultación, esos campos deben ser `private static` y exponerse mediante
métodos de consulta.
De este modo, el exterior puede consultar los máximos sin poder modificarlos
arbitrariamente, evitando inconsistencias y manteniendo el control dentro de la clase.
public class Punto {
 private final double x;
 private final double y;
 private static double maxX = Double.NEGATIVE_INFINITY;
 private static double maxY = Double.NEGATIVE_INFIN
 public Punto(double x, double y) {
 this.x = x;
 this.y = y;
 if (x > maxX) maxX = x;
 if (y > maxY) maxY = y;
 }
 public static double getMaxX() { return maxX; }
 public static double getMaxY() { return maxY; }
}
""""



## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta
Un método factoría suele ser `static` porque se invoca desde la clase sin necesitar una
 instancia previa. Permite centralizar reglas de creación (como redondeo o validación)
en un único punto.
Usar un método factoría puede ser preferible al constructor si se desea expresar
intención (`crearRedondeado`) o aplicar transformaciones antes de construir el objeto.
public static Punto crearRedondeado(double x, double y) {
 return new Punto(Math.round(x), Math.round(y));
}
""""



## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta
Este cambio ilustra por qué la encapsulación es útil: si la interfaz pública se
mantiene, el código cliente no debería verse afectado. Se puede cambiar la
representación interna (dos campos vs. array) sin modificar cómo se usa la clase.
El array debe permanecer `private` para no romper la ocultación. Exponer el array
permitiría que código externo cambiase coordenadas sin control, violando invariantes y
creando acoplamiento con la representación.
public class Punto {
 private final double[] coords = new double[2]; // coords[0]=x, coords[1]=y
 public Punto(double x, double y) {
 coords[0] = x;
 coords[1] = y;
 }
 public double calcularDistanciaAOrigen() {
 double x = coords[0];
 double y = coords[1];
 return Math.sqrt(x * x + y * y);
 }
}
""""


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta
Aunque exista getter y setter, declarar el atributo público suele ser peor porque
elimina la posibilidad de controlar el acceso y limita cambios futuros. Un setter puede
 validar, normalizar o disparar actualizaciones internas; con un campo público eso se
pierde.
La convención más habitual es declarar atributos **privados**. Así se reduce
acoplamiento y se mantiene la libertad de cambiar la implementación interna sin romper
a los clientes.
Esto está ligado a las **invariantes**: si el estado sólo se cambia a través de métodos
 controlados, se puede garantizar que el objeto no queda en estados inválidos tras cada
 operación.



## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta
Una clase es **inmutable** si, tras construir un objeto, su estado no cambia. Esto se
logra típicamente con atributos `private final` y sin métodos que alteren el estado. Si
 se necesita un valor “distinto”, se devuelve una nueva instancia.
Un método modificador es cualquier método que cambia el estado interno. No siempre es
un setter: puede ser un método con semántica de operación (por ejemplo, `mover`,
`añadir`, `incrementar`) que modifica uno o varios atributos.
Las ventajas de la inmutabilidad incluyen menos efectos laterales, razonamiento más
simple, preservación natural de invariantes y mejor comportamiento en concurrencia
(objetos compartidos sin necesidad de sincronización adicional).



## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta
No es recomendable añadir setters por convención automática. Cada setter amplía la
interfaz pública y permite cambios externos, lo que puede dificultar mantener
invariantes y razonar sobre el ciclo de vida del objeto.
En muchos casos conviene exponer operaciones con significado del dominio en vez de
setters genéricos. Por ejemplo, `depositar(cantidad)` expresa intención y permite
validación, mientras que `setSaldo(...)` expone el estado de forma más frágil.
Por tanto, los setters deben existir sólo cuando son necesarios y coherentes con el
modelo y las reglas del objeto.


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta
`String` en Java es **inmutable**: su contenido no cambia tras crearse. Cuando se
concatenan cadenas, se crea una nueva instancia que contiene el resultado, dejando las
originales intactas.
Concatenar repetidamente en bucles suele ser ineficiente porque genera muchos objetos
intermedios y aumenta el trabajo del recolector de basura.
Para construir cadenas largas paso a paso, se debe usar `StringBuilder` (mutable) y al
final convertirlo a `String` con `toString()`.



## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta
En POO se distingue entre **identidad** (ser el mismo objeto) e **igualdad por
contenido** (representar el mismo valor lógico). En Java, `==` compara identidad para
objetos (la referencia), mientras que la igualdad por contenido se expresa con
`equals`.
`equals` en Java define la igualdad lógica. Por defecto, si no se sobrescribe,
`Object.equals` se comporta como identidad. Para comparar contenido en clases propias,
se sobrescribe `equals` (y normalmente `hashCode`) cumpliendo el contrato.
Las cadenas deben compararse con `equals` (`a.equals(b)`), no con `==`, porque `==`
sólo verifica si ambas referencias apuntan al mismo objeto.



## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta
En Java, las clases **wrapper** envuelven tipos primitivos para tratarlos como objetos
(por ejemplo, `Integer` para `int`, `Double` para `double`). Esto es necesario en APIs
que operan con objetos, como colecciones genéricas.
La conversión puede ser automática mediante **autoboxing/unboxing**: Java puede
convertir un primitivo a wrapper y viceversa en muchas expresiones. Aun así, conviene
recordar que existe creación/uso de objetos y que un `null` puede causar problemas al
desempaquetar.
Las ventajas incluyen compatibilidad con genéricos y métodos utilitarios. No todos los
lenguajes OO separan primitivos y objetos: algunos tratan casi todo como objeto y no
requieren wrappers en el mismo sentido.



## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta
Un **enumerado** representa un conjunto finito y cerrado de valores posibles. Es útil
para modelar opciones discretas (meses, estados, tipos) evitando valores inválidos que
podrían aparecer usando enteros o cadenas.
En Java, un `enum` es una clase especial: cada constante es una instancia única y se
pueden definir atributos, métodos y constructores. Esto permite asociar datos y
comportamiento a cada valor.
En términos de encapsulación, los enums impiden crear instancias fuera del conjunto
definido y permiten mantener datos internos `private`, exponiendo sólo métodos
públicos. Esto refuerza la consistencia del dominio.


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado.

### Respuesta
En un `enum`, el conjunto de instancias está cerrado, lo que evita valores inválidos.
Se pueden almacenar atributos `private final` y proporcionar getters para exponer
información de forma controlada.
El constructor del `enum` se usa para inicializar los atributos de cada constante. Al
ser parte del `enum`, no se permite crear instancias desde fuera, reforzando la
encapsulación.
public enum Mes {
 ENERO(31, 1),
 FEBRERO(28, 2),
 MARZO(31, 3),
 ABRIL(30, 4),
 MAYO(31, 5),
 JUNIO(30, 6),
 JULIO(31, 7),
 AGOSTO(31, 8),
 SEPTIEMBRE(30, 9),
 OCTUBRE(31, 10),
 NOVIEMBRE(30, 11),
 DICIEMBRE(31, 12);
 private final int dias;
 private final int ordinal;
 Mes(int dias, int ordinal) {
 this.dias = dias;
 this.ordinal = ordinal;
 }
 public int getDias() { return dias; }
 public int getOrdinal() { return ordinal; }
}
""""



## 24. Añade a la clase `Mes` del ejercicio anterior cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta
Para una clasificación sencilla por meses se puede usar la división meteorológica
típica: en el hemisferio norte, invierno (dic-ene-feb), primavera (mar-abr-may), verano
 (jun-jul-ago), otoño (sep-oct-nov). En el hemisferio sur se invierte seis meses.
Centralizar esta lógica en el `enum` es coherente con encapsulación: el resto del 
programa pregunta al mes por su estación sin duplicar reglas en múltiples sitios.
public boolean esDePrimavera(boolean esHemisferioNorte) {
 int m = this.getOrdinal();
 return esHemisferioNorte ? (m >= 3 && m <= 5) : (m >= 9 && m <= 11);
}
public boolean esDeVerano(boolean esHemisferioNorte) {
 int m = this.getOrdinal();
 return esHemisferioNorte ? (m >= 6 && m <= 8) : (m == 12 || m <= 2);
}
public boolean esDeOtono(boolean esHemisferioNorte) {
 int m = this.getOrdinal();
 return esHemisferioNorte ? (m >= 9 && m <= 11) : (m >= 3 && m <= 5);
}
public boolean esDeInvierno(boolean esHemisferioNorte) {
 int m = this.getOrdinal();
 return esHemisferioNorte ? (m == 12 || m <= 2) : (m >= 6 && m <= 8);
}
""""
```