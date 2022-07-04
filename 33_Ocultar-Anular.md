# Ocultar y Anular
Java brinda la oportunidad de declarar un metodo en una subclase con el mismo nombre que un metodo en la superclase. Esto se conoce como **anulacion de metodos**. El beneficio de anular es que una subclase puede dar su propia implementacion especifica de un metodo de superclase.  
La **anulacion de metodos** en las subclases permite que una clase herede de una superclase cuyo comportamiento es **"lo suficientemente parecido"** y luego cambia este comportamiento segun las necesidades de la subclase. Los metodos de instancia se pueden anular si son heredados por la subclase. El metodo anulado debe tener el mismo nombre, parametros (numero y tipo de parametros) y el tipo de retorno (o una subclase de tipo) que el metodo anulado.  
~~~java
class Mammal {

    public String sayHello() {
        return "ohlllalalalalalaoaoaoa";
    }
}

class Cat extends Mammal {

    @Override
    public String sayHello() {
        return "meow";
    }
}

class Human extends Mammal {

    @Override
    public String sayHello() {
        return "hello";
    }
}
~~~
La jerarquia incluye tres clases: `Mammal`, `Cat` y `Human`. La clase `Mammal` tiene el metodo `sayHello`. Cada subclase anula este metodo. La anotacion `@Override` indica que el metodo esta anulado. Esta anotacion es opcional pero util. Vamos a crear instancias e invocar el metodo.
~~~java
Mammal mammal = new Mammal();
System.out.println(mammal.sayHello()); // imprime "ohlllalalalalalaoaoaoa"

Cat cat = new Cat();
System.otu.println(cat.sayHello()); // imprime "meow"

Human human = new Human();
System.out.println(human.sayHello()); // imprime "hello"
~~~
Como puede ver, cada subclase tiene su propia implementacion del metodo `sayHello()`. Puede invocar el metodo de la clase base en el metodo anulado usando la palabra clave `super`.
---
## Reglas para anular metodos
Hay varias reglas para los metodos de las subclases que deberian anular los metodos de una superclase:
- El metodo debe tener el mismo nombre que en la superclase.
- Los argumentos deben ser exactamente los mismos que el metodo de la superclase.
- El tipo de retorno debe ser el mismo tipo o un subitpo del tipo de retorno declarado en el metodo de la superclase.
- El nivel de acceso debe ser igual o mas abierto que el nivel de acceso del metodo anulado.
- Un metodo privado no se puede anular porque las subclases no lo heredan.
- Si la superclase y su subclase estan en el mismo paquete, los metodos privados del paquete pueden anularse.
- Los metodos estaticos no se pueden anular.  
Para verificar estas reglas, esta la anotacion especial `@Override`. Le permite saber si un metodo se **anulara** o no.  
Si desea prohibir la anulacion de un metodo, declarelo con la palabra clave `final`.
~~~java
public final void method() {
    // do something
}
~~~
Recuerde, que la **sobrecarga** es una caracteristica que permite que una clase tenga mas de un metodo con el mismo nombre, si sus argumentos son diferentes. Tambien podemos anular y sobrecargar un metodo de instancia en una subclase al mismo tiempo. Los metodos sobrecargados no anulan los metodos de instancia de la superclase. Son metodos nuevos, exclusivos de la subclase.
~~~java
class SuperClass {

    public void invokeInstanceMethod() {
        System.out.println("SuperClass: invokeInstanceMethod");
    }
}

class SubClass extends SuperClass {

    @Override
    public void invokeInstanceMethod() {
        System.out.println("SubClass: invokeInstanceMethod is overriden");
    }

    // @Override -- metodo que no anula nada
    public void invokeInstanceMethod(String s) {
        System.out.println("SubClass: overloaded invokeInstanceMethod(String)");
    }
}

// crea una instancia y llama a ambos metodos
SubClass clazz = new SubClass();

clazz.invokeInstanceMethod(); // SubClass: invokeInstanceMethod is overriden
clazz.invokeInstanceMethod("s"); // SubClass: overloaded invokeInstanceMethod(String)
~~~
Recuerde, anular y sobrecargar son mecanismo diferentes, pero puede combinarlos en una jerarquia de clases.  
Los metodos estaticos no se pueden anular. Si una subclase tiene un metodo estatico con la misma firma que un metodo estatico de la superclase, entonces el metodo en la subclase oculta el de la superclase. Es completamente diferente de la anulacion de metodos. Si los metodos tienen el mismo nombre pero diferentes parametros, no deberia haber problemas.
---
## Ejercicios
1. Cuales de los siguientes metodos se anulara en `SubClass`?
~~~java
class SuperClass {

    public static void staticMethod(int i) {}

    public void method1(int 1) {}

    public void method2(int i, String s) {}

    private void method3(int i) {}
}

class SubClass extends SuperClass {

