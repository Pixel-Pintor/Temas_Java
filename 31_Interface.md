# Interfaz
La idea general de OOP y uno de sus principios es la abstraccion. Significa que los objetos del mundo real pueden ser representados por sus modelos abstractos. Diseñar modelos consiste en centrarse en las caracteristicas esenciales de los objetos y descartar las demas. Imagina un lapiz, es un objeto que podemos utilizar para dibujar. Otras propiedades, como el material o la longitud, pueden ser importantes para nosotros, pero no definen la idea de un lapiz.  
Ahora consideremos la clase `Pencil`, que es una abstraccion de un lapiz. Como ya discutimos, la clase al menos deberia tener un metodo `draw` que acepta un modelo de una curva. Esta es una funcion crucial de un lapiz para nuestro programa. Esta es una clase `Curve` que representa cualquier curva:
~~~java
class Pencil {
    ...
    public void draw(Curve curve) { ... }
}
~~~
Definamos clases para otras herramientas, por ejemplo, un pincel:
~~~java
class Brush {
    ...
    public void draw(Curve curve) { ... }
}
~~~
Cada uno de ellos tiene el metodo `draw`, aunque lo usa a su manera. La habilidad para dibujar es una funcion caracteristica comun para todos ellos. Llamemos a esta funcion `DrawingTool`. Entonces podemos decir que si una clase tiene la caracteristica `DrawingTool`, entonces deberia poder dibujar, eso significa que la clase deberia tener el metodo `void draw(Curve curve) { ... }`.  
Java permite declarar esta funcion mediante la introduccion de interfaces. Asi es como se ve nuestra interfaz:
~~~java
interface DrawingTool {
    void draw(Curve curve);
}
~~~
Declara el metodo `draw` sin implementacion.  
Ahora marquemos las clases que pueden dibujar agregando `implements DrawingTool` a la declaracion de clase. Si una clase implementa una interfaz, tiene que implementar todos los metodos declarados:
~~~java
class Pencil implements DrawingTool {
    ...
    public void draw(Curve curve) { ... }
}

class Brush implements DrawingTool {
    ...
    public void draw(Curve curve) { ... }
}
~~~
Ahora basta con echar un vistazo rapido a la declaracion de la clase para comprendenr que la clase puede dibujar. En otras palabra, la idea principal de una interfaz es **declarar funcionalidad**.  
Otra ventaja importante de introducir interfaces es que puedes usarlas como un tipo:
~~~java
DrawingTool pencil = new Pencil();
DrawingTool brush = new Brush();
~~~
Ahora tanto un lapiz como un pincel tienen el mismo tipo. Significa que ambas clases pueden ser tratadas de manera similar como una `DrawingTool`. Esta es otra forma de admitir **polimorfismo**, que ayuda a diseñar la funcion de dibujo reutilizable del programa editor grafico.
~~~java
void drawCurve(DrawingTool tool, Curve curve) {
    System.out.println("Drawing a curve " + curve + " using a " + tool);
    tool.draw(curve);
}
~~~
En muchos casos, es mas importante saber que puede hacer un objeto que como hace lo que hace. Esta es una razon por la cual las interfaces se usan comunmente para declarar un tipo de variable.  
Una interfaz se puede considerar como un tipo especial de clase de la que no se puede crear una instancia. Para declarar una interfaz debe escribir la palabra clave `interface` en lugar de la palabra `class`:
~~~java
interface Interface {}
~~~
Una interfaz puede contener:
- constantes publicas.
- metodos abstractos sin una implementacion (la palabra `abstract` no es necesaria).
- metodos predeterminados con implementacion (la palabra `default` es requerida).
- metodos estaticos con implementacion (la palabra `static` es requerida).
- metodos privados con implementacion.  
La palabra clave `abstract` antes de un metodo significa que el metodo no tiene cuerpo, solo declara una forma. Una interfaz no puede contener constructores, metodos abstractos no publicos ni ningun otro campo que no sea `public static final` (**constantes**). Declaremos una interfaz que contenga todos los miembros posibles:
~~~java
interface Interface {

    // constante de clase
    int INT_CONSTANT = 0; // constante, igual que public static final int INT_FIELD = 0

    // metodos abstractos
    void instanceMethod1();
    void instanceMethod2();

    // metodo estatico normal
    static void staticMethod() {
        System.out.println("Interface: static method");
    }

    // implementacion que se puede anular en una subclase
    default void defaultMethod() {
        System.out.println("Interface: default method. it can be overriden.");
    }

    // metodo que tiene una implementacion
    // se puede usar para descomponer metodos default
    private void privateMethod() {
        System.out.println("Interface: private methods in interfaces are acceptable but should have a body");
    }
}
~~~
Los metodos estaticos, predeterminados y privados deben tener una implementacion en la interfaz.  
Una clase puede implementar una interfaz usando la palabra `implements`. Debemos proporcionar implementaciones para todos los metodos abstractos de la interfaz. Implementemos la interfaz que hemos creado anteriormente.
~~~java
class Class implements Interface {

    @Override
    public void instanceMethod1() {
        System.out.println("Class: instance method1");
    }

    @Override
    public void instanceMethod2() {
        System.out.println("Class: instance method2");
    }
}
~~~
Ahora podemos crear una instancia de la clase y llamar a sus metodos.
~~~java 
Interface instance = new Class();

