# Metodos de instancia
Todos los metodos se pueden dividir en dos grupos: **instancia** y **estaticos**.  
Veamos el codigo a continuacion. Aqui tenemos una clase llamada `Human` con dos campos y dos metodos.
~~~java
class Human {
    String name; 
    int age;

    public static void printStatic() {
        System.out.println("It's a static method");
    }

    public void printInstance() {
        System.out.println("It's an instance method");
    }
}
~~~
Como ves, los metodos `printStatic` y `printInstance` tienen diferencias en la declaracion. Si un metodo no tiene la palabra `static`, entonces es un metodo de **instancia**.
---
## Comprension: Estatica e Instancia
Para invocar un metodo estatico no necesitamos crear un objeto. Simplemente llamamos al metodo con el nombre de la clase.
~~~java
public static void main(String[] args) {
    Human.printStatic();
}
~~~
El metodo de instancia requiere una invocacion diferente. Primero tenemos que crear un objeto, de lo contrario no hay forma de usar un metodo de instancia. Aqui llamamos el metodo `printInstance` para dos objetos diferentes.
~~~java
public static void main(String[] args) {

    Human peter = new Human();
    peter.printInstance(); 

    Human alice = new Human();
    alice.printInstance();
}
~~~
Entonces podemos decir que un metodo de instancia es un metodo que pertenece a cada objeto que creamos de la clase en particular.  
---
## Metodos de instancia: Caracteristicas
Los metodos de instancia tienen la gran ventaja de que pueden acceder a los campos de la clase.
~~~java
class Human {
    String name;
    int age;

    public static void averageWorking() {
        System.out.println("An average human works 40 hours per week.");
    }

    public void work() {
        System.out.println(this.name + " loves working!");
    }

    public void workTogetherWith(Human other) {
        System.out.println(this.name + " loves working with " + other.name + '!');
    }
}

public static void main(String[] args) {
        
    Human.averageWorking(); // "An average human works 40 hours per week."

    Human peter =  new Human();
    peter.name = "Peter";
    peter.work(); // "Peter loves working!"

        
    Human alice =  new Human();
    alice.name = "Alice";
    alice.work(); // "Alice loves working!"

    peter.workTogetherWith(alice); // "Peter loves working with Alice"
}
~~~
La palabra `this` representa una instancia particular de la clase.  
Tenemos diferentes salidas para el metodo `work` porque dos objetos diferentes tienen valores diferentes para `name`. En `workTogetherWith` la palabra clave `this` nos permite acceder a un campo del objeto en particular y distinguirlo del mismo campo de otro objeto.
---
## Ejercicios
1. Agregar metodos de instancia a la clase `Car`.
~~~java
class Car {

    int yearModel;
    String make;
    int speed;
    final int changeSpeed = 5;

    void accelerate() {
        this.speed += changeSpeed;
    }

    void brake() {
        this.speed -= changeSpeed;
        this.speed = this.speed < 0 ? 0 : this.speed;
    }
}
~~~
2. Necesita metodos de instancia para poder calcular la suma y la diferencia de dos numeros complejos.
~~~java
class Complex {

    double real;
    double image;

    public void add(Complex num) {
        this.real = this.real + num.real;
        this.image = this.image + num.image;
    }

    public void subtract(Complex num) {
        this.real = this.real - num.real;
        this.image = this.image - num.image;
    }
}
~~~
3. Simulacion de un reloj.
~~~java
class Clock {

    int hours = 12;
    int minutes = 0;

    void next() {
        final int maxMinute = 59;
        final int maxHours = 12;

        if (minutes == maxMinute) {
            this.hours = this.hours == maxHours ? 1 : this.hours + 1;
            this.minutes = 0;
        } else {
            this.minutes += 1;
        }
    }
}
~~~
4. El paciente necesita un doctor.
~~~java
class Patient {

    String name;

    void say() {
        String str = String.format("Hello, my name is %s, I need a doctor.", this.name);
        System.out.println(str);
    }
}
~~~~