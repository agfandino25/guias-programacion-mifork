## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?

### Respuesta

En orientación a objetos, la **herencia** es un mecanismo que permite definir una nueva clase (subclase) a partir de otra ya existente (superclase), reutilizando sus atributos y métodos y pudiendo además añadir o modificar comportamiento. Este mecanismo facilita la organización jerárquica del código y promueve la reutilización.

La relación "A es-un B" significa que un objeto de la subclase puede ser tratado como un objeto de la superclase. Por ejemplo, un `Artillero` es-un `Soldado`, lo que implica que cualquier lugar donde se espere un `Soldado` puede recibir un `Artillero`.

La primera implicación es la **compatibilidad de tipos**, que permite trabajar con referencias del tipo base para manejar objetos de distintos subtipos. La segunda es la **herencia de estado y comportamiento**, mediante la cual la subclase reutiliza atributos y métodos de la superclase, evitando duplicación de código y favoreciendo la mantenibilidad.

```java
class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

class Artillero extends Soldado {
    private int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre);
        this.cohetes = cohetes;
    }

    public int getCohetes() {
        return cohetes;
    }
}

class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public int getMinas() {
        return minas;
    }
}

Soldado[] soldados = {
    new Artillero("Juan", 5),
    new Zapador("Luis", 3)
};

for (Soldado s : soldados) {
    s.saludar();
}
```


---

## 2. Constructores y `super`

### Respuesta

Al instanciar un objeto de una subclase, se ejecuta una cadena de constructores que comienza en la superclase más alta de la jerarquía y termina en la subclase concreta. Esto garantiza que todos los atributos heredados se inicialicen correctamente antes de ejecutar el código propio de la subclase.

La palabra clave `super` se utiliza dentro del constructor de la subclase para invocar explícitamente el constructor de la superclase. Esta llamada debe ser la primera instrucción del constructor. Su función principal es inicializar la parte heredada del objeto.

Si la superclase no dispone de un constructor sin parámetros accesible, es obligatorio llamar a `super(...)` con los argumentos adecuados. En caso contrario, el compilador generará un error, ya que no podrá inicializar correctamente la superclase.

---

## 3. Atributos privados en memoria

### Respuesta

Los atributos privados definidos en la superclase forman parte de la estructura interna de los objetos de la subclase. Es decir, cuando se crea un objeto de una subclase, este contiene en memoria todos los atributos heredados, incluidos los privados.

Sin embargo, el hecho de que estos atributos existan en memoria no implica que puedan ser accedidos directamente desde la subclase. La encapsulación impide el acceso directo a atributos privados, obligando a utilizar métodos públicos o protegidos definidos en la superclase.

Por ejemplo, en el caso de `Soldado`, el atributo `nombre` está presente en un `Artillero`, pero solo puede ser utilizado a través de métodos como `saludar()`, garantizando así el control del acceso.

---

## 4. Extensibilidad del código

### Respuesta

La compatibilidad de tipos proporcionada por la herencia favorece enormemente la extensibilidad del código. Permite añadir nuevas subclases sin necesidad de modificar el código existente que trabaja con el tipo base.

Este enfoque sigue el principio de abierto/cerrado: el sistema está abierto a extensión pero cerrado a modificación. Es decir, se pueden añadir nuevas funcionalidades sin alterar el código ya probado.

```java
class Medico extends Soldado {
    public Medico(String nombre) {
        super(nombre);
    }
}
```

```
```java
Soldado[] soldados = {
    new Artillero("Juan", 5),
    new Zapador("Luis", 3),
    new Medico("Ana")
};

