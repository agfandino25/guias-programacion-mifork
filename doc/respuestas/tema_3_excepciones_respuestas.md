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

## 1. En C, donde no existen las excepciones… ¿Cómo indicamos error desde fuera de la función?
### Respuesta
En C, al no existir un sistema integrado de excepciones, los errores deben comunicarse de forma manual utilizando valores de retorno o parámetros adicionales. Una opción habitual consiste en devolver un valor especial que indique error, como `-1` o `NAN`, y permitir que el código llamador consulte ese valor para reaccionar adecuadamente. Este método es simple y funciona bien para funciones sencillas, aunque requiere disciplina del programador para comprobar siempre el resultado.

Otra opción consiste en utilizar un parámetro adicional que funcione como indicador de error, normalmente un puntero a una variable entera. La función puede modificar el valor apuntado para informar al llamador sobre si el error ocurrió o no. Esto separa el resultado real del indicador de error, permitiendo distinguirlos de forma clara.

```c
/* Opción 1: devolver valor especial */
double raiz(double x) {
    if (x < 0) return -1;  // indica error
    return sqrt(x);
}

/* Opción 2: parámetro adicional */
double raiz(double x, int* error) {
    if (x < 0) {
        *error = 1;
        return 0;
    }
    *error = 0;
    return sqrt(x);
} 
```
***
## 2. Brevemente ¿Qué es una *"excepción"*? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?
### Respuesta
Una *excepción* es un mecanismo de control de errores que permite interrumpir el flujo normal de ejecución cuando ocurre una situación anómala. En lugar de devolver valores especiales o códigos de error, el programa puede señalar un fallo lanzando una excepción, lo que produce un salto automático hacia un manejador adecuado. De esta forma, el código que detecta el error queda separado del que lo gestiona.

Para quien implementa funciones, las excepciones permiten comunicar fallos de manera más clara y menos dependiente de convenciones. Para quienes las llaman, proporcionan un mecanismo estructurado para concentrar la respuesta a los errores en lugares específicos del programa. Esto facilita escribir código más legible y robusto, evitando múltiples comprobaciones dispersas.



## 3. Reescribe el mismo ejemplo de raiz, pero en Java...
### Respuesta
En Java, es posible encapsular el cálculo de la raíz cuadrada en una clase `Calculadora`, lanzando una excepción cuando se recibe un argumento negativo. El método puede utilizar `throw` para indicar el error, mientras que el método `main` se encarga de capturar la excepción mediante un bloque `try-catch`. Así se consigue separar la detección del error del manejo del mismo.

```java
class Calculadora {
    double raiz(double x) throws Exception {
        if (x < 0) {
            throw new Exception("Número negativo");
        }
        return Math.sqrt(x);
    }
}

public class Main {
    public static void main(String[] args) {
        Calculadora c = new Calculadora();
        try {
            double r = c.raiz(-4);
            System.out.println("Raíz: " + r);
        } catch (Exception e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```
***



## 4. ¿Qué es *"lanzar"* una excepción? ¿Qué es *"controlar"* o *"capturar"* una excepción? ¿Qué es que se *"propague"* una excepción?

### Respuesta
*Lanzar* una excepción consiste en interrumpir la ejecución normal de un método cuando ocurre un error utilizando la palabra clave `throw`. Esta acción detiene inmediatamente el flujo del método en el punto donde aparece el lanzamiento. A partir de ese momento, el programa comienza a buscar un bloque `catch` adecuado donde manejar la excepción lanzada.

*Capturar* o *controlar* una excepción significa proporcionar un bloque `catch` donde el programa pueda reaccionar ante el fallo ocurrido. Dicho bloque solo se ejecuta si el tipo de la excepción lanzada coincide con el declarado en el `catch`. Si no existe un bloque capturador compatible, la excepción se *propaga* automáticamente hacia arriba en la pila de llamadas.

Cuando una excepción se propaga, cada método abandona su ejecución inmediatamente sin reanudarse después. Esto incluye todas las funciones intermedias, que no continúan desde el punto en que la excepción las interrumpió. Si la excepción llega hasta `main` y nadie la captura, el programa termina, mostrando un mensaje de error.

---

## 5. ¿Qué ventajas tiene frente a C la *"propagación natural"* de las excepciones a través de la pila?

### Respuesta
Una ventaja importante es la eliminación de comprobaciones manuales constantes en el código. En C, cada llamada a una función debe ir seguida de una comprobación de error, lo cual genera código repetitivo y propenso a olvidos. Con excepciones, si algo falla, la propagación automática asegura que el error no pase desapercibido.