    public static void staticMethod(int i) {}

    public void method1(int i) {}

    public void method2(String s) {}

    public void method3(int i) {}
}

// staticMethod -> NO por que es estatico, entonces la subclase lo oculta
// method1 -> SI
// method2 -> NO por que cambia la firma, entonces los sobrecarga
// method3 -> NO no por que es privado
~~~
1. Existe una clase y una subclase, seleccione cual metodo de `A` que es incorrectamente anulado en la clase `B`.
~~~java
class A {

    public A method1() { return new A(); }

    public B method2() { return new B(); }

    public A method3(A a) { return a; }

    public B method4() { return new B(); }
}

class B extends A {

    @Override
    public B method1() { return new B(); }

    @Override
    public B method2() { return new B(); }

    public A method2(A a) { return a; }

    public A method4() { return new A(); }
}

// method4() esta mal anulado
~~~
2. Existe una clase y una subclase, seleccione todos los metodos de la clase `A` que se anulan correctamente en la clase `B`.
~~~java
class A {
    public void doA() {}

    public final void doB() {}

    public void doC(int k) {}

    protected void doD() {}

    protected void doE() {}
}

class B extends A {
    @Override
    public void doA() {}

    @Override
    public void doB() {}

    @Override
    public void doC(String s) {}

    @Override
    private void doD() {}

    public void doE() {}
}
~~~
3. Tenemos cuatro clase: `Publication`, `Newspaper`, `Article` y `Announcement`. Necesitas anular el metodo `getDetails()` en las clase heredadas de `Publication`. Estas clases usan `getDetails()` de `Publication` para obtener informacion sobre el titulo y agregar sus propios datos adicionales.
~~~java
class Publication {

    private String title;

    public Publication(String title) {
        this.title = title;
    }

    public String getDetails() {
        return "title=\"" + title + "\"";
    }

}

class Newspaper extends Publication {

    private String source;

    public Newspaper(String title, String source) {
        super(title);
        this.source = source;
    }

    public String getDetails() {
        return super.getDetails() + "source=\"" + source + "\"";
    }
}

class Article extends Publication {

    private String author;

    public Article(String title, String author) {
        super(title);
        this.author = author;
    }

    public String getDetails() {
        return super.getDetails() + ", author=\"" + author + "\"";
    }
}

class Announcement extends Publication {

    private int daysToExpire;

    public Announcement(String title, int daysToExpire) {
        super(title);
        this.daysToExpire = daysToExpire;
    }

    public String getDetails() {
        return super.getDetails() + ", daysToExpire=" + daysToExpire;
    }
}
~~~
4. Cual es el resultado de la siguiente jerarquia de clases.
~~~java
class A {

    public void print() {
        System.out.println("A");
    }
}

class B extends A {

    @Override
    public void print() {
        System.out.println("B");
    }
}

class C extends B { }

class D extends C {

    @Override
    public void print() {
        System.out.println("D");
    }
}

C c = new C();
c.print()

// imprime -> B
~~~
5. Anula el metodo para que imprima el sonido correcto de cada animal.
~~~java
class Animal {

    public void say() {
        System.out.println("...An incomprehensible sound...");
    }
}

class Cat extends Animal {
    public void say() {
        System.out.println("meow-meow");
    }
}

class Dog extends Animal {
    public void say() {
        System.out.println("arf-arf");
    }
}

class Duck extends Animal {
    public void say() {
        System.out.println("quack-quack");
    }
}
~~~
6. Hay dos clases, seleccione la forma correcta de anular el metodo `calcSalary` en la clase `Programmer`.
~~~java
class Employee {

    protected long baseSalary;

    public Employee(long baseSalary) {
        this.baseSalary = baseSalary;
    }

    public long calcSalary() {
        return baseSalary;
    }
}

class Programmer extends Employee {

    private int yearsOfExperience;

    public Programmer(long baseSalary, int yearsOfExperience) {
        super(baseSalary);
        this.yearsOfExperience = yearsOfExperience;
    }

    public long calcSalary() {
        return yearsOfExperience * 500 + super.baseSalary;
    }
}
~~~
7. Tienes cinco clases. La clase `Shape` tiene un metodo `area()`. Este metodo no hace nada. Anula el metodo en todas las subclases. Los metodos anulados deben devolver un area de una figura en particular. Use campos de clase para esto.
~~~java
class Shape {
    public double area() {
        return 0;
    }
}

class Triangle extends Shape {
    double height;
    double base;

    public double area() {
        return (base * height) / 2;
    }
}

class Circle extends Shape {
    double radius;

    public double area() {
        return Math.PI * Math.pow(radius, 2);
    }
}

class Square extends Shape {
    double side;

    public double area() {
        return Math.pow(side, 2);
    }
}

class Rectangle extends Shape {
    double width;
    double height;

    public double area() {
        return width * height;
    }
}
~~~