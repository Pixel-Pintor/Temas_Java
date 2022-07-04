# Polimorfismo
En general, **polimorfismo** significa que algo (un objeto u otra entidad) tiene muchas formas.  
Java proporciona dos tipos de polimorfismo: **estatico (en tiempo de compilacion)** y **dinamico (en tiempo de ejecucion)**. El primero se logra mediante **sobrecarga de metodos**, el segundo se basa en la herencia y en la **anulacion de metodos**.  
El enfoque mas teorico subdivide el polimorfismo en varios tipos fundamentales diferentes:
- El **polimorfismo ad-hoc** se refiere a funciones polimorficas que se pueden aplicar a argumentos de diferentes tipos, pero se comportan de manera diferentes segun el tipo de argumento al que se aplican. Java lo admite como metodo de **sobrecarga**.
- El **polimorfismo de subtipo** es una posibilidad de usar una instancia de una subclase cuando se permite una instancia de la clase base.
- El **polimorfismo parametrico** es cuando el codigo se escribe sin mencionar ningun tipo especifico y, por lo tanto, se puede usar de forma transparente con cualquier cantidad de tipos nuevos. Java lo admite como **genericos** o **programacion generica**.  
En este tema consideraremos solo el **polimorfismo de subtipo** que se usa ampliamente en la programacion orientada a objetos.  
El polimorfismo en tiempo de ejecucion se basa en dos principios:
- Una variable de referencia de la superclase puede referirise a cualquier objeto de subtipo.
- Un metodo de superclase puede anularse en una subclase.  
El polimorfismo en tiempo de ejecucion funciona cuando se llama a un metodo anulado a traves de la variable de referencia de una superclase. Java determina en tiempo de ejecucion que version del metodo (superclase/subclases) se ejecutaran segun el tipo de objeto al que se hace referencia, no el tipo de referencia. Utiliza un mecanismo conocido como **envio de metodos dinamicos**.  
**Ejemplp**. Aqui puede ver una jerarquia de clases. La superclase `MythicalAnimal` tiene dos subclases: `Chimera` y `Dragon`. La clase base tiene un metodo `hello`. Ambas subclases anulan este metodo.
~~~java
class MythicalAnimal {

    public void hello() {
        System.out.println("Hello, I'm an unknown animal");
    }
}

class Chimera extends MythicalAnimal {
    @Override
    public void hello() {
        System.out.println("Hello! Hello!");
    }
}

class Dragon extends MythicalAnimal {
    @Override
    public void hello() {
        System.out.println("Rrrr...");
    }
}
~~~
Podemos crear una referencia a la clase `MythicalAnimal` y asignarle el objeto de la subclase:
~~~java
MythicalAnimal chimera = new Chimera();
MythicalAnimal dragon = new Dragon();
MythicalAnimal animal = new MythicalAnimal();
~~~
Tambien podemos invocar metodos anulados a traves de las referencias de la clase base:
~~~java
chimera.hello(); // Hello! Hello!
dragon.hello(); // Rrrr...
animal.hello(); // Hello, I'm an unknown animal
~~~
Por lo tanto, el resultado de una llamada de metodo depende del tipo real de instancia, no del tipo de referencia. Es una caracteristica polimorfica en Java.  
El polimorfismo de subtipo permite que una clase especifique metodos que seran comunes a todas sus subclases. El polimorfirsmo de subtipo tambien hace posible que las subclases anulen las implementaciones de esos metodos.  
Lo mismo funciona con metodos que se usan solo dentro de una jerarquia y no son accesibles desde el exterior.  
En el siguiente ejemplo, tenemos una jerarquia de archivos. La clase padre `File` representa una descripcion de un solo archivo en el sistema de archivos. Tiene una subclase llamada `ImageFile`. Anula el metodo `getFileInfo` de la clase padre.
~~~java
class File {

    protected String fullName;

    // constructor
    // getters and setters

    public void printFileInto() {
        String info = this.getFileInfo(); // este es conportamiento polimorfico
        System.out.println(info);
    }

    protected String getFileInfo() {
        return "File: " + fullName;
    }
}

class ImageFile extends File {

    protected int width;
    protected int height;
    protected byte[] content;

    // constructor
    // getters and setters

    @Override
    protected String getFileInfo() {
        return String.format("Image: %s, widht: %d, height: %d", fullName, width, height);
    }
}
~~~
La clase padre tiene une metodo publico, `printFileInto` y uno metodo protegido `getFileInfo`. El segundo metodo se anula en la subclase, pero la subclase no anula el primero metodo.   
Vamos a crear una instancia de `ImageFile` y asignarlo a una variable de `File`.
~~~java
File img = new ImageFile("/path/to/file/img.png", 480, 640, someBytes); // asignando un objeto
~~~
Ahora, cuando llamamos el metodo `printFileInfo`, invoca la version anulada del metodo `getFileInfo`.
~~~java
img.printFileInfo(); // llama al metodo de ImageFile "Image: /path/to/file/img.png, width: 480, height: 640"
~~~
Entonces el **polimorfismo en tiempo de ejecucion** le permite invocar un metodo anulado de una subclase que tiene una referencias a la clase base.
---
## Ejercicios
Tiene una jerarquia de clases de notificaciones. La clase padre es `Notification`. Tiene dos subclases. Cada subclase anula un metodo. Hay tres objetos, selecciona todas las afirmaciones correctas sobre estos objetos.
~~~java
class Notification {