Otra ventaja es que se puede centralizar el manejo de errores en un único punto del programa. En lugar de dispersar comprobaciones por todo el código, se pueden agrupar todos los tratamientos de errores en lugares estratégicos. De esta manera, las funciones se mantienen más limpias y centradas en su responsabilidad principal, dejando la lógica de tratamiento de errores para niveles superiores donde tenga más sentido.

---

## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto? ¿Podemos crear excepciones personalizadas?

### Respuesta
En orientación a objetos, las excepciones se representan típicamente como *objetos*. Esto permite encapsular en un único elemento información útil asociada al error, como el mensaje descriptivo, la causa original y la traza de la pila donde ocurrió. Gracias a este enfoque, el manejo de errores se integra naturalmente con el resto del sistema orientado a objetos.

El hecho de que las excepciones sean objetos permite crear tipos de excepciones personalizadas mediante clases propias. Estas pueden añadir información relevante al dominio del problema, permitiendo manejar los errores de una manera más específica. Esto otorga gran flexibilidad al programador, que puede definir categorías de errores claras y adaptadas a las necesidades del proyecto.

---

## 7. ¿Qué *información esencial* lleva cualquier objeto excepción que es muy útil al llegar a un manejador?

### Respuesta
Una excepción contiene un mensaje textual que describe la causa del error. Este mensaje resulta fundamental para entender rápidamente qué ha fallado. Además, incluye una *traza de la pila* que refleja el camino de llamadas que condujo a la excepción, lo que facilita el diagnóstico y la depuración del programa.

Además del mensaje y la traza, una excepción puede contener otra excepción como *causa*. Esto permite que errores de bajo nivel se conserven dentro de otros más generales, sin perder el contexto original. De esta forma, se puede rastrear el motivo interno del fallo incluso si la excepción fue encapsulada varias veces.

---

## 8. En Java, sobre el bloque *try-catch*, ¿se pueden tener más de un catch? ¿cuántos se ejecutan?

### Respuesta
En Java es posible colocar varios bloques `catch` junto a un mismo `try`, cada uno orientado a tratar un tipo particular de excepción. Esto permite diferenciar el comportamiento dependiendo del tipo de fallo que ocurra. Es habitual ordenar los bloques `catch` desde tipos más específicos hasta tipos más generales.

Aunque haya varios bloques `catch`, solo *uno* de ellos se ejecuta. Java selecciona el primer bloque compatible con el tipo de excepción lanzado, ignorando los demás. Esto garantiza que el tratamiento del error sea claro, determinista y no ambiguo.

---

## 9. ¿Cómo garantizar que se ejecuta siempre código necesario? Ejemplo con *finally*

### Respuesta
El bloque `finally` garantiza que un código se ejecuta siempre, independientemente de si se lanza una excepción o no durante la ejecución del bloque `try`. Esto resulta indispensable para liberar recursos, cerrar ficheros o realizar tareas de limpieza que no deberían omitirse aunque ocurra un fallo.

El bloque `finally` puede acompañar a un `catch` o puede colocarse directamente tras un `try` sin necesidad de incluir un manejador. En ambos casos, Java asegura su ejecución incluso si dentro de `try` se ejecuta un `return`. Únicamente fallos externos como apagados abruptos impedirían su ejecución.

```java
try {
    FileReader f = new FileReader("datos.txt");
} catch (IOException e) {
    System.out.println("No se pudo abrir");
} finally {
    System.out.println("Cerrando recursos...");
}

try {
    int x = 10 / 2;
} finally {
    System.out.println("Esto siempre se ejecuta");
}
```
***

## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?
### Respuesta
El bloque `finally` puede aparecer sin necesidad de un bloque `catch`. Esto permite ejecutar código que debe realizarse siempre, incluso si el método no desea capturar la excepción. De este modo, `finally` se usa para operaciones críticas como liberar recursos, cerrar ficheros o restablecer estados internos del programa.

El contenido de `finally` se ejecuta ocurra o no ocurra una excepción dentro del bloque `try`. Incluso si el bloque try contiene un `return`, el código de finally se ejecuta antes de que el método devuelva el control al llamador. Solo circunstancias extremas, como una detención abrupta del proceso, impedirían su ejecución.

---

## 11. En Java, qué son las excepciones *"controladas"* y las *"no controladas"*? ¿Qué papel juega `RuntimeException`?
### Respuesta
Las excepciones *controladas* son las que el compilador exige manejar mediante `try-catch` o declarar explícitamente con `throws`. Representan situaciones esperables que pueden recuperarse, como fallos de E/S o problemas con recursos externos. Su uso obliga a los desarrolladores a considerar el manejo de estos fallos de manera explícita.

Las excepciones *no controladas* derivan de `RuntimeException`. Estas excepciones no requieren declaración ni captura obligatoria y suelen emplearse para errores de programación, como argumentos inválidos o incoherencias lógicas. Su papel consiste en simplificar la escritura de métodos, evitando llenar el código con manejos obligatorios para errores que deberían evitarse mediante validación.

