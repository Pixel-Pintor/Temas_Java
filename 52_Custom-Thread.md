# Custom Threads
El hilo **main** es un punto de partida desde el que puede generar nuevos subprocesos para realizar sus tareas. Para hacer eso, debe escribir el codigo que se ejecutara en un hilo separado y luego iniciarlo.  
Java tiene dos forma principales de crear un nuevo hilo que realiza una tareas que necesita.
- Extender la clase `Thread` y anulado el metodo `run`.
~~~java
class HelloThread extends Thread {

    @Override
    public void run() {
        String helloMsg = String.format("Hello, i'm %s", getName());
        System.out.println(helloMsg);
    }
}
~~~
- Implementando la interfaz `Runnable` y pasando la implementacion al constructor de la clase `Thread`.
~~~java
class HelloRunnable implements Runnable {

    @Override
    public void run() {
        String threadName = Thread.curretThread().getName();
        String helloMsg = String.forma("Hello, i'm %s", threadName);
        System.out.println(helloMsg);
    }
}
~~~
En ambos casos, debe anular el metodo `run`, que es un metodo Java normal y contiene codigo para realizar una tarea. Que enfoque elegir depende de la tarea y de sus preferencias. Si extiendes la clase `Thread`, puede aceptar campos y metodos de la clase base, pero no puede extender otras clases ya que Java no tiene herencia multiple de clases.  
Aqui hay dos objetos obtenidos por los enfoques descritos anteriormente en consecuencia.
~~~java
Thread t1 = new HelloThread(); // una subclase de Thread
Thread t2 = new Thread(new HelloRunnable()); // pasando runnable
~~~
Y aqui hay otra forma de especificar un nombre para su subproceso pasandolo al constructor.
~~~java
Thread myThread = new Thread(new HelloRunnable(), "my-thread");

// usando una expresion lambda
Thread t3 = new Thread(() -> {
    System.out.println(String.format("Hello, i'm %s", Thread.currentThread().getName()));
});
~~~

## Iniciar Threads
La clase `Thread` tiene un metodo llamado `start()` que se utiliza para iniciar un hilo. En alguno momento despues de invocar este metodo, el metodo `run()` se invocara automaticamente, pero no sucedera de inmediato.  
Supongamos que dentro de **main** crear un objeto de `HelloThread` llamado `t` y lo inicializa.
~~~java
Thread t = new HelloThread(); // objeto que representa una hilo
t.start();

// imprime
// Hello, i'm Thread-0
~~~
Como puede ver, hay cierto retraso enter el inicio de un hilo y el momento en que realmente comienza a funcionar (ejecutarse).  
De forma predeterminada, un nuevo subproceso se ejecuta en **no daemon**. Recordatorio: la diferencia entre **daemon** y **no daemon** es que JVM no finalizara el program en ejecucion mientras sea **no daemon**, mientras que los **daemon** no evitaran que la JVM finalice.  
A pesar de que dentro de un unico subproceso todas las declaraciones se ejecutan secuencialmente, es imposible determinar el orden relativo de las declaraciones entre multiples subprocesos sin medidas adicionales que no consideraremos en esta leccion.
~~~java
public class StartingMultipleThreads {

    public static void main(String[] args) {
        Thread t1 = new HelloThread();
        Thread t2 = new HelloThread();

        t1.start();
        t2.start();

        System.out.println("Finished");
    }
}

// el orden de visualizacion de los mensajes puede ser diferentes
// Hello, i'm Thread-1
// Finished
// Hello, i'm Thread-0

// Finished
// Hello, i'm Thread-0
// Hello, i'm Thread-1
~~~
Esto significa que a pesar de que llamamos a `start()` secuencialmente para cada subproceso, no sabemos cuando realmente se llamara al metodo `run()`.  

## Un programa multiproceso simple
Consideremos un programa simple de multiples procesos con dos subprocesos. El primer hilo lee numeros de la entrada estandar e imprime sus cuadrados. Al mismo tiempo, el **main** ocasionalmente imprime mensajes en la salida estandar. Ambos hilos funcionan simultaneamente.  
Aqui hay un hilo que lee numeros en un bucle y los eleva al cuadrado. Tiene una declaracion de interrupcion para detener el ciclo si el numero dado es 0.
~~~java
class SquareWorkerThread extends Thread {
    private final Scanner scanner = new Scanner(System.in);

    public SquareWorkerThread(String name) {
        super(name);
    }

    @Override
    public void run() {
        while (true) {
            int number = scanner.nextInt();
            if (number == 0) {
                break;
            }
            System.out.println(number * number);
        }
        System.out.println(String.format("%s finished", getName()));
    }
}
~~~
Dentro del metodo `main`, el programa inicia un objeto de `SquareWorkerThread` que escribe mensajes en la salida estandar del subproceso **main**.
~~~java
public class SimpleMultithreadedProgram {

    public static void main(String[] args) {
        Thread worker = new SquareWorkerThread("square-worker");
        worker.start(); // start a worker (not run!)

        for (long i = 0; i < 5_555_555_543L; i++) {
            if (i % 1_000_000_000 == 0) {
                System.out.println("Hello from the main!");
            }
        }
    }
}
~~~
Aqui hay un ejemplo de entradas y salidas con comentarios:
~~~java
Hello from the main!   // the program outputs it
2                      // the program reads it
4                      // the program outputs it
Hello from the main!   // outputs it
3                      // reads it
9                      // outputs it
5                      // reads it
Hello from the main!   // outputs it
25                     // outputs it
0                      // reads it
square-worker finished // outputs it
Hello from the main!   // outputs it
Hello from the main!   // outputs it

