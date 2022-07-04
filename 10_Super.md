# La palabra Super
A veces, cuando definimos una nueva subclase, necesitamos acceder a miembros o constructores de su superclase. Java proporciona una palabra clase especial `super` para hacer esto. Esta palabra se puede utilizar en varios casos:
- Para acceder a los campos de instancia de la clase padre.
- Para invocar metodos de la clase padre.
- Para invocar constructores de la clase principal (sin argumentos o parametrizados).
---
## Acceso a campos y metodos de superclase
Puede usar `super` para acceder a metodos de instancia o campos de la superclase, en cierto sentido es similiar a `this`, pero se refiere al objeto de clase del padre inmediato.
~~~java
class SuperClass {
    
    protected int field;

    protected int getField() {
        return field;
    }
    
    protected void printBaseValue() {
        System.out.println(field);
    }
}

class SubClass extends SuperClass {
    
    protected int field;

    public SubClass() {
        this.field = 30;  // inicializa el campo de la subclase
        field = 30;       // tambien inicializa el campo de la subclase
        super.field = 20; // inicializa el campo de la superclase
    }

    /**     
     * Imprime el valor de SuperClass y en valor de SubClass
     */
    public void printSubValue() {
        super.printBaseValue(); // invoca el metodo de SuperClass, super es opcional aqui
        System.out.println(field);
    }
}
~~~
En el constructor de `SubClass`, el campo de la superclase se inicializa con la palabra `super`. Necesitamos usar la palabra aqui porque el campo de la subclase oculta el campo de la clase base con el mismo nombre.
---
## Invocando constructor de superclase
Las subclase no heredan los constructores, pero se puede invocar un constructor de superclase desde una subclase usando `super` entre **parentesis**. Tambien podemos pasar algunos argumentos al constructor de la superclase.
- La invocacion `super(...)` debe ser la primera declaracion en un constructor de subclase.
- El constructor predeterminado de una subclase llama automaticamente al constructor sin argumentos de la superclase.
~~~java
class Person {

    protected String name;
    protected int yearOfBirth;
    protected String address;

    public Person(String name, int yearOfBirth, String address) {
        this.name = name;
        this.yearOfBirth = yearOfBirth;
        this.address = address;
    }

    // getters and setters
}

class Employee extends Person {

    protected Date startDate;
    protected Long salary;

    public Employee(String name, int yearOfBirth, String address, Date startDate, Long salary) {
        super(name, yearOfBirth, address); // invocando un constructor de la superclase
        
        this.startDate = startDate;
        this.salary = salary;
    }

    // getters and setters
}
~~~
El constructor de la clase `Employee` invoca al constructor de la clase principal para asignar valores a los campos pasados. En cierto modo, se parece a trabajar con multiples constructores usando `this()`.
---
## Ejercicios
1. Crear una jerarquia de clase y extiende el constructor de la superclase en los constructores de las subclases.
~~~java
class BankAccount {

    protected String number;
    protected Long balance;

    public BankAccount(String number, Long balance) {
        this.number = number;
        this.balance = balance;
    }
}

class CheckingAccount extends BankAccount {
    double fee;

    CheckingAccount(String number, Long balance, double fee) {
        super(number, balance);
        this.fee = fee;
    }
}

class SavingsAccount extends BankAccount {
    double interestRate;

    SavingsAccount(String number, Long balance, double interestRate) {
        super(number, balance);
        this.interestRate = interestRate;
    }
}
~~~
2. Define tres clases que representen empleados de una empresa tecnologica utilizando la herencia y la palabra `super`.
~~~java
class Employee {

    // write fields
    String name;
    String email;
    int experience;

    // write constructor
    public Employee(String name, String email, int experience) {
        this.name = name;
        this.email = email;
        this.experience = experience;
    }

    // write getters

    public String getName() {
        return name;
    }

    public String getEmail() {
        return email;
    }

    public int getExperience() {
        return experience;
    }
}

class Developer extends Employee {

    // write fields
    String mainLanguage;
    String[] skills;

    // write constructor
    public Developer(String name, String email, int experience, String mainLanguage, String[] skills) {
        super(name, email, experience);
        this.mainLanguage = mainLanguage;
        this.skills = skills;
    }

    // write getters
    public String getMainLanguage() {
        return mainLanguage;
    }

    public String[] getSkills() {
        return skills;
    }
}

class DataAnalyst extends Employee {

    // write fields
    boolean phd;
    String[] methods;

    // write constructor
    public DataAnalyst(String name, String email, int experience, boolean phd, String[] methods) {
        super(name, email, experience);
        this.phd = phd;
        this.methods = methods;
    }

    // write getters

    public boolean isPhd() {
        return phd;
    }

    public String[] getMethods() {
        return methods;
    }
}

class Main {
    public static void main(String[] args) {
        String[] skills = {"git", "Scala", "JBoss", "UML"};
        Developer developer = new Developer("Mary", "mary@mail.com", 3, "Java", skills);

        String[] methods = {"neural networks", "decision tree", "bayesian algorithms"};
        DataAnalyst analyst = new DataAnalyst("John", "jhon@mail.com", 2, true, methods);
    }
}
~~~
3. Que imprimira la instancia de esta jerarquia de clase.
~~~java
class A {
    
    protected int n = 10;
    protected String s = "abc";
    protected char ch = 'q';
    
    public A(int n) {
        this.n = n;
    }
}

class B extends A {
    
    protected int n = 20;
    protected String s = "str";
    protected char ch = 'z';
    
    public B(int n, String s, char ch) {
        super(n);
        this.s = s;
        super.ch = ch;
    }

    public void print() {
        System.out.println(super.n + " " + super.s + " " + ch);
    }
}

B b = new B(100, "txt", 'k');
b.print();

// imprime: 100 abc z
~~~