for (Soldado s : soldados) {
    s.saludar();
}
```


---

## 5. Referencias, casting y `instanceof`

### Respuesta

En Java, una referencia del supertipo puede apuntar a un objeto de cualquiera de sus subtipos, lo que permite trabajar de forma polimórfica. Sin embargo, esta referencia solo permite acceder a los métodos definidos en el tipo declarado.

El **upcasting** es la conversión automática de un subtipo a un supertipo. El **downcasting** es la conversión inversa y requiere una comprobación previa para evitar errores en tiempo de ejecución.

El operador `instanceof` permite comprobar el tipo real del objeto antes de realizar un casting, evitando excepciones como `ClassCastException`.

```java
for (Soldado s : soldados) {
    if (s instanceof Artillero) {
        Artillero a = (Artillero) s;
        System.out.println("Cohetes: " + a.getCohetes());
    }
}
```


---

## 6. Acceso protegido

### Respuesta

El modificador `protected` permite que un atributo o método sea accesible desde la propia clase, desde sus subclases y desde otras clases del mismo paquete. Representa un punto intermedio entre `private` y `public`.

Este tipo de acceso es especialmente útil en herencia, ya que permite a las subclases reutilizar directamente ciertos atributos sin exponerlos completamente al exterior.

```java
class Soldado {
    protected String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }
}

class Zapador extends Soldado {
    public Zapador(String nombre) {
        super(nombre);
    }

    public void ponerMina() {
        System.out.println(nombre + " ha puesto una mina");
    }
}
```


---

## 7. Clase base de todos los objetos

### Respuesta

En muchos lenguajes orientados a objetos existe una clase raíz común para todos los objetos, lo que permite definir comportamiento compartido universal.

En Java, todas las clases heredan implícitamente de la clase `Object`. Esto implica que cualquier objeto dispone de métodos como `toString()`, `equals()` o `hashCode()`, lo que proporciona una base común de funcionalidad.

---

## 8. Herencia múltiple

### Respuesta

La herencia múltiple permite que una clase herede de más de una superclase, lo que puede generar ambigüedades en el comportamiento heredado.

Java no permite herencia múltiple de clases para evitar estos problemas. Sin embargo, sí permite implementar múltiples interfaces, lo que proporciona una alternativa más segura y flexible.

---

## 9. Excepción personalizada

### Respuesta

```java
class Usuario {
    private String nombre;

    public Usuario(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}

class UsuarioNoEncontradoException extends RuntimeException {
    private Usuario usuario;

    public UsuarioNoEncontradoException(Usuario usuario) {
        super("Usuario no encontrado: " + usuario.getNombre());
        this.usuario = usuario;
    }

    public UsuarioNoEncontradoException(Usuario usuario, Throwable causa) {
        super("Usuario no encontrado: " + usuario.getNombre(), causa);
        this.usuario = usuario;
    }
}
```

---

## 10. Herencia vs reutilización

### Respuesta

La herencia no debe utilizarse únicamente como mecanismo de reutilización de código, ya que implica una relación fuerte entre clases basada en el concepto "es-un". Si esta relación no es conceptualmente correcta, el diseño resultante será erróneo.

Además, la herencia introduce un fuerte acoplamiento entre la superclase y las subclases, lo que dificulta la evolución del sistema. Cambios en la superclase pueden afectar inesperadamente a todas las subclases.

---

## 11. Favorecer composición frente a herencia

### Respuesta

La composición consiste en construir clases utilizando otras clases como componentes internos, delegando en ellas parte del comportamiento.

Se recomienda frente a la herencia porque ofrece mayor flexibilidad, permite cambiar comportamientos en tiempo de ejecución y reduce el acoplamiento. Además, evita las limitaciones de las jerarquías rígidas.

---

## 12. La herencia rompe la encapsulación

### Respuesta

La herencia puede romper la encapsulación porque las subclases dependen de detalles internos de la superclase, incluso si estos no forman parte de su interfaz pública.

Esto significa que cambios en la implementación interna de la superclase pueden afectar al comportamiento de las subclases, generando errores difíciles de detectar.

---

## 13. Herencia vs composición

### Respuesta

```java
class Persona {
    protected String dni;
    protected String nombre;

    public Persona(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }
}
```


```java
class DatosPersonales {
    private String dni;
    private String nombre;

    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }
}
```