---

## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?
### Respuesta
La palabra clave `throws` en la firma de un método indica que el método puede lanzar una excepción controlada y que no desea manejarla localmente. Esta declaración transfiere la responsabilidad al método llamador, que deberá capturar la excepción o volver a declararla. Así se permite estructurar el manejo de errores en niveles superiores con más contexto.

Usar `throws` es una alternativa a incluir un bloque `try-catch` dentro del propio método. Esto evita mezclar la lógica principal con la gestión del error cuando la función no tiene capacidad de actuar sobre él. De esta manera, el código se mantiene más limpio y centrado en su objetivo.

---

## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa manejar la excepción
### Respuesta
```java
void abrir() throws IOException {
    FileReader f = null;
    try {
        f = new FileReader("datos.txt");
    } finally {
        if (f != null) {
            System.out.println("Finalizando intento de apertura");
        }
    }
} 
```
***

## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch`? ¿Qué sentido tendría?
### Respuesta
Es posible incluir excepciones no controladas, como las que derivan de `RuntimeException`, dentro de la cláusula `throws`. Sin embargo, hacerlo no es necesario en Java, ya que estas excepciones no requieren ser declaradas ni capturadas de forma obligatoria. Su inclusión en la firma de un método suele tener únicamente un propósito documental, indicando que el método podría fallar en determinadas circunstancias.

El método llamador no está obligado a incluir bloques `try-catch` para este tipo de excepciones. De hecho, su manejo explícito suele ser poco habitual, ya que las excepciones no controladas representan normalmente errores de programación o condiciones que no deberían recuperarse dinámicamente. En la mayoría de los casos, la solución apropiada es corregir el código que provoca la excepción en lugar de interceptarla en tiempo de ejecución.

---

## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones?
### Respuesta
Las excepciones controladas se recomiendan cuando la situación de error es previsible y puede gestionarse razonablemente por el código llamador. Esto ocurre, por ejemplo, en operaciones de entrada/salida, manejo de ficheros o comunicación con sistemas externos. En estos casos, obligar al manejo forzoso ayuda a escribir código más robusto, consciente de los posibles fallos derivados del entorno.

Las excepciones no controladas son adecuadas cuando el error representa un fallo lógico o de programación, como pasar un valor inválido o violar una condición preestablecida. En tales casos, no tiene sentido obligar al desarrollador a capturar la excepción, ya que el problema debe corregirse directamente en el código.

No todos los lenguajes distinguen entre excepciones controladas y no controladas. En aquellos donde solo existe un tipo de excepción, lo más habitual es un modelo similar al de las no controladas de Java: el programador decide libremente cuándo capturar la excepción y cuándo permitir que se propague.

---

## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacerlo?
### Respuesta
Lanzar una excepción dentro de un bloque `catch` puede tener sentido cuando se desea transformar un error bajo en otro más abstracto o adaptado al contexto de nivel superior. Este patrón permite encapsular la excepción original en otra más clara, permitiendo que el resto del sistema entienda el tipo de problema sin exponer detalles técnicos innecesarios.

También es posible relanzar la misma excepción capturada cuando el método actual no puede resolverla, pero desea realizar alguna acción útil antes de que continúe propagándose, como registrar información de diagnóstico. Este enfoque conserva la información completa del fallo original, permitiendo que niveles superiores decidan cómo manejarlo.

```java
catch (IOException e) {
    throw e; // relanzar la misma excepción
}

catch (SQLException e) {
    throw new Exception("Fallo en base de datos", e); // nueva excepción con causa
}
```
***


## 17. ¿En qué consiste que una excepción sea la *"causa"* de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?
### Respuesta
Que una excepción sea la *causa* de otra significa que el error original se encapsula dentro de una nueva excepción de nivel superior. Esto permite comunicar el fallo a capas externas del programa utilizando un mensaje más apropiado para ese nivel, sin perder la información técnica del error interno. De esta forma, se abstrae el detalle del fallo concreto, pero se preserva la posibilidad de analizarlo en profundidad cuando sea necesario.

Este mecanismo permite construir cadenas de excepciones en las que cada una representa un nivel distinto de abstracción del error. Al imprimir la excepción, la traza muestra no solo el mensaje de la excepción externa, sino también la traza completa de la excepción interna. Esto facilita seguir la ruta exacta del fallo, desde el problema original hasta la capa donde fue finalmente detectado o comunicado.

```java
try {
    leerBD();  // puede lanzar SQLException
} catch (SQLException e) {
    throw new MiExcepcion("Error de acceso a base de datos", e);
}
```
***

