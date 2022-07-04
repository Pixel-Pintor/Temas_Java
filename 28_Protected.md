# Modificador Protected
Como sabe existen 4 modificadores:
- `private`: disponibles solo para la clase en si.
- `default`: disponibles para clases del mismo paquete **paquete-privado**.
- `protected`: disponible para clases del mismo paquete y las clases extendidas.
- `public`: disponibles en todas partes.  
Ya hemos considerado la mayoria de ellos, pero queda el mas interesante: el modificador de acceso `protected`. Este modificador determina que solo las subclases y cualquier clase del mismo paquete pueden usar un miembro de clase.
---
## Protected vs Default
Puede pensar en clases del mismo paquete como vecinos y las subclases como hijos de una clase particular. Hay algunas cosas que puede compartir con los vecinos, estas cosas y acciones serian el paquete privado (`default`). Tambien hay cosas que pueden hacer los niños y amigos cercanos, estas cosas seran `protected`.
## Protected vs Private
Si una variable, un metodo o una clase es utilizada solo por la clase misma, entonces es `private`, de lo contrario es `protected`. Si no esta seguro de si el metodo es util para otras clases, es mejor hacerlo privado primero y expandir su disponibilidad mas adelante si es necesario.
## Ejemplo
En el siguiente ejemplo, el paquete `org.hyperskill.bluetooth` tiene tres clases: `Laptop`, `SmarthPhone` y `SmartWatch`. Todos los dispositivos del paquete se pueden conectar a traves de Bluetooth. `Laptop` tiene un metodo `receiveInfo()`, responsable de obtener cualquier informacion de los dispositivos conectados.
~~~java
public class Laptop {

    private String info;

    void receiveInfo(String info) {
        this.info = info;
    }
}
~~~
La clase `Laptop` tiene un solo campo `info` a la que no se puede acceder directamente ya qye esta declarada como privada. Pero todas las clases del mismo paquete pueden acceder a el invocando el metodo `receiveInfo()` que se declara como paquete privado (sin modificador).  
Consideramos que `SmartPhone` y `SmartWatch` las clases se extienden igual con la clase `MobileGadget` con el metodo `printNotification`:
~~~java
public class MobileGadget {

    protected void printNotification(String data) {
        System.out.println(data);
    }
}
~~~
El metodo `printNotification` es accesible para todas las subclases de esta clase, asi como para todas las clase en el mismo paquete (incluida la clase `Laptop`). La clase `SmartPone` puede acceder al metodo `receiveInfo` de la clase `Laptop` y el metodo `printNotification` de la clase `MobileGadget`.
~~~java
public class SmartPhone extends MobileGadget {

    private Laptop connectedLaptop;

    public SmartPhone() {
        this.connectedLaptop = new Laptop();
    }

    private void sendInfoToLaptop(String data) {
        printNotification("Sending data to laptop: " + data);
        connectedLaptop.receiveInfo(data);
    }
}
~~~
La clase `SmartWatch` tiene un metodo privado `countHeartRate`, que no esta disponible desde otras clases (incluoso desde una clase "hermana" `SmartPhone`). Tambien utiliza el metodo `Laptop` de recibir datos y el metodo de un padre para imprimir la notificacion:
~~~java
public class SmartWatch extends MobielGadget {

    private int avgHeartRate;
    private Laptop connectedLaptop;

    public SmartWatch() {
        this.avgHeartRate = 75;
        this.connectedLaptop = new Laptop();
    }

    private int countHeartRate() {
        System.out.println("Counting heart rate");
        return aveHeartRate;
    }

    private void sendInfoToLaptop(String data) {
        printNotification("Sending data to laptop: " + data);
        connectedLaptop.receiveInfo(data);
    }
}
~~~
---
## Ejercicios
1. Cual sera la salida del siguiente codigo.
~~~java
public class Tiger {

    private String name;

    public Tiger(String name) {
        this.name = name;
    }

    void sayHello() {
        System.out.println("Rrrrrr!);
    }

    protected void run() {
        System.out.println(this.name + " is running!");
    }
}

public class Main {
    public static void main(String[] args) {

        Tiger tiger = new Tiger();
        tiger.run();   
    }
}

// la salida es 'compile error' por que no declaro modificador en el metodo sayHello()
~~~
2. Existen cuatro clases dentro de diferentes paquetes, seleccione las llamadas a metodos legales.
~~~java
package birds;

/**
 * Bird class
 */
public class Bird {

    public void fly() {
        System.out.println("Goodbye, I'm flying away");
    }

    protected void sing() {
        System.out.println("I'm singing!");
    }
}

// --------------
package mammals.felines;

import mammals.Mammal;

/**
 * Cat class
 */
public class Cat extends Mammal { }

// -----------------

package mammals;

/**
 * Mammal class
 */
public class Mammal {

    protected void motherChild() {
        System.out.println("My baby was just born");
    }

    void yell() {
        System.out.println("I am a mammal!");
    }
}

// -----------------

package mammals;

import birds.Bird;
import mammals.Mammal;
import mammals.felines.Cat;

/**
 * The main class
 */
public class Main {

    public static void main(String[] args) {
        Cat cat = new Cat();
        Bird bird = new Bird();
        Mammal mammal = new Mammal();
    }
}

// cat.yell() -> invalido
// mammal.sing() -> invalido
// bird.fly() -> valido
// cat.motherChild() -> valido
~~~