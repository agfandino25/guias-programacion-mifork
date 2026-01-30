<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Clases y Objetos". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: ninguno.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

### Respuesta# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una
### Respuesta
Las cuatro características fundamentales de la programación orientada a objetos son *abstracción*, *encapsulación*, *herencia* y *polimorfismo*. La *abstracción* consiste en representar conceptos complejos del mundo real mediante modelos simplificados que recogen únicamente la información relevante para el problema. Esto permite trabajar con objetos sin conocer en detalle cómo están implementados internamente.

La *encapsulación* agrupa datos y comportamiento dentro de una clase y limita el acceso a información interna mediante niveles de visibilidad. De esta forma, se protege la consistencia del estado interno del objeto y se reduce el acoplamiento con el código externo.

La *herencia* permite crear nuevas clases basadas en otras ya existentes, heredando atributos y métodos y permitiendo añadir o modificar comportamientos. Esto favorece la reutilización de código y la creación de jerarquías lógicas. Por último, el *polimorfismo* posibilita que diferentes clases respondan de forma distinta a un mismo método, permitiendo tratar objetos diversos bajo una interfaz común.

---

## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos
### Respuesta
Cuatro lenguajes populares que soportan la programación orientada a objetos son Java, C++, Python y C#. Java y C# ofrecen un enfoque orientado a objetos muy uniforme, con recolección automática de memoria y un diseño centrado en clases y objetos. Son especialmente utilizados en entornos empresariales y de desarrollo de aplicaciones complejas.

Python también admite orientación a objetos, aunque de forma más flexible y dinámica. Es posible combinar estilos funcionales, procedimentales y orientados a objetos sin demasiadas restricciones. C++, por su parte, ofrece un modelo mixto que combina varias filosofías de programación y aporta un control detallado sobre la memoria.

---

## 3. Los paradigmas anteriores a la POO: ¿Qué es la programación estructurada? y, todavía mejor, ¿Qué es la programación modular?
### Respuesta
La *programación estructurada* organiza el código en estructuras de control bien definidas como secuencia, selección e iteración, evitando el uso abusivo del salto incondicional. Este paradigma busca mejorar la claridad del código y facilitar el razonamiento sobre su ejecución, promoviendo programas más robustos y fáciles de depurar.

La *programación modular* amplía este enfoque dividiendo el programa en módulos independientes, cada uno con responsabilidades claramente definidas. En C, esto se refleja mediante archivos de cabecera y archivos de implementación. Esta organización permite mantener, extender y probar partes del sistema sin afectar al resto, aumentando la reutilización y reduciendo errores.

---

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?
### Respuesta
Un objeto se define principalmente por tres elementos: *identidad*, *estado* y *comportamiento*. La *identidad* distingue a un objeto de otro, incluso si sus valores internos coinciden. En lenguajes como Java, esta identidad se representa mediante la referencia al objeto.

El *estado* corresponde a los valores actuales de los atributos del objeto. Este estado cambia a lo largo del tiempo como consecuencia de las operaciones realizadas. El *comportamiento* está definido por los métodos del objeto, que permiten consultarlo, modificarlo o interactuar con otros objetos del sistema.

---

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?
### Respuesta
Una clase es un molde o plantilla que describe qué atributos y métodos tendrán los objetos creados a partir de ella. No se debe confundir una clase con un objeto: la clase es la definición, mientras que el objeto es una *instancia* concreta que ocupa memoria y posee un estado propio.

El término *instancia* se refiere precisamente a un objeto creado a partir de una clase. Aunque la mayoría de lenguajes orientados a objetos populares se basan en clases, no todos funcionan así. Existen lenguajes *prototipales*, como JavaScript, donde se crean objetos a partir de otros objetos sin necesidad de clases formales.

---

## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la recolección de basura?
### Respuesta
La ubicación en memoria depende del lenguaje. En Java, los objetos se crean normalmente en el *heap*, mientras que las variables locales y referencias viven en la *pila* de ejecución. En C++, en cambio, los objetos pueden situarse tanto en la pila como en el heap o tener almacenamiento estático, dependiendo de cómo se creen.

No todos los lenguajes manejan la memoria igual. La *recolección de basura* es un sistema automático que libera memoria que ya no está en uso, presente en lenguajes como Java o C#. En C y C++, esta tarea recae en el programador, lo que aporta más control pero también más riesgo de errores si no se maneja correctamente.

---

## 7. ¿Qué es un método? ¿Qué es la sobrecarga de métodos?
### Respuesta
Un método es una función asociada a una clase que define una operación disponible para sus objetos. Permite interactuar con el estado interno del objeto o realizar cálculos relacionados. En Java, los métodos son parte fundamental de la interfaz de una clase y determinan cómo se comportan sus instancias.

