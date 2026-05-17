<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Genericidad". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia y polimorfismo.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 6. Genericidad

## 1. Empleando void* en C o Object en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

En C, el tipo `void*` permite almacenar la dirección de memoria de cualquier tipo de dato. Gracias a ello, se pueden construir estructuras de datos genéricas, aunque el lenguaje no conozca el tipo concreto almacenado. Un ejemplo típico es una lista basada en un array de punteros `void*`.

Esto permite almacenar distintos tipos de datos en la misma estructura, pero perdiendo el control de tipos en compilación, lo que obliga a hacer conversiones manuales en ejecución.

```c
#include <stdio.h>

typedef struct {
    void* elementos[10];
    int numElementos;
} Lista;

int main() {
    int x = 5;
    double y = 3.14;

    Lista lista;
    lista.numElementos = 0;

    lista.elementos[lista.numElementos++] = &x;
    lista.elementos[lista.numElementos++] = &y;

    printf("%d\n", *(int*)lista.elementos[0]);
    printf("%f\n", *(double*)lista.elementos[1]);

    return 0;
}
```


En Java ocurre algo parecido usando `Object`, ya que todas las clases heredan de Object.

```java
public class Lista {
    private Object[] elementos = new Object[10];
    private int numElementos = 0;

    public void añadir(Object obj) {
        elementos[numElementos++] = obj;
    }

    public Object get(int i) {
        return elementos[i];
    }

    public static void main(String[] args) {
        Lista lista = new Lista();
        lista.añadir("Hola");
        lista.añadir(25);

        String texto = (String) lista.get(0);
        int numero = (Integer) lista.get(1);

        System.out.println(texto);
        System.out.println(numero);
    }
}
```


---

## 2. Programación genérica

La programación genérica consiste en escribir código reutilizable capaz de trabajar con distintos tipos de datos sin duplicar estructuras. En lugar de crear versiones para cada tipo, se define una única estructura genérica.

El ejemplo anterior es una forma básica de genericidad, pero insegura, porque no hay control de tipos en compilación.

La programación genérica moderna (Java Generics / C++ Templates) soluciona esto introduciendo seguridad de tipos en compilación.

---

## 3. Problemas de void* y Object

El principal problema es la pérdida del chequeo estático de tipos. El compilador no sabe qué tipo contiene la estructura.

Esto provoca errores en ejecución y obliga a hacer casts manuales.

```c
double y = *(double*)lista.elementos[0];


Si el tipo es incorrecto, el comportamiento es indefinido.

En Java:

```java
String texto = (String) lista.get(0);


Si el tipo no coincide, aparece `ClassCastException`.

---

## 4. Parámetros de tipo

Los parámetros de tipo permiten definir clases genéricas usando identificadores como `T`, `E`, `K`, etc.

```java
public class Lista<T> {
    private T[] elementos;

    public T get(int i) {
        return elementos[i];
    }
}
```

Esto permite que el compilador conozca el tipo real y evite casts.

---

## 5. Generics en Java y templates en C++

En Java:

```java
import java.util.ArrayList;

public class Main {
    public static void main(String[] args) {
        ArrayList<String> nombres = new ArrayList<>();
        nombres.add("Ana");
        nombres.add("Luis");

        for (String s : nombres) {
            System.out.println(s.toUpperCase());
        }
    }
}
```


En C++:

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<string> nombres;
    nombres.push_back("Ana");
    nombres.push_back("Luis");

    for (string s : nombres) {
        cout << s << endl;
    }
}
```


---

## 6. Funcionamiento de generics

Java usa **type erasure**, eliminando los tipos genéricos en compilación.

```java
List<String> lista;
```

Se convierte en:

```java
List lista;
```

C++ usa **instanciación de plantillas**, generando código distinto para cada tipo:

- vector<int>
- vector<string>

---

## 7. Clase genérica Par

```java
public class Par<A, B> {
    private A primero;
    private B segundo;

    public Par(A primero, B segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public A getPrimero() {
        return primero;
    }

    public B getSegundo() {
        return segundo;
    }
}
```
```

Ejemplo:

```java
public class Main {

    public static Par<Double, Double> calcular(double[] datos) {
        double suma = 0;

        for (double d : datos) {
            suma += d;
        }

        double media = suma / datos.length;

        double suma2 = 0;
        for (double d : datos) {
            suma2 += Math.pow(d - media, 2);
        }

        double desviacion = Math.sqrt(suma2 / datos.length);

        return new Par<>(media, desviacion);
    }
}
```


---

## 8. Métodos genéricos

Sin generics:

```java
public static Object seleccionaUno(Object a, Object b) {
    return Math.random() < 0.5 ? a : b;
}
```
```

Con generics:

```java
public static <T> T seleccionaUno(T a, T b) {
    return Math.random() < 0.5 ? a : b;
}
```

---

## 9. Restricciones en generics

```java
public class Punto<T extends Number> {
    private T x;
    private T y;

    public Punto(T x, T y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaA(Punto<T> p) {
        double dx = x.doubleValue() - p.x.doubleValue();
        double dy = y.doubleValue() - p.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```


Tras compilación (type erasure), `T` se sustituye por `Number`.

---

## 10. Tipos Number vs generics

Sin generics:

```java
Punto p = new Punto(5, 3.2);
```


Con generics:

```java
Punto<Integer> p = new Punto<>(2, 4);
```


El método `getX()` devuelve `Number` sin generics y `T` con generics.

---

## 11. Genericidad avanzada con interfaces

```java
public interface Punto<T extends Punto<T>> {
    double distanciaA(T p);
}
```

```java
public class Punto2D implements Punto<Punto2D> {
    public double distanciaA(Punto2D p) {
        return 0;
    }
}
```


---

## 12. Covarianza e invarianza

`List<String>` NO es subtipo de `List<Object>`.

Los arrays sí son covariantes:

```java
Object[] arr = new String[2];
```


Pero puede fallar en runtime:

```java
arr[0] = 5; // ArrayStoreException
```


---

## 13. Wildcards

Covarianza:

```java
public static double suma(List<? extends Number> lista) {
    double total = 0;
    for (Number n : lista) {
        total += n.doubleValue();
    }
    return total;
}
```


Contravarianza:

```java
public static void añadir(List<? super Integer> lista) {
    lista.add(10);
    lista.add(20);
}
```
