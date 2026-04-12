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

## 1. En C, podemos crear estructuras mayores componiendo unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.
### Respuesta
Descripción:
- Definimos `struct Punto { double x, y; }`.
- Definimos `struct Linea { Punto a, b; }`.
- Función `distancia(Punto p1, Punto p2)` devuelve la distancia euclídea.
- Función `longitud(Linea l)` devuelve la longitud como distancia entre sus extremos.

-- CODIGO (C) --
#include <math.h>

typedef struct {
    double x;
    double y;
} Punto;

typedef struct {
    Punto a;
    Punto b;
} Linea;

double distancia(Punto p1, Punto p2) {
    double dx = p2.x - p1.x;
    double dy = p2.y - p1.y;
    return sqrt(dx*dx + dy*dy);
}

double longitud(Linea l) {
    return distancia(l.a, l.b);
}

/* Ejemplo de uso (opcional):
#include <stdio.h>
int main(void) {
    Punto p1 = {0.0, 0.0};
    Punto p2 = {3.0, 4.0};
    Linea l = {p1, p2};
    printf("distancia = %.2f\n", distancia(p1, p2));
    printf("longitud  = %.2f\n", longitud(l));
    return 0;
}
*/
-- FIN CODIGO --

---

## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de composición en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.
### Respuesta
Descripción:
- `Punto` y `Linea` son inmutables (campos `final`, sin setters).
- `Punto.distanciaA(Punto otro)` usa `Math.hypot`.
- `Linea.longitud()` delega en `Punto.distanciaA`.

-- CODIGO (Java) --
final class Punto {
    private final double x;
    private final double y;

    Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    double getX() { return x; }
    double getY() { return y; }

    double distanciaA(Punto otro) {
        return Math.hypot(otro.x - this.x, otro.y - this.y);
    }
}

final class Linea {
    private final Punto a;
    private final Punto b;

    Linea(Punto a, Punto b) {
        if (a == null || b == null) throw new IllegalArgumentException("Puntos no pueden ser null");
        this.a = a;
        this.b = b;
    }

    Punto getA() { return a; }
    Punto getB() { return b; }

    double longitud() {
        return a.distanciaA(b);
    }
}

class Demo {
    public static void main(String[] args) {
        Punto p1 = new Punto(0, 0);
        Punto p2 = new Punto(3, 4);
        Linea l = new Linea(p1, p2);
        System.out.println(l.longitud()); // 5.0
    }
}
-- FIN CODIGO --

---

## 3. ¿Qué significa la multiplicidad en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.
### Respuesta
La multiplicidad indica cuántas instancias de una clase están relacionadas con otra.

- De `Linea` a `Punto`: **exactamente 2** (`2`).
- De `Punto` a `Linea`:
  - En **composición fuerte**: **exactamente 1** (`1`) si modelamos que cada `Punto` solo existe dentro de una `Linea`.
  - En **agregación (débil)**: **0..***, un `Punto` podría pertenecer a ninguna o a múltiples `Linea`.

---

## 4. ¿Qué significa composición fuerte y composición débil? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como "asociación o agregación" y a cuál como "composición" propiamente.
### Respuesta
- **Composición fuerte (composición propiamente)**: el contenedor **posee** las partes y su **ciclo de vida está ligado**; si el contenedor deja de existir, sus partes también.
- **Composición débil (asociación/aggregación)**: el contenedor **referencia** las partes, pero **no controla** su ciclo de vida; las partes existen independientemente.

Consecuencia: en fuerte, hay una dependencia vital; en débil, solo una relación de uso/tenencia sin dependencia de existencia.

---

## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de "dependencia"?
### Respuesta
Eso es **dependencia** (o asociación de uso temporal), no composición. La composición implica que el objeto contenedor **mantiene** y **gestiona** las partes como parte de su estado.

---

## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una como composición fuerte, donde el ciclo de vida de los puntos está ligado al de Linea y otra como composición débil, donde no.
### Respuesta
Composición fuerte: `Linea` **crea internamente** sus `Punto` y no permite sustituirlos.

-- CODIGO (Java) --
final class LineaFuerte {
    private final Punto a;
    private final Punto b;

    LineaFuerte(double x1, double y1, double x2, double y2) {
        this.a = new Punto(x1, y1);
        this.b = new Punto(x2, y2);
    }

    Punto getA() { return a; }
    Punto getB() { return b; }
    double longitud() { return a.distanciaA(b); }
}
-- FIN CODIGO --

Composición débil (agregación): `Linea` **recibe** `Punto` ya existentes (compartibles).

-- CODIGO (Java) --
final class LineaDebil {
    private final Punto a;
    private final Punto b;

    LineaDebil(Punto a, Punto b) {
        if (a == null || b == null) throw new IllegalArgumentException("Puntos no pueden ser null");
        this.a = a;
        this.b = b;
    }

