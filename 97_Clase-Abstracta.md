# Clase Abstracta

A veces, tiene un conjunto de campos y metodos que necesita reutilizar en todas las clase dentro de una jerarquia. Es posible colocar todos los miembros comunes en una clase base especial y luego declarar subclases que puedan acceder a estos miembros. Al mismo tiempo, no necesita crear objetos de la clase base. Para lograrlo, puede utilizar una *clase abstracta* como clase base de la jerarquia.

## Que es una clase abstracta?

Una *clase abstracta* es una clase declarada con la palabra clave **abstract**. Representa un concepto abstracto que se utiliza como clase base para las subclases.  
Las clases abstractas tienen algunas caracteristicas especiales:

- Es imposible crear una instancia de una clase abstracta
- Una clase abstracta puede contener metodos abstractos que deben implementarse en subclases no abstractas
- Puede contener campos y metodos no abstractos (incluidos los estaticos)
- Una clase abstracta puede extender otra clase, incluida la abstracta
- Puede contener un constructor

Como puede ver, una clase abstracta tiene dos diferencias principales con las clases regulares (concretas): *sin instancia* y *metodos abstractos*.  
Los metodos abstractos se declaran agregando la palabra clave **abstract**. Tiene una declaracion (modificadores, un tipo de retorno y una firma) pero no tienen una implementacion. Cada subclase concreta (no abstracta) debe implementar estos metodos.

## Ejemplo

Aqui hay una clase abstracta **Pet**:

~~~java
public abstract class Pet {

    protected String name;
    protected int age;

    protected Pet(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public abstract void say(); // metodo abstracto
}
~~~

La clase tiene dos campos, un constructor y un metodo abstracto.  
Ya que **Pet** es una clase abstracta, no podemos crear instancias de esa clase.  
El metodo **say()** se declara abstracto porque, a este nivel de abstraccion, se desconoce su implementacion. Subclases concretas de la clase. **Pet** debe tener una implementacion de este metodo.  
A continuacion se muestran dos subclases concretas de **Pet**. Puedes ver que anulan el metodo abstracto:

~~~java
class Cat extends Pet {

    // puede tener metodos adicionales

    public Cat(String name, int age) {
        super(name, age);
    }

    @Override
    public void say() {
        System.out.println("Meow!");
    }
}

class Dog extends Pet {

    // puede tener metodos adicionales

    public Dog(String name, int age) {
        super(name, age);
    }

    @Override
    public void say() {
        System.out.println("Woof!");
    }
}
~~~

Podemos crear instancias de estas clases y llamar al metodo **say()**.

~~~java
Dog dog = new Dog("Boss", 5);
Cat cat = new Cat("Tiger", 2);

dog.say(); // imprime "Woof!"
cat.say(); // imprime "Meow!"
~~~

No olvide que Java no admite herencia multiple para clases. Por lo tanto, una clase puede extender solo una clase abstracta.

---

## Ejemplos

1. Escriba una nueva clase abstracta llamda **BaseEntity**, que tenga los metodos y campos comunes a todas las clases.

    ~~~java
    abstract class BaseEntity {

        protected long id;
        protected long version;

        public long getId() {
            return id;
        }

        public void setId(long id) {
            this.id = id;
        }

        public long getVersion() {
            return version;
        }

        public void setVersion(long version) {
            this.version = version;
        }
    }

    class User extends BaseEntity {

        protected String name;

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }
    }

    class Visit extends BaseEntity {

        protected User user;

        protected WebSite site;

        public User getUser() {
            return user;
        }

        public void setUser(User user) {
            this.user = user;
        }

        public WebSite getSite() {
            return site;
        }

        public void setSite(WebSite site) {
            this.site = site;
        }
    }

    class WebSite extends BaseEntity {

        protected String url;

        public String getUrl() {
            return url;
        }

        public void setUrl(String url) {
            this.url = url;
        }
    }
    ~~~

2. Seleccione todos los metodos que deben anularse en la clase **C**.

    ~~~java
    abstract class A { 
    
        public static void staticFoo() { }
    
        public void instanceBar() { }
    
        public abstract void abstractFoo();
    
        public abstract void abstractBar();
    }

    abstract class B extends A { 
    
        public static void anotherStaticFoo() { }
    
        public void anotherInstanceBar() { }

        @Override
        public void abstractBar() { }
    
        public abstract void qq();
    }

    class C extends B { 
        // qq()
        // abstractFoo()
    }
    ~~~

3. Que puede tener una clase abstracta?

    - Metodos estaticos
    - Metodos privados
    - Metodos abstractos

4. Cual clase tiene una declaracion incorrecta?

    ~~~java
    abstract class A { }
    abstract class B extends A { }
    final abstract class C { } // INCORRECTO
    class D extends A { }
    ~~~

5. Declaraciones correctas sobre *clases abstractas*.

    - Una clase abstracta puede tener varios metodos abstractos
    - Una clase abstracta puede tener metodos privados

6. Cuales son las caracteristicas de los *metodos abstractos* en las clases abstractas?

    - Se declaran con la palabra clave **abstract**
    - No tienen cuerpo

7. Necesita declarar e implementar tres clases: **Triangle**, **Rectangle** y **Circle**. Las clases deben ampliar la clase **Shape** e implementar todos los metodos abstractos.

    ~~~java
    abstract class Shape {

        abstract double getPerimeter();

        abstract double getArea();
    }

    class Triangle extends Shape {
        double a;
        double b;
        double c;
    
        public Triangle(double a, double b, double c) {
            super();
            this.a = a;
            this.b = b;
            this.c = c;
        }
    
        public double getPerimeter() {
            return this.a + this.b + this.c; 
        }
    
        public double getArea() {
            double semiPerimeter = getPerimeter() / 2;
            return Math.sqrt(semiPerimeter * (semiPerimeter - this.a) 
                            * (semiPerimeter - this.b) * (semiPerimeter - this.c));
        }
    }

    class Rectangle extends Shape {
        double a;
        double b;
    
        public Rectangle(double a, double b) {
            super();
            this.a = a;
            this.b = b;
        }
    
        public double getPerimeter() {
            return 2 * this.a + 2 * this.b;
        }
    
        public double getArea() {
            return this.a * this.b;
        }
    }

    class Circle extends Shape {
        public double radius;
    
        public Circle(double radius) {
            super();
            this.radius = radius;
        }
    
        public double getPerimeter() {
            return 2 * Math.PI * this.radius;
        }
    
        public double getArea() {
            return Math.PI * Math.pow(this.radius, 2);
        }
    }
    ~~~