La *sobrecarga de métodos* consiste en definir varios métodos con el mismo nombre dentro de la misma clase, pero con diferentes listas de parámetros. El compilador selecciona automáticamente cuál debe ejecutarse según los argumentos proporcionados en cada llamada.

---

## 8. Ejemplo mínimo de clase en Java
### Respuesta
```java
// Punto.java
class Punto {
    double x;   // visibilidad por defecto
    double y;   // visibilidad por defecto

    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}

// Ejemplo de uso
public class Main {
    public static void main(String[] args) {
        Punto p = new Punto();
        p.x = 3.0;
        p.y = 4.0;
        double d = p.calculaDistanciaAOrigen();
        System.out.println("Distancia al origen: " + d);
    }
}


## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es static y para qué vale? ¿Sólo se emplea para ese método main? ¿Para qué se combina con final?
### Respuesta
El punto de entrada en un programa Java es el método `public static void main(String[] args)`. Esta firma concreta permite a la máquina virtual localizar el método sin necesidad de instanciar ningún objeto, ya que debe ejecutar el código inicial de forma independiente a las clases que intervengan después. La estructura del método es rígida porque la JVM lo invoca directamente.

La palabra clave *static* indica que un método o atributo pertenece a la clase y no a una instancia concreta. Esto permite utilizarlo sin necesidad de crear un objeto, lo que resulta esencial para el método `main`. Sin embargo, su uso no se limita a este caso: se emplea en utilidades, constantes de clase y en métodos que no dependen del estado de un objeto.

La combinación *static final* suele emplearse para definir constantes cuyo valor no debe modificarse. El modificador *final* garantiza que el valor asignado no cambie, y al combinarlo con *static* se consigue que la constante sea única y accesible desde cualquier parte sin necesidad de instanciar la clase.

---

## 10. Intenta ejecutar un poco de Java de forma básica… ¿Cómo podemos compilar y ejecutar el programa desde línea de comandos? ¿Java es compilado? ¿Qué es la máquina virtual? ¿Qué es el byte-code y los ficheros .class?
### Respuesta
Un programa Java se compila desde la línea de comandos utilizando `javac`. Por ejemplo, `javac Main.java Punto.java` genera los archivos `.class` correspondientes. Estos archivos contienen *bytecode*, que es un conjunto de instrucciones intermedias independientes de la plataforma. Para ejecutar el programa, se utiliza el comando `java Main`, sin la extensión `.class`.

Java puede considerarse un lenguaje compilado e interpretado. Primero se compila a *bytecode*, y luego la máquina virtual Java (JVM) interpreta o compila dinámicamente estas instrucciones a código nativo mediante optimizaciones como la compilación JIT (*Just In Time*). Esto permite la portabilidad del código entre diferentes sistemas operativos.

La *máquina virtual* actúa como intermediaria entre el programa y el sistema real. Gracias a ella, el mismo archivo `.class` puede ejecutarse en cualquier plataforma que tenga una JVM compatible. El bytecode es precisamente el formato portable que la JVM entiende y ejecuta, lo que diferencia a Java de otros lenguajes más acoplados al hardware.

---

## 11. En el código anterior de la clase Punto: ¿Qué es new? ¿Qué es un constructor? Ejemplo de constructor en una clase Empleado
### Respuesta
La palabra clave `new` sirve para crear una nueva instancia de una clase en el heap. Al ejecutar `new`, la JVM reserva memoria para el objeto y devuelve una referencia al mismo. Durante este proceso, se invoca automáticamente un *constructor*, que es un método especial encargado de inicializar el estado del objeto recién creado.

Un constructor tiene el mismo nombre que la clase y no especifica tipo de retorno. Puede recibir parámetros para inicializar los atributos del objeto de forma personalizada. Si no se declara ningún constructor, Java genera uno por defecto sin argumentos. A continuación se muestra un ejemplo:

```java
class Empleado {
    private final String dni;
    private String nombre;
    private String apellidos;

    public Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }
}
## 12. ¿Qué es la referencia this? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de this en la clase Punto
### Respuesta
La referencia *this* se utiliza dentro de los métodos y constructores para señalar al objeto actual. Cuando un atributo y un parámetro comparten el mismo nombre, *this* permite diferenciar entre ambos, evitando ambigüedades durante la asignación. También se emplea para acceder de forma explícita a métodos de la propia instancia.

Aunque el concepto existe en casi todos los lenguajes orientados a objetos, no siempre se llama igual. En C++ aparece como `this` pero es un puntero, mientras que en Python se utiliza el parámetro `self`, que debe escribirse de forma explícita en los métodos. En C#, igual que en Java, se mantiene el término `this`.