    Punto getA() { return a; }
    Punto getB() { return b; }
    double longitud() { return a.distanciaA(b); }
}
-- FIN CODIGO --

---

## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?
### Respuesta
En Java **no destruimos explícitamente**; el **garbage collector** libera memoria cuando **no quedan referencias alcanzables** a un objeto. Por eso, aunque `Linea` sea el único que referencia sus `Punto`, al desaparecer la `Linea` (o perder sus referencias), los `Punto` quedan **candidatos a recolección** automáticamente. No hay `delete` ni gestión manual.

---

## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.
### Respuesta
Descripción de invariantes:
- Capacidad máxima 50.
- Siempre existe director y **debe pertenecer** al departamento.
- No exponer la estructura interna (no devolver el array).

-- CODIGO (Java) --
final class Profesor {
    private final String nombre;
    Profesor(String nombre) {
        if (nombre == null || nombre.isBlank()) throw new IllegalArgumentException("Nombre requerido");
        this.nombre = nombre;
    }
    String getNombre() { return nombre; }
}

final class Departamento {
    private static final int MAX = 50;

    private final String nombre;
    private final Profesor[] profesores = new Profesor[MAX];
    private int size = 0;
    private Profesor director;

    Departamento(String nombre, Profesor directorInicial) {
        if (nombre == null || nombre.isBlank()) throw new IllegalArgumentException("Nombre depto requerido");
        if (directorInicial == null) throw new IllegalArgumentException("Director inicial requerido");
        this.nombre = nombre;
        // agregamos director como primer profesor
        this.profesores[size++] = directorInicial;
        this.director = directorInicial;
        // invariante: director ∈ profesores
    }

    String getNombre() { return nombre; }

    int getNumProfesores() {
        return size;
    }

    Profesor getProfesor(int pos) {
        if (pos < 0 || pos >= size) throw new IndexOutOfBoundsException("Pos fuera de rango");
        return profesores[pos];
    }

    void addProfesor(Profesor p) {
        if (p == null) throw new IllegalArgumentException("Profesor null");
        if (size >= MAX) throw new IllegalStateException("Capacidad máxima alcanzada");
        profesores[size++] = p;
    }

    void removeProfesor(int pos) {
        if (pos < 0 || pos >= size) throw new IndexOutOfBoundsException("Pos fuera de rango");
        Profesor aEliminar = profesores[pos];
        // no se puede eliminar al director sin cambiarlo antes
        if (aEliminar == director) {
            throw new IllegalStateException("No puedes eliminar al director actual; cámbialo antes");
        }
        // compactar array
        for (int i = pos; i < size - 1; i++) {
            profesores[i] = profesores[i + 1];
        }
        profesores[--size] = null;
        // invariante sigue cumpliéndose porque director no ha sido eliminado
    }

    Profesor getDirector() { return director; }

    void setDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) throw new IllegalArgumentException("Director null");
        // verificar que nuevoDirector ya pertenece al departamento
        boolean pertenece = false;
        for (int i = 0; i < size; i++) {
            if (profesores[i] == nuevoDirector) { pertenece = true; break; }
        }
        if (!pertenece) throw new IllegalStateException("El director debe ser profesor del departamento");
        this.director = nuevoDirector;
    }
}
-- FIN CODIGO --

---

## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?
### Respuesta
Con `List` evitamos:
- Gestión manual de capacidad y `size`.
- Compactación al eliminar (no hay bucle de desplazamiento).
- Comprobaciones de índice personalizadas (List ya las hace).

Problema de devolver la lista interna: **rompe la encapsulación**; el cliente podría modificar el contenido saltándose invariantes. Solución: devolver **vista inmutable** o **copia defensiva**.

-- CODIGO (Java) --
import java.util.*;

final class DepartamentoList {
    private final String nombre;
    private final List<Profesor> profesores = new ArrayList<>();
    private Profesor director;

    DepartamentoList(String nombre, Profesor directorInicial) {
        if (nombre == null || nombre.isBlank()) throw new IllegalArgumentException("Nombre depto requerido");
        if (directorInicial == null) throw new IllegalArgumentException("Director inicial requerido");
        this.nombre = nombre;
        profesores.add(directorInicial);
        this.director = directorInicial;
    }

    int getNumProfesores() { return profesores.size(); }

    Profesor getProfesor(int pos) { return profesores.get(pos); }

    void addProfesor(Profesor p) {
        if (p == null) throw new IllegalArgumentException("Profesor null");
        profesores.add(p);
    }

    void removeProfesor(int pos) {
        Profesor aEliminar = profesores.get(pos);
        if (aEliminar == director) {
            throw new IllegalStateException("No puedes eliminar al director actual; cámbialo antes");
        }
        profesores.remove(pos);
    }

    Profesor getDirector() { return director; }

