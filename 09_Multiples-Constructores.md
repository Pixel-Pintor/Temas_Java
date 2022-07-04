# Multiples Constructores
A veces necesitamos inicializar todos los campos de un objetos al crearlo, pero hay casos en los que puede ser adecuado inicializar solo uno o varios campos. Afortunadamente, para este proposito, una clase puede tener varios constructores que asignan valores a los campos de diferentes maneras.  
Puede definir tantos constructores como necesite. Cada constructor debe tener un nombre que coinicida con el nombre de la clase, pero los parametros deben ser diferentes, esta situacion se llama **sobrecarga de constructores**.
~~~java
public class Robot {
    String name;
    String model;

    public Robot() {
        this.name = "Anonymous";
        this.model = "Unknown";
    }

    public Robot(String name, String model) {
        this.name = name;
        this.model = model;
    }
}

Robot anonymous = new Robot(); // name is "Anonymous", model is "Unknown"
Robot andrew = new Robot("Andrew", "NDR-114"); // name is "Andrew", model is "NDR-114"
~~~
La clase `Robot` tiene dos constructores:
- `Robot()` es un constructor sin argumentos que inicializa campos con valores predeterminados.
- `Robot(String name, String model)` toma dos parametros y los asigna a los campos correspondientes.  
Tenga en cuenta que no puede definir dos constructores con el mismo numeros, tipo y orden de los parametros.  
Tambien podemos invocar un costructor desde otros. Le permite inicializar una parte de un objeto por un constructor y otra por otros constructor. Llamar a un constructor dentro de otro se hace usando `this`.
~~~java
this(); // llama al constructor sin argumentos
this("arg1", "arg2"); // llama al constructor con dos argumentos
~~~
La declaracion para invocar un constructor debe ser la primera declaracion en el cuerpo de un constructor llamador.
~~~java
public class Robot {
    String name;
    String model;
    int lifetime;

    public Robot() {
        this.name = "Anonymous";
        this.model = "Unknown";
    }

    public Robot(String name, String model) {
        this(name, model, 20);
    }

    public Robot(String name, String model, int lifetime) {
        this.name = name;
        this.model = model;
        this.lifetime = lifetime;
        System.out.println("The third constructor is invoked");
    }
}
~~~
La clase tiene tres constructores y el segundo constructor invoca al tercero y le pasa los argumentos que recibido ademas de un tercer argumento definido en 20. El tercer constructor inicializa todos los campos de la clase.
~~~java
// instancia creada con el segundo constructor
Robot andrew = new Robot("Andrew", "NDR-114");
// imprime: The third constructor is invoked
~~~
---
## Ejercicios
1. Seleccione todas las formas correctas de crear una instancia de la clase `SomeClass`.
~~~java
class SomeClass {
    
    public SomeClass(int arg1) { }

    public SomeClass(int arg1, int arg2) { }
}

// Instanciaciones correctas
SomeClass instance = new SomeClass(2);
SomeClass instance = new SomeClass(2, 3);
~~~
2. La clase `SomeClass` tiene cuatro constructores, creaste una instancia de esta clase, encuentra los valores correctos de los campos de esta instancia.
~~~java
class SomeClass {

    int val = 50;
    String str = "default";

    public SomeClass() {
       this(100);
    }

    public SomeClass(int val) {
        val = val;
    }

    public SomeClass(String str) {
        this();
        this.str = "some-value";
    }

    public SomeClass(int val, String str) {
        this(str);
    }
}

// instancia creada
SomeClass instance = new SomeClass(300, "another-value");

// Ningun constructor asigna el numero usando this, entonces
// el valor permanece por defecto en 50
// El tercer constructor asigna el valor con this.str = "some-value"
// val = 50, str = "some-value"
~~~
3. Crea una clase con tres constructores que inicializen los campos.
~~~java
class Employee {

    final String UNK = "unknown";
    final int ZERO = 0;

    String name;
    int salary;
    String address;

    public Employee() {
        this.name = UNK;
        this.salary = ZERO;
        this.address = UNK;
    }

    public Employee(String name, int salary) {
        this.name = name;
        this.salary = salary;
        this.address = UNK;
    }

    public Employee(String name, int salary, String address) {
        this.name = name;
        this.salary = salary;
        this.address = address;
    }
}
~~~