```java
class Punto {
    double x;
    double y;

    Punto(double x, double y) {
        this.x = x;  // evita confusión con el parámetro
        this.y = y;
    }
}


## 13. Añade ahora otro nuevo método que se llame distanciaA, que reciba un Punto como parámetro y calcule la distancia entre this y el punto proporcionado
### Respuesta
Para calcular la distancia entre dos puntos, se utiliza la fórmula de distancia euclídea en dos dimensiones. El método distanciaA recibe como parámetro otro objeto de tipo Punto y realiza la operación empleando sus coordenadas. El uso de *this* permite dejar claro que los valores de partida corresponden al objeto que invoca el método.

El método devuelve un valor numérico con la distancia resultante. Esta forma de modelar el comportamiento ayuda a encapsular una operación propia de los puntos en el plano, manteniendo el sentido de cohesión dentro de la clase.

```java
class Punto {
    double x;
    double y;

    double distanciaA(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}


## 14. El paso del Punto como parámetro a un método, ¿es por copia o por referencia?, ¿afectan los cambios al objeto fuera del método?, ¿qué ocurre con un entero?
### Respuesta
Java emplea siempre paso *por valor* para cualquier parámetro. En el caso de los objetos, lo que se pasa por valor es la referencia, no el objeto en sí. Esto implica que tanto el método como el código llamador comparten acceso al mismo objeto en memoria. Por ello, si dentro del método se modifican los atributos del objeto pasado como parámetro, estos cambios serán visibles fuera del método.

Cuando se pasan tipos primitivos como un *int*, el valor copiado es el dato literal. Al intentar modificar ese valor dentro del método, la variable original que está fuera no se ve afectada. De esta forma, los cambios realizados sobre parámetros primitivos quedan completamente aislados dentro del ámbito local del método.

---

## 15. ¿Qué es el método toString() en Java? ¿Existe en otros lenguajes? Pon un ejemplo de toString() en la clase Punto
### Respuesta
El método `toString()` se encuentra definido en la clase base Object y su función es proporcionar una representación textual del objeto. Dado que la implementación por defecto suele mostrar poca información útil, es habitual sobrescribirlo para mostrar datos significativos del estado del objeto. Este método también se invoca automáticamente cuando un objeto se imprime por consola o se concatena con una cadena.

Muchos lenguajes incluyen mecanismos equivalentes. En C# existe `ToString()`, en Python aparece `__str__()`, y en C++ se recurre normalmente a la sobrecarga del operador `<<` para integrarlo con los streams de salida. Todos ellos comparten el objetivo de facilitar la visualización clara del contenido de un objeto.

```java
class Punto {
    double x;
    double y;

    @Override
    public String toString() {
        return "(" + x + ", " + y + ")";
    }
}

## 16. Reflexiona: ¿una clase es como un struct en C? ¿Qué le falta al struct para ser como una clase y las variables de ese tipo ser instancias?
### Respuesta
Un `struct` en C únicamente permite agrupar datos bajo un nombre común, pero no incorpora las características propias de la programación orientada a objetos. Carece de elementos como *encapsulación*, métodos asociados al tipo, control de visibilidad o inicialización automática mediante constructores. Por ello, un struct puede considerarse solo como un contenedor de datos sin comportamiento integrado.

Para que un struct se comportara como una clase, debería permitir integrar funciones internas que actuaran como métodos y proporcionar mecanismos que protegieran el acceso a sus campos. También necesitaría soportar herencia y polimorfismo, lo que permitiría modelar relaciones entre tipos y adaptar comportamientos en función del contexto. Su diseño actual está orientado a la programación estructurada, no a la orientada a objetos.

---

## 17. Quitemos un poco de magia a todo esto: ¿cómo se podría “emular”, con struct en C, la clase Punto con su función para calcular la distancia al origen? ¿Qué ha pasado con this?
### Respuesta
En C, puede emularse parcialmente el comportamiento de una clase utilizando un `struct` para almacenar los datos y funciones externas que operen sobre un puntero a ese struct. El puntero pasado como argumento cumple el papel equivalente a *this*, ya que indica sobre qué objeto se ejecuta la función. Esta aproximación permite organizar código de manera similar, aunque sin integración directa entre datos y comportamiento.

Este método muestra cómo lenguajes orientados a objetos vinculan internamente métodos a las instancias. Sin embargo, en C este vínculo siempre debe hacerse de forma manual, lo que evidencia la ausencia de encapsulación y otras características propias de la orientación a objetos. Aun así, es una técnica útil para comprender cómo se construyen abstracciones más complejas a partir de herramientas básicas del lenguaje.

```c
typedef struct {
    double x;
    double y;
} Punto;

double distancia_al_origen(const Punto* p) {
    return sqrt(p->x * p->x + p->y * p->y);
}

double distancia_a(const Punto* p, const Punto* otro) {
    double dx = p->x - otro->x;
    double dy = p->y - otro->y;
    return sqrt(dx * dx + dy * dy);
}