    void setDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) throw new IllegalArgumentException("Director null");
        if (!profesores.contains(nuevoDirector)) {
            throw new IllegalStateException("El director debe ser profesor del departamento");
        }
        this.director = nuevoDirector;
    }

    // Opción A: vista inmutable
    List<Profesor> verProfesores() {
        return Collections.unmodifiableList(profesores);
    }

    // Opción B: copia defensiva
    List<Profesor> copiarProfesores() {
        return new ArrayList<>(profesores);
    }
}
-- FIN CODIGO --

---

## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.
### Respuesta
Descripción:
- `Persona` inmutable con campos `nombre` y `madre` (opcional).
- Cadena madre → abuela → bisabuela… (recursión).
- Ejemplos clásicos: árbol de carpetas, árbol binario, AST de expresiones, lista enlazada, menús con submenús.

-- CODIGO (Java) --
import java.util.Optional;

final class Persona {
    private final String nombre;
    private final Optional<Persona> madre;

    Persona(String nombre, Optional<Persona> madre) {
        if (nombre == null || nombre.isBlank()) throw new IllegalArgumentException("Nombre requerido");
        this.nombre = nombre;
        this.madre = madre == null ? Optional.empty() : madre;
    }

    String getNombre() { return nombre; }
    Optional<Persona> getMadre() { return madre; }

    String lineaMaterna() {
        StringBuilder sb = new StringBuilder(nombre);
        Optional<Persona> actual = madre;
        while (actual.isPresent()) {
            sb.append(" <- ").append(actual.get().nombre);
            actual = actual.get().madre;
        }
        return sb.toString();
    }
}

class DemoFamilia {
    public static void main(String[] args) {
        Persona abuela = new Persona("Abuela", Optional.empty());
        Persona madre  = new Persona("Madre", Optional.of(abuela));
        Persona nieto  = new Persona("Nieto", Optional.of(madre));
        System.out.println(nieto.lineaMaterna()); // Nieto <- Madre <- Abuela
    }
}
-- FIN CODIGO --

---

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?
### Respuesta
Una composición (o agregación) **bidireccional** mantiene **referencias en ambos sentidos** y exige **consistencia**:
- `Departamento` conoce a sus `Profesor`.
- Cada `Profesor` conoce su `Departamento`.
Para implementarlo:
1) Centraliza la mutación en una de las clases (p.ej. `Departamento`) para evitar estados inconsistentes.
2) Al **añadir** un profesor, asigna también su `departamento`.
3) Al **eliminar** un profesor, pon su `departamento` a `null`.
4) Controla la invariante del director.

-- CODIGO (Java) --
import java.util.*;

final class ProfesorBidi {
    private final String nombre;
    private DepartamentoBidi departamento; // referencia inversa

    ProfesorBidi(String nombre) {
        if (nombre == null || nombre.isBlank()) throw new IllegalArgumentException("Nombre requerido");
        this.nombre = nombre;
    }

    String getNombre() { return nombre; }
    DepartamentoBidi getDepartamento() { return departamento; }

    // Solo el Departamento debe llamar a este método (no exponer públicamente en APIs reales)
    void setDepartamentoInterno(DepartamentoBidi d) { this.departamento = d; }
}

final class DepartamentoBidi {
    private final String nombre;
    private final List<ProfesorBidi> profesores = new ArrayList<>();
    private ProfesorBidi director;

    DepartamentoBidi(String nombre, ProfesorBidi directorInicial) {
        if (nombre == null || nombre.isBlank()) throw new IllegalArgumentException("Nombre depto requerido");
        if (directorInicial == null) throw new IllegalArgumentException("Director inicial requerido");
        this.nombre = nombre;
        addProfesor(directorInicial);
        setDirector(directorInicial);
    }

    void addProfesor(ProfesorBidi p) {
        if (p == null) throw new IllegalArgumentException("Profesor null");
        if (p.getDepartamento() != null && p.getDepartamento() != this) {
            throw new IllegalStateException("El profesor ya pertenece a otro departamento");
        }
        if (!profesores.contains(p)) {
            profesores.add(p);
            p.setDepartamentoInterno(this);
        }
    }

    void removeProfesor(ProfesorBidi p) {
        if (p == null) throw new IllegalArgumentException("Profesor null");
        if (p == director) throw new IllegalStateException("No puedes eliminar al director actual");
        if (profesores.remove(p)) {
            p.setDepartamentoInterno(null);
        }
    }

    void setDirector(ProfesorBidi nuevoDirector) {
        if (nuevoDirector == null) throw new IllegalArgumentException("Director null");
        if (!profesores.contains(nuevoDirector)) {
            throw new IllegalStateException("El director debe ser profesor del departamento");
        }
        this.director = nuevoDirector;
    }

    ProfesorBidi getDirector() { return director; }
    List<ProfesorBidi> verProfesores() { return Collections.unmodifiableList(profesores); }
}
-- FIN CODIGO --
