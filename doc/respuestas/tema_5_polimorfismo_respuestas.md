<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Polimorfismo". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones, Composición y Herencia.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
## Tema 5. Polimorfismo
### 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?
#### Respuesta
El polimorfismo es un concepto fundamental de la programación orientada a objetos que permite que objetos de distintas clases relacionadas por herencia puedan ser tratados de forma uniforme mediante referencias a una clase base común. Gracias a ello, una misma llamada a un método puede producir comportamientos distintos dependiendo del tipo concreto del objeto que la recibe, lo que aporta mayor flexibilidad al diseño del software.

Desde el punto de vista del diseño, el polimorfismo facilita la extensibilidad y el mantenimiento del código, ya que permite añadir nuevas clases derivadas sin necesidad de modificar el código que trabaja con los tipos base. Esto resulta especialmente útil cuando se parte de conocimientos previos de herencia y composición, ya que el polimorfismo se apoya directamente en dichas relaciones.

La sobreescritura de métodos es el mecanismo que hace posible el polimorfismo. Consiste en redefinir en una subclase un método heredado de la clase base, manteniendo exactamente la misma firma, de forma que la subclase proporcione una implementación propia y especializada de dicho método.

### 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.
#### Respuesta
La ligadura dinámica, también conocida como enlace tardío, es el proceso mediante el cual la decisión de qué implementación concreta de un método se va a ejecutar se toma en tiempo de ejecución, y no en tiempo de compilación. Esta decisión se basa en el tipo real del objeto al que apunta la referencia, y no en el tipo de la referencia en sí.

La ligadura dinámica está directamente relacionada con el polimorfismo, ya que es el mecanismo que permite que una llamada a un método mediante una referencia de la clase base invoque automáticamente la versión sobrescrita correspondiente a la subclase concreta del objeto. Sin enlace tardío, el polimorfismo no podría funcionar correctamente.

En C++, la ligadura dinámica debe indicarse explícitamente mediante la palabra clave virtual en los métodos. En Java, por el contrario, los métodos son virtuales por defecto, salvo algunas excepciones, por lo que no es necesario indicarlo explícitamente. En Python, al tratarse de un lenguaje dinámico, la resolución de métodos se realiza siempre en tiempo de ejecución de forma automática.

### 3. Pon un ejemplo sencillo en Java, de un Soldado, con un método saluda, con dos subclases: Zapador y Artillero, donde Zapador sobreescribe el método saludar, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de Soldados de dos tipos y luego recorriéndolo empleando referencias de tipo Soldado y llamando a saludar.
#### Respuesta
En este ejemplo se define una clase base Soldado que incluye un método saludar. A partir de esta clase se crean dos subclases: Zapador y Artillero. La clase Zapador sobreescribe el método saludar para proporcionar un comportamiento completamente distinto al de la clase base.

El polimorfismo se ilustra al crear un array de referencias de tipo Soldado que almacena objetos de distintas subclases. Al recorrer el array y llamar al método saludar, se ejecuta automáticamente la versión correspondiente al tipo real de cada objeto.

```java
class Soldado {
    public void saludar() {
        System.out.println("Soldado: a sus órdenes");
    }
}

class Zapador extends Soldado {
    @Override
    public void saludar() {
        System.out.println("Zapador: listo para abrir camino");
    }
}

class Artillero extends Soldado {
}

public class Prueba {
    public static void main(String[] args) {
        Soldado[] soldados = { new Soldado(), new Zapador(), new Artillero() };
        for (Soldado s : soldados) {
            s.saludar();
        }
    }
}
```

### 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?
#### Respuesta
Al sobreescribir un método en una subclase, es posible reutilizar el comportamiento definido en la clase base y ampliarlo o modificarlo parcialmente. Esto permite evitar duplicación de código y facilita la evolución del comportamiento común.

Para invocar el método original de la clase base desde la subclase se utiliza la palabra clave super. De esta forma, la subclase puede ejecutar primero la lógica general y después añadir su comportamiento específico.

Esta técnica resulta especialmente útil cuando se desea mantener la funcionalidad general definida en la superclase, pero adaptarla ligeramente a las necesidades de la subclase concreta.

```java
@Override
public void saludar() {
    super.saludar();
    System.out.println("ZAPADOR A SUS ORDENES");
}
```

### 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (_overriding_) y sobrecarga (_overloading_)? ¿Para qué sirve la anotación @Override y por qué es recomendable usarla siempre?
#### Respuesta
En Java, para que un método pueda ser sobrescrito, debe mantener exactamente la misma lista de parámetros que el método original de la clase base. El tipo de retorno debe ser el mismo o un subtipo del original, lo que se conoce como retorno covariante. Además, no se pueden reducir los niveles de acceso del método sobrescrito.

La sobreescritura consiste en redefinir un método heredado para cambiar su implementación, mientras que la sobrecarga implica definir varios métodos con el mismo nombre pero con diferentes parámetros dentro de la misma clase. Ambos conceptos cumplen propósitos distintos dentro del diseño de clases.

La anotación @Override no es obligatoria, pero es altamente recomendable, ya que permite al compilador verificar que realmente se está sobrescribiendo un método existente y ayuda a detectar errores de forma temprana.

### 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo toString o sobreescribiendo equals, ¿ya estoy usando polimorfismo?
#### Respuesta
El polimorfismo se utiliza en Java desde etapas muy tempranas del aprendizaje del lenguaje, incluso antes de que el concepto se estudie de forma explícita. Al sobrescribir métodos heredados de la clase Object, como toString o equals, ya se está aplicando polimorfismo.