instance.instanceMethod1(); // imprime: Class: instance method1
instance.instanceMethod2(); // imprime: Class instance method2
instance.defaultMethod(); // imprime: default method. It can be overriden
~~~
Tenga en cuenta que la variable `instance` tiene el tipo `Interface`, aunque esta bien usar `Class`.
~~~java
Class instance = new Class();
~~~
Una de las caracteristicas importantes de la interfaz es la **herencia multiple**. Una clase puede implementar multiples interfaces:
~~~java
interface A {}
interface B {}
interface C {}

class D implements A, B, C {}
~~~
Una interfaz puede extender una o mas interfaces usando `extends`:
~~~java
interface A {}
interface B {}
interface C {}

interface E extends A, B, C {}
~~~
Una clase tambien puede extender otra clase e implementar multiples interfaces.
~~~java
class A {}

interface B {}
interface C {}

class D extends A implements B, C {}
~~~
En algunas situaciones, una interfaz puede no tener ningun miembro. Estas interfaces se denominan interfaces **marcadoras** o **etiquetadas**. Por ejemplo, una interfaz conocida `Serializable` es una interfaz de marcador.
~~~java
public interface Serializable {}
~~~
Puede declarar e implementar un metodo estatico en una interfaz.
~~~java
interface Car {
    static float convertToMilesPerHour(float kmh) {
        return 0.62 * kmh;
    }
}

// para usar un metodo estatico, lo invoca desde la interfaz
Car.convertToMilesPerHour(4.5);
~~~
El objetivo principal de los metodos estaticos de interfaz es definir la funcionalidad de utilidad que es comun para todas las clases que implementan la interfaz. Ayudan a evitar la duplicacion de codigo y la creacion de clases de utilidad adicionales.
---
## Ejercicios 
1. Tenemos una interfaz y una clase, cuales metodos deben ser implementados en la clase `B`?
~~~java
interface A {

    static void staticMethod() {}

    void method();

    default void defaultMethod() {}

    abstract void abstractMethod();
}

class B implements A {}

// solo debe implementar los metodos abstractos 
// method() y abstractMethod()
~~~
1. Cual conjunto de modificadores tendra un metodo por defecto? -> `public abstract`.
2. La clase `Circle` representa un circulo. Implemente la interfaz `Measurable` y agregue el metodo `area()` que retorne el area del circulo. Use `Math.PI`, la clase sera testeada creando una instancia de `Circle` e invocando su metodo `area()`.
~~~java
class Circle implements Measurable {
    private double radius;

    public Circle(double radius) {
        this.radius = radius;
    }

    @Override
    public double area() {
        return Math.PI * Math.pow(radius, 2);
    }
}

interface Measurable {
    double area();
}
~~~
3. Suponga que esta escribiendo un motor geometrico. Incluye las clase `Circle` y `Rectangle`, ademas de las interfaces `Movable` y `Scalable`, implemente las interfaces.
~~~java
interface Movable {

    void move(float dx, float dy);
}

interface Scalable {

    void scale(float factor);
}

interface MutableShape extends Movable, Scalable {

}

final class Circle implements MutableShape {

    private float centerX;
    private float centerY;
    private float radius;

    public Circle(float centerX, float centerY, float radius) {
        this.centerX = centerX;
        this.centerY = centerY;
        this.radius = radius;
    }

    public float getCenterX() {
        return centerX;
    }

    public float getCenterY() {
        return centerY;
    }

    public float getRadius() {
        return radius;
    }

    @Override
    public void move(float dx, float dy) {
        this.centerX += dx;
        this.centerY += dy;
    }

    @Override
    public void scale(float factor) {
        this.radius *= factor;
    }
}

final class Rectangle implements MutableShape {

    private float x;
    private float y;
    private float width;
    private float height;

    public Rectangle(float x, float y, float w, float h) {
        this.x = x;
        this.y = y;
        this.width = w;
        this.height = h;
    }

    public float getX() {
        return x;
    }

    public float getY() {
        return y;
    }

    public float getWidth() {
        return width;
    }

    public float getHeight() {
        return height;
    }

    @Override
    public void move(float dx, float dy) {
        this.x += dx;
        this.y += dy;
    }

    @Override
    public void scale(float factor) {
        this.width *= factor;
        this.height *= factor;
    }
}
~~~
4. Escribe una clase llamada `AsciiCharSequence` para almacenar secuencias de caracteres ASCII, deberia:
- Implementar la interfaz `java.lang.CharSequence`.
- tener un costructor que tome una matriz de byes.
- tener metodos `int length()`, `char charAt(int idx)`, `CharSequence subSequence(int from, int to)` y `String toString()`.
~~~java
class AsciiCharSequence implements CharSequence {

    byte[] chars;

    public AsciiCharSequence(byte[] chars) {
        this.chars = Arrays.copyOf(chars, chars.length);
    }

    @Override
    public int length() {
        return chars.length;
    }

    @Override
    public char charAt(int i) {
        return (char) chars[i];
    }

    @Override
    public CharSequence subSequence(int i, int i1) {
        return new AsciiCharSequence(Arrays.copyOfRange(chars, i, i1));
    }

    @Override
    public String toString() {
        return new String(chars);
    }
}
~~~