Process finished with exit code 0
~~~
Como puede ver, este programa realiza dos tareas **"al mismo tiempo"**: una en el **main** y otra en el hilo **work**. Puede que no sea **"el mismo tiempo"** en el sentido fisico, sin embargo, ambas tareas tienen tiempo para ejecutarse.
---

## Ejercicios
1. Seleccione todas las declaraciones correctas sobre el inicio de hilos.
- Si invocamos `run()` en una instancia de la clase `Thread`, no se generaran nuevo hilos.
- Una instancia de la clase `Thread` tiene metodos `start()` y `run()`. Si invocamos el primero, llama al segundo en un nuevo hilo.
2. Cual es la diferencia entre los metodos `start()` y `run()`.
- El metodo `start()` crea un nuevo subproceso y llama al metodo `run()` en el nuevo subproceso. El metodo `run()` no llama a `start()`.
3. Hay una clase que extiende `Thread` y anula el metodo `run()`, seleccione la declaracion correcta sobre el orden de las lineas.
~~~java
class MyThread extends Thread {

    public MyThread(String name) {
        super(name);
    }

    @Override
    public void run() {
        System.out.println("Hello from " + getName());
    }
}

MyThread t1 = new MyThread("My-thread-1");
t1.start();

MyThread t2 = new MyThread("My-thread-2");
t2.start();

System.out.println("Finished");

// Las lineas se podrian imprimir en cualquier orden
~~~
4. Tienes una clase llamada `PrintMessageTask` que implementa la interfaz `Runnable`. Esta clase se declara para imprimir un mensaje en la salida estandar en un nuevo hilo. En el metodo `main`, hay un objeto de esta clase y un subproceso se basa en este objeto. Cuando se imprimira el mensaje?
~~~java
Runnable task = new PrintMessageTask("Hi, I'm good.");
Thread worker = new Thread(task);

// Se imprimira un tiempo despues de iniciar el worker usando el metodo start()
~~~
5. Si tenemos una instancia de la clase `Thread` llamada `thread`. No podemos invocar `thread.start()` varias veces.
6. Escribe un servicio que tome un mensaje y el numero de repeticiones como parametros del constructor e imprima el mensaje en la salida estandar el numero de veces especificado.
~~~java
class MessageNotifier extends Thread {

    String message;
    int reps;

    public MessageNotifier(String msg, int repeats) {
        this.message = msg;
        this.reps = repeats;
    }

    @Override
    public void run() {
        for (int i = 0; i < reps; i++) {
            System.out.println(message);
        }
    }
}
~~~
7. Escriba una clase llamada `NumbersThread` que amplie `Thread`. La clase debe tener un constructor que tome dos numeros enteros e imprima los numeros del rango.
~~~java
class NumbersThread extends Thread {

    private int from;
    private int to;

    public NumbersThread(int from, int to) {
        this.from = from;
        this.to = to;
    }

    @Override
    public void run() {
        for (; from <= to; from++) {
            System.out.println(from);
        }
    }
}
~~~
8. Implemente un metodo que tome un array de `Runnables`. Debe iniciar cada runnable en un nuevo hilo.
~~~java
class Starter {

    public static void startRunnables(Runnable[] runnables) {
        for (Runnable runnable : runnables) {
            new Thread(runnable).start();
        }
    }
}
~~~
9. Escriba la clase `NumbersFilter` que amplie el metodo `run()` de `Thread`. Debe leer numeros enteros de la entrada estandar. Si el numero es par, el worker debe imprimirlo en la salida estandar, si un numero es 0, el worker debe detenerse.
~~~java
class NumbersFilter extends Thread {
    private final Scanner scanner = new Scanner(System.in);

    @Override
    public void run() {
        int num;
        do {
            num = scanner.nextInt();
            if (num % 2 == 0 && num != 0) {
                System.out.println(num);
            }
        } while (num != 0);
    }
}
~~~
10. Cree tres subprocesos utilizando instancias de `RunnableWorker`. Iniciar todos los hilos creados, el metodo `run()` de cada instancia debe ejecutarse en un nuevo hilo.
~~~java
class RunnableWorker implements Runnable {
    
    @Override
    public void run() {
        // the method does something
    }
}

public class Main {

    public static void main(String[] args) {
        final int THREADS = 3;
        for (int i = 1; i <= THREADS; i++) {
            new Thread(new RunnableWorker(), "worker-" + (i)).start();
        }
    }
}

// Don't change the code below       
class RunnableWorker implements Runnable {

    @Override
    public void run() {
        final String name = Thread.currentThread().getName();

        if (name.startsWith("worker-")) {
            System.out.println("too hard calculations...");
        } else {
            return;
        }
    }
}
~~~
11. Crea dos instancias de la clase y inicia los hilos creados. El metodo `run()` de cada instancia debe ejecutarse en un nuevo hilo.
~~~java
class WorkerThread extends Thread {
    
    @Override
    public void run() {
        // the method does something
    }
}

public class Main {
    public static void main(String[] args) {
        Thread t1 = new WorkerThread("worker-I");
        Thread t2 = new WorkerThread("worker-II");
        t1.start();
        t2.start();
    }
}

class WorkerThread extends Thread {
    private static final int NUMBER_OF_LINES = 3;

    public WorkerThread(String name) {
        super(name);
    }

    @Override
    public void run() {
        final String name = Thread.currentThread().getName();

        if (!name.startsWith("worker-")) {
            return;
        }

        for (int i = 0; i < NUMBER_OF_LINES; i++) {
            System.out.println("do something...");
        }
    }
}
~~~