En estos casos, una referencia de tipo Object puede apuntar a instancias de cualquier clase, y la llamada a estos métodos ejecutará la versión correspondiente a la clase concreta del objeto. Este comportamiento es un ejemplo claro de ligadura dinámica.

Por tanto, aunque no se identifique inicialmente como tal, el polimorfismo forma parte del uso habitual de Java desde los primeros programas orientados a objetos.

### 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos Soldado, hagamos que, además del método saluda que ya tenía, tenga un método atacar, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner abstract?
#### Respuesta
Una clase abstracta es una clase que no puede ser instanciada directamente y que se utiliza como base para otras clases. Su finalidad es proporcionar una estructura común y, opcionalmente, parte de la implementación a sus subclases.

Un método abstracto es un método que no tiene implementación y que obliga a las subclases a proporcionar su propia definición. Las clases que contienen métodos abstractos deben declararse también como abstractas.

La palabra clave abstract debe utilizarse tanto en la declaración de la clase como en la de los métodos abstractos, indicando claramente que dichas partes deben ser completadas por las subclases.

```java
abstract class Soldado {
    public void saludar() {
        System.out.println("Soldado saluda");
    }
    public abstract void atacar();
}

class Zapador extends Soldado {
    @Override
    public void atacar() {
        System.out.println("El zapador coloca explosivos");
    }
}
```

### 8. ¿Qué efecto tiene la palabra clave final sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase final en la propia API estándar de Java?
#### Respuesta
La palabra clave final, aplicada a un método, impide que este sea sobrescrito por las subclases. Cuando se aplica a una clase, evita que dicha clase pueda ser heredada. En ambos casos, se limita el uso del polimorfismo.

El uso de final puede justificarse por motivos de diseño, seguridad o rendimiento, cuando se desea garantizar un comportamiento inalterable. Sin embargo, su utilización reduce la flexibilidad del sistema.

Un ejemplo de clase final dentro de la API estándar de Java es la clase String, cuyo diseño impide que sea extendida.

### 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?
#### Respuesta
Las interfaces en Java definen un conjunto de métodos que una clase se compromete a implementar, actuando como un contrato. No contienen estado y, tradicionalmente, no incluían implementación de métodos.

Aunque comparten similitudes con las clases abstractas, las interfaces permiten un mayor grado de abstracción y favorecen un diseño más desacoplado. Una diferencia clave es que una clase puede implementar varias interfaces, algo que no es posible con las clases abstractas.

Gracias a esta capacidad, las interfaces son una herramienta fundamental para aplicar polimorfismo sin recurrir a la herencia de clases.

### 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase Punto, con un método calcularDistanciaA, que permite calcular la distancia a otro Punto. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea instanceof y _downcasting_ para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase Linea, que acepta Punto, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).
#### Respuesta
En este diseño se define una clase abstracta Punto que declara un método abstracto para calcular la distancia a otro punto. Las subclases Punto2D y Punto3D implementan dicho método de acuerdo con sus propias dimensiones.

El uso de instanceof y el downcasting permite comprobar que los puntos con los que se opera son compatibles, evitando cálculos incorrectos entre puntos de diferentes dimensiones.

La clase Linea trabaja únicamente con referencias de tipo Punto, lo que demuestra el uso del polimorfismo al poder calcular la longitud sin conocer el subtipo concreto de los puntos.

```java
abstract class Punto {
    public abstract double calcularDistanciaA(Punto otro);
}

class Punto2D extends Punto {
    double x, y;
    @Override
    public double calcularDistanciaA(Punto otro) {
        if (otro instanceof Punto2D) {
            Punto2D p = (Punto2D) otro;
            return Math.hypot(x - p.x, y - p.y);
        }
        throw new IllegalArgumentException();
    }
}

class Punto3D extends Punto {
    double x, y, z;
    @Override
    public double calcularDistanciaA(Punto otro) {
        if (otro instanceof Punto3D) {
            Punto3D p = (Punto3D) otro;
            return Math.sqrt(Math.pow(x - p.x, 2)
                    + Math.pow(y - p.y, 2)
                    + Math.pow(z - p.z, 2));
        }
        throw new IllegalArgumentException();
    }
}

class Linea {
    Punto a, b;
    public Linea(Punto a, Punto b) {
        this.a = a;
        this.b = b;
    }
    public double longitud() {
        return a.calcularDistanciaA(b);
    }
}
```

### 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz Fichero que tenga un método para leer su contenido en forma de String y luego dicha interfaz sea extendida por otra que sea FicheroEscribible que permita enviar contenido e incluso eliminar el fichero.
#### Respuesta
La herencia de interfaces se produce cuando una interfaz extiende a otra, heredando sus métodos y pudiendo añadir nuevos. Este mecanismo permite reutilizar contratos y estructurar jerarquías de interfaces.

En Java existe herencia múltiple de interfaces, ya que una interfaz puede extender varias interfaces y una clase puede implementar varias interfaces simultáneamente. Esto no presenta los problemas asociados a la herencia múltiple de clases.

Este enfoque facilita el uso del polimorfismo y la definición de comportamientos comunes sin depender de una jerarquía rígida de clases.

```java
interface Fichero {
    String leer();
}

interface FicheroEscribible extends Fichero {
    void escribir(String contenido);
    void borrar();
}
```