    protected String msg;

    public Notification(String msg) {
        this.msg = msg;
    }

    public void show() {
        System.out.println(getMsg());
    }

    public String getMsg() {
        return msg;
    }
}

class Warning extends Notification {

    public Warning(String msg) {
        super(msg);
    }
    @Override
    public String getMsg() {
        return "WARN: " + msg;
    }
}

class Alarm extends Notification {

    public Alarm(String msg) {
        super(msg);
    }
    @Override
    public void show() {
        System.out.println("ALARM: " + msg);
    }
}

class Main {
    public static void main(String[] args) {

        // hay tres objetos
        Notification notif = new Notification("No problems");
        Notification warn = new Warning("Money ends");
        Notification alarm = new Alarm("The ship sank");

        warn.show(); // Money ends
        warn.getMsg(); // WARN: Money ends
        alarm.show(); // ALARM: The ship sank
        alarm.getMsg(); // The ship sank
        notif.show(); // No problems
    }
}
~~~
2. Que son **anulacion de metodos** y **sobrecarga de metodos**, seleccione las respuestas correctas
- La **anulacion** es un tipo de polimorfismo dinamico.
- La **sobrecarga** es un tipo de polimorfismo estatico.  
3. Cual de los siguientes es el tipo correcto de polimorfismo en Java? -> Polimorfismo en tiempo de ejecucion.
4. Tiene una jerarquia de calses que consta de dos dos clases, la clase basica y la clase derivada. Cual es el resultado del codigo?
~~~java
public class BaseClass {

    public void print() {
        System.out.println("Base class");
    }
}

public class DerivedClass extends BaseClass {

    public void print() {
        System.out.println("Derived class");
        super.print();
    }
}

BaseClass clazz = new DerivedClass();
clazz.print();

// resultado
// Derived class
// Base class
~~~
5. Aqui hay una jerarquia de clases, que hara el codigo.
~~~java
class A {

    public void print() {
        System.out.println("class-A");
    }
}

class B extends A {}

class C extends B {

    public void print() {
        System.out.println("class-C");
    }
}

class D extends C {

    public void print() {
        System.out.println("class-D");
    }
}

B instance = new C();
instance.print();

// class-C
~~~
6. Invocar un metodo de subclase llamando al metodo de superclase con el mismo nombre es una especie de... Polimorfismo en tiempo de ejecucion.
7. Se le dan cuatro clases. Necesitas anular los metodos `getType()` y `getDetails()` en clases heredadas de la clase `Publication`. Entonces necesitas implementar `getInfo()` en la clase `Publication` utilizando `getType()` y `getDetails()`. El metodo debe devolver un `String` con un tipo de publicacion en primero lugar, luego los detalles entre parentesis y el titulo despues de dos puntos. 
~~~java
class Publication {

    private final String title;

    public Publication(String title) {
        this.title = title;
    }

    public final String getInfo() {
        return getType() + getDetails() + title;
    }

    public String getType() {
        return "Publication";
    }

    public String getDetails() {
        return ": ";
    }

}

class Newspaper extends Publication {

    private final String source;

    public Newspaper(String title, String source) {
        super(title);
        this.source = source;
    }

    @Override
    public String getType() {
        return "Newspaper ";
    }

    @Override
    public String getDetails() {
        return String.format("(source - %s): ", source);
    }
}

class Article extends Publication {

    private final String author;

    public Article(String title, String author) {
        super(title);
        this.author = author;
    }

    @Override
    public String getType() {
        return "Article ";
    }

    @Override
    public String getDetails() {
        return String.format("(author - %s): ", author);
    }
}

class Announcement extends Publication {

    private final int daysToExpire;

    public Announcement(String title, int daysToExpire) {
        super(title);
        this.daysToExpire = daysToExpire;
    }

    @Override
    public String getType() {
        return "Announcement ";
    }

    @Override
    public String getDetails() {
        return String.format("(days to expire - %d): ", daysToExpire);
    }
}
~~~
8. Aqui hay una jerarquia de un generador de numeros magicos. Tienes una instancia, cual sera el resultado de `generator.generate()`.
~~~java
class BaseNumberGenerator {

    protected int base;

    public BaseNumberGenerator(int base) {
        this.base = base;
    }

    public int generate() {
        return base + 11;
    }
}

class NumberGenerator extends BaseNumberGenerator {

    public NumberGenerator(int base) {
        super(base);
    }

    @Override
    public int generate() {
        return super.generate() + getNumber();
    }

    protected int getNumber() {
        return this.base - 7;
    }
}

class MagicNumberGenerator extends NumberGenerator {

    public MagicNumberGenerator(int base) {
        super(base);
    }

    @Override
    protected int getNumber() {
        return this.base + 7;
    }
}

class Main {
    public static void main(String[] args) {
        BaseNumberGenerator generator = new MagicNumberGenerator(10);
        System.out.println(generator.generate());
    }
}

// imprime 38