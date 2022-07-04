# Hacer referencia a objetos de subclase
Las clases estan organizadas en una jerarquia, lo que nos permite referirnos a los objetos de diferentes maneras. Una clase que se deriva de otra clase se llama subclase. Una clase de la que se deriva la subclase se denomina superclase.  
Hay dos formas de referirse a un objeto de subclase:  
1. **Usando la referencia de la subclase**: puede usar la referencia de la subclase para referirse a su objeto.
2. **Usando la referencia de superclase**: puede usar una variable de referencia de la superclase para referirse a cualquier objeto de subclase derivado de esa superclase porque una subclase es un caso especial de la superclase.
~~~java
class Person {

    protected String name;
    protected int yearOfBirth;
    protected String address;

    // getters y setters
}

class Client extends Person {

    protected String contractNumber;
    protected boolean gold;

    //getters y setters
}

class Employee extends Person {

    protected Date startDate;
    protected Long salary;

    // getters y setters
}
~~~
Como sabes, cada una de las clases presentadas tiene un constructor predeterminado sin argumentos. Ahora veamos ambos enfoques de la referencia en accion.  
1. **Referencia de subclase**. Podemos crear instancias de las subclases usando el constructor:
~~~java
Person person = new Person(); // la referencia es Person, el objeto es Person
Client client = new Client(); // la referencia es Client, el objeto es Client
Employee employee = new Employee(); // la referencia es Employee, el objeto es Employee
~~~
En este caso, usamos **referencias a subclases** porque los tipos de las referencias y el objeto creado son los mismos.  
2. **Referencias de superclase**. Al crear objetos usando el constructor, podemos referirnos a un objeto de subclase usando la referencias a la superclase:
~~~java
Person client = new Client(); // la referencia es Person, el objeto es Person
Person employee = new Employee(); // la referencia es Person, el objeto es Employee
~~~
En este caso, usamos la **superclase** porque las referencias tienen el tipo de la superclase y los tipos reales de objetos creados con subclase.  
La regla basica es asi:  
Si la clase A es una superclase de la clase B y la clase B es una superclase de la clase C, entonces una variable de la clase A puede hacer referencia a cualquier objeto derivado de esa clase (por ejemplo, objetos de la clase B y la clase C). Esto es posible porque cada objeto de subclase es un objeto de su superclase, pero no alreves.  
Podemos usar una referencias de superclase para cualquier objeto de subclase derivado de ella. Sin embargo, no podemos acceder a miembros especificos de la subclase a traves de la referencia de la clase base. Solo tenemos acceso a aquellos miembros del objeto que estan definidos por el tipo de referencia.  
Aqui cada clase tiene getters y setters para acceder a campos protegidos desde el exterior.
~~~java
Person employee = new Employee();

employee.setName("Ginger R. Lee"); // OK
employee.setYearOfBirth(1980); // OK
employee.setSalary(30000); // compile error, the base class "doesn't know" about the method
~~~
La superclase `Person` no tiene el metodo `setSalary()` de la clase `Employee`. No puede invocar el metodo a traves de la referencia de la superclase. La misma regla se aplica a los campos.  
Siempre puedes convertir un objeto de una subclase a su superclase. Tambien puede ser posibles convertir un objeto de un tipo de superclase a una subclase, pero solo si el objeto es una instancia de esta subclase; de lo contrario, un `ClassCastException` sera arrojado. Tenga cuidado al convertir una clase en su subclase.
~~~java
Person person = new Client();

Client clientAgain = (Client) person; // esta bien
Employee employee = (Employee) person; // arroja ClassCastException
~~~
Despues de convertir con exito una superclase en una subclase, podemos acceder a miembros especificos de la subclase.  
El uso de una referencia de superclase impone algunas restricciones en el acceso a los miembros de la clase. Lo que hicimos fue combinar ambos casos en un solo ejemplo. Nuestro metodo llamado `printName()` toma una serie de `Person` y muestra los nombres.
~~~java
public static void printName(Person[] persons) {
    for (Person person : persons) {
        System.out.println(person.getName());
    }
}
~~~
Este metodo funcionara para una matriz con objetos `Person`, `Client` y `Employee`.
~~~java
Person person = new Employee();
person.setName("Ginger R. Lee");

Client client = new Client();
client.setName("Pauline E. Morgan");

Employee employee = new Employee();
employee.setName("Lawrence V. Jones");

Person[] persons = { person, client, employee };

printNames(persons);

// resultado:
// Ginger R. Lee
// Pauline E. Morgan
// Lawrence V. Jones
~~~
Como puede ver, las referencias a clases base tienen aplicaciones en algunos casos practicos. Otros casos de uso de las referencias de superclase se consideraran en temas relacionados con el **polimorfismo**.
---
## Ejercicios
1. Tiene una jerarquia de clases que consta de tres clases, elija todas las asignaciones **incorrectas**.
~~~java
class A {}

class B extends A {}

class C extends B {}

A a1 = new A(); // correcto
A a2 = new C(); // correcto

B b1 = new B(); // correcto
B b2 = new A(); // incorrecto
B b3 = new C(); // correcto

C c1 = new B(); // incorrecto
C c2 = new C(); // correcto
~~~
2. Tienes una jerarquia que consiste en tres clase, elige las asignaciones correctas.
~~~java
class A {}

class B extends A {}

class C extends A {}

C c = new B(); // incorrecta
A a = new B(); // correcta
B b = new A(); // incorrecta
B b = new B(); // correcta
~~~
3. Hay una jerarquia que incluye tres clases, dado un objeto, selecciona todas las invocaciones **no validas** de metodos.
~~~java
class Animal {

    protected int age;

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}

class Pet extends Animal {

    protected String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}

class Cat extends Pet {
    
    protected String color;

    public String getColor() {
        return color;
    }

    public void setColor(String color) {
        this.color = color;
    }
}

// la referencia es Pet entonces no puede usar los metodos de Cat
Pet cat = new Cat();

cat.setName("Pharaoh"); // valido
cat.getName(); // valido
cat.setAge(5); // valido
cat.getAge(); // valido
cat.getColor(); // invalido
cat.setColor("Gray"); // invalido
~~~
4. Existe una jerarquia de clases, cuales clases se pueden poner en el lugar de `TYPE`.
~~~java
class A {}

class B extends A {}

class C extends B {}

class D extends B {}

class E extends D {}

TYPE variable = new D();

// A -> valida
// B -> valida
// C -> no valida
// D -> valida
// E -> no valida
~~~
