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
# TEMA 7

## 1. ¿Qué es un puntero a una función?

Un puntero a función es una variable que almacena la dirección de memoria de una función. Esto permite invocar funciones de forma dinámica, pasarlas como parámetros o almacenarlas en estructuras.

En C es necesario definir el tipo exacto del puntero, que debe coincidir con la firma de la función.

```c
#include <stdio.h>
#include <ctype.h>

char* convertirMayusculas(char* texto) {
    for (int i = 0; texto[i] != '\0'; i++) {
        texto[i] = toupper(texto[i]);
    }
    return texto;
}

int main() {
    char texto[] = "hola";

    char* (*aMayusculas)(char*);
    aMayusculas = convertirMayusculas;

    printf("%s\n", aMayusculas(texto));

    return 0;
}
```

---

## 2. Funciones lambda

Una función lambda es una función anónima que puede almacenarse en variables, pasarse como parámetro o devolverse desde otras funciones.

En JavaScript son ciudadanos de primera clase:

```javascript
let aMayusculas = (texto) => {
    return texto.toUpperCase();
};

console.log(aMayusculas("hola"));
```

En Java se usa `Function`:

```java
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        Function<String, String> aMayusculas =
            texto -> texto.toUpperCase();

        System.out.println(aMayusculas.apply("hola"));
    }
}
```

---

## 3. Paradigma funcional

El paradigma funcional se basa en el uso de funciones como elemento principal del programa, evitando estado mutable y efectos secundarios.

Java es multi-paradigma porque combina programación orientada a objetos con programación funcional desde Java 8.

Las funciones son ciudadanos de primera clase porque pueden almacenarse, pasarse como parámetros o devolverse como resultado.

---

## 4. Sintaxis de lambdas

La sintaxis básica es:

```java
(parametros) -> expresion
```

o

```java
(parametros) -> {
    instrucciones;
}
```

Ejemplos:

```java
x -> x * 2
(a, b) -> a + b
```


---

## 5. Función como parámetro

Permite pasar funciones a métodos para modificar comportamiento dinámicamente.

JavaScript:

```javascript
function transformar(texto, funcion) {
    return funcion(texto);
}

let aMayusculas = texto => texto.toUpperCase();

console.log(transformar("hola", aMayusculas));
```


Java:

```java
import java.util.function.Function;

public class Main {
    public static String transformar(String texto, Function<String, String> funcion) {
        return funcion.apply(texto);
    }

    public static void main(String[] args) {
        Function<String, String> aMayusculas =
            s -> s.toUpperCase();

        System.out.println(transformar("hola", aMayusculas));
    }
}
```
``

---

## 6. Lambda inline

JavaScript:

```javascript
console.log(
    transformar("hola",
        texto => texto.split("")
        .reverse()
        .join("")
    )
);
```


Java:

```java
System.out.println(
    transformar(
        "hola",
        texto -> new StringBuilder(texto).reverse().toString()
    )
);
```

---

## 7. Closures

Un closure es una función que recuerda el entorno donde fue creada.

Java permite capturar variables externas si son effectively final.

```java
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        String sufijo = "!!!";

        Function<String, String> transformar =
            texto -> texto + sufijo;

        System.out.println(transformar.apply("Hola"));
    }
}
```

---

## 8. Diferencia con punteros a función

Un puntero a función en C solo guarda una dirección de función.

Una lambda puede capturar contexto (closures), lo que la hace más potente.

---

## 9. Funciones que devuelven funciones

```java
import java.util.function.Function;

public class Main {

    public static Function<Double, Double> crearDescuento(double porcentaje) {
        return precio -> precio - precio * porcentaje / 100;
    }

    public static void main(String[] args) {
        Function<Double, Double> desc10 = crearDescuento(10);
        Function<Double, Double> desc25 = crearDescuento(25);

        System.out.println(desc10.apply(100.0));
        System.out.println(desc25.apply(100.0));
    }
}
```

---

## 10. Interfaz funcional

Una interfaz funcional tiene un único método abstracto.

```java
@FunctionalInterface
public interface Transformador {
    String transformar(String s);
}
```

---

## 11. Interfaz funcional propia

```java
@FunctionalInterface
public interface Transformador {
    String transformar(String texto);
}
```

---

## 12. Genéricos en interfaces funcionales

```java
@FunctionalInterface
public interface Transformador<T, R> {
    R transformar(T valor);
}
```
``

```java
Transformador<Double, Integer> redondear =
    x -> (int) Math.round(x);

System.out.println(redondear.transformar(3.8));
```

---

## 13. Interfaces funcionales de Java

- Function<T, R>
- Consumer<T>
- Supplier<T>
- Predicate<T>
- UnaryOperator<T>
- BinaryOperator<T>

---

## 14. forEach funcional

```java
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<Integer> numeros = List.of(3, -2, 7, -1);

        numeros.forEach(n -> {
            if (n > 0) {
                System.out.println(n + " es positivo");
            }
        });
    }
}
```

---

## 15. PECS

PECS:
- Producer Extends
- Consumer Super

```java
import java.util.List;

public static double suma(List<? extends Number> lista) {
    double total = 0;
    for (Number n : lista) {
        total += n.doubleValue();
    }
    return total;
}
```


```java
public static void añadir(List<? super Integer> lista) {
    lista.add(10);
    lista.add(20);
}
```

---

## 16. Referencias a métodos

JavaScript:

```javascript
class Persona {
    constructor(nombre) {
        this.nombre = nombre;
    }

    saludar() {
        console.log("Hola soy " + this.nombre);
    }
}

let p = new Persona("Ana");
let saludo = p.saludar.bind(p);

saludo();
```

Java:

```java
public class Persona {
    private String nombre;

    public Persona(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola soy " + nombre);
    }
}

public class Main {
    public static void main(String[] args) {
        Persona p = new Persona("Ana");

        Runnable saludo = p::saludar;
        saludo.run();
    }
}
```

---

## 17. Tipos de referencias a método

```java
Function<String, Integer> f1 = Integer::parseInt;
Supplier<StringBuilder> f2 = StringBuilder::new;

String texto = "hola";
Supplier<String> f3 = texto::toUpperCase;

Function<String, String> f4 = String::toUpperCase;
```

---

## 18. Ordenación con lambda y Comparator

Manual:

```java
Collections.sort(personas, (p1, p2) -> {
    if (p1.getEdad() != p2.getEdad()) {
        return p1.getEdad() - p2.getEdad();
    }
    return p1.getNombre().compareTo(p2.getNombre());
});
```

Con Comparator:

```java
Collections.sort(
    personas,
    Comparator.comparing(Persona::getEdad)
        .thenComparing(Persona::getNombre)
);
```
