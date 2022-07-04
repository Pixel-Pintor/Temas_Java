# Herencia
La **herencia** es un mecanismo para derivar una nueva clase de otra clase (clase base). La nueva clase adquiere algunos campos y metodos de la clase base. La herencia es uno de los principios fundamentales de la programacion orientada a objetos. Permite a los desarrolladores crear jerarquias de clases convenientes y reutilizar el codigo existente.  
Cuando una clase es **derivada** de otra clase se denomina **subclase** y la clase de la que se deriva se llama **superclase**, para derivar una clase de otra se usa la palabra clave `extends`.
~~~java
class SuperClass { }
class SubClassA extends SuperClass { }
class SubClassB extends SuperClass { }
class SubClassC extends SubClassA { }
~~~
Hay puntos importantes sobre la herencia en Java:
- Java no admite la herencia de clases multiples, lo que significa que una clase solo puede heredar de un unica superclase.
- Una jerarquia de clases puede tener multiples niveles (clase `C` puede extender la clase `B` que extiende la clase `A`).
- Una superclase puede tener mas de una subclase.  

Una subclase hereda todos los campos y metodos publicos y protegidos de la superclase. Una sublclase tambien puede agregar nuevos campos y metodos. Los miembros heredados y agregados se utilizaran de la misma manera.  
Los constructores no se heredan, pero el constructor de la super clase se puede invocar desde la subclase usando la palabra clave `super`.  
Si desea que los miembros de la clase base sean accesibles desde todas las subclases pero no desde le codigo externo, use el modificador de acceso `protected`. La siguiente es la jerarquia de clases para una empresa se telecomunicaciones.
- La clase basica `Person` tiene campos para almacenar datos comunes: nombre, año de nacimiento y direccion.
- La clase `Client` tiene campos adicionales para almacenar el numero de contrato y el estado.
- La clase `Employee` almacena la fecha de inicio del trabajo para la empresa y el salario.
- La clase `Programmer` tiene una variedad de lenguajes de programacion que usa el programador.
- La clase `Manager` puede terne una sonrisa deslumbrante.
~~~java
class Person {
    protected String name;
    protected int yearOfBirth;
    protected String address;

    // public getters and setters for all fields here
}

class Client extends Person {
    protected String contractNumber;
    protected boolean gold;

    // public getters and setters for all fields here
}

class Employee extends Person {
    protected Date startDate;
    protected Long salary;

    // public getters and setters for all fields here
}

class Programmer extends Employee {
    protected String[] programmingLanguages;

    public String[] getProgrammingLanguages() {
        return programmingLanguages;
    }

    public void setProgrammingLanguages(String[] programmingLanguages) {
        this.programmingLanguages = programmingLanguages;
    }
}

class Manager extends Employee {
    protected boolean smile;

    public boolean isSmile() {
        return smile;
    }

    public void setSmile(boolean smile) {
        this.smile = smile;
    }
}
~~~
Esta jerarquia tiene dos niveles y cinco clase en total. Todos los campos son `protected` lo que significa que son visibles para las subclases.  
Vamos a crear un objeto de la clase `Programmer` y rellenar los campos heredados utilizando los **setters** heredados. Para leer los valores de los campos podemos usar **getters** heredados.
~~~java
Programmer p = new Programmer();

p.setName("John Elephant");
p.setYearOfBirth(1985);
p.setAddress("Some street, 15");
p.setStartDate(new Date());
p.setSalary(500_000L);
p.setProgrammingLanguages(new String[] { "Java", "Scala", "Kotlin" });

System.out.println(p.getName()); // John Elephant
System.out.println(p.getSalary()); // 500000
System.out.println(Arrays.toString(p.getProgrammingLanguages())); // [Java, Scala, Kotlin]
~~~
La **herencia** proporciona un mecanismo poderoso para la reutilizacion de codigo y la escritura de jerarquias convenientes.  
Si una clase se declara con la palabra clave `final`, no puede tener subclases en absoluto.
~~~java
final class SuperClass { }
~~~
---
## Ejercicios
1. Necesitas definir una jerarquia de clases para un sistema hospitalario.
~~~java
class Person { ... }

class Employee extends Person { ... }

class Doctor extends Employee { ... }

class Patient extends Person { ... }
~~~
2. Hay dos clases `Pet` y `Cat`, cuales miembros de la clase `Pet` puede heredar la clase `Cat`?
~~~java
class Pet {
        
    private String birthDate; // No puede por que es un campo privado
    protected String name; // Si puede
    
    public String getBirthDate() { // si puede
        return birthDate;
    }
        
    public String getName() { // si puede
        return name;
    }

    private void setName(String name) { // No puede por que es un metod privado
        this.name = name;
    }
}

class Cat extends Pet { ... }
~~~
3. Tiene una jerarquia de clases, ambas clases estan en diferentes paquetes, cual modificador de acceso puede usarse para que el codigo compile?
~~~java
public class BaseClass {
    ??? int a; // puede usar: protected y public
}

public class DerivedClass extends BaseClass {
    private int b; 

    public int sum() {
        return a + b;
    }
}
~~~
