# Gestions de Threads
Ya hemos aprendido como iniciar un nuevo hilo simplemente invocando el metodo `start` en un objeto correspondiente. Sin embargo, a veces es necesario administrar el ciclo de vida de un subproceso mientras esta funcionando en lugar de simplemente iniciarlo y dejarlo asi.  

## Sleep
El metodo estatico `Thread.sleep()` hace que el subproceso que se esta ejecutando suspenda la ejecucion durante el numero especificado de milisegundos. Este es un medio eficiente de hacer que el tiempo del procesador este disponible para los otros subprocesos de una aplicaciones u otras aplicaciones que puedan estar ejecutandose en la computadora.
~~~java
System.out.println("Started");
Thread.sleep(2000L); // suspende el hilo actual
System.out.println("Finished");
~~~
Al principio, imprime "Started". Luego el hilo se suspende 2000 milisegundos. Eventualmente, el subproceso se despierta e imprime "Finished".  
Otra forma de hacer que el hilo actual duerma es usar la clase `TimeUnit`:
- `TimeUnit.MILISECONDS.sleep(2000)`
- `TimeUnit.SECONDS.sleep(2)`  
Hay mas peridoso de tiempo: `NANOSECONDS`, `MICROSECONDS`, `MINUTES`, `HOURS`, `DAYS`.

## Join
El metodo `join` obliga al subproceso actual a esperar la finalizacion de otro subproceso en el que se llamo al metodo. En el siguiente ejemplo, la cadena **"Do something else"** no se imprimira hasta que finalice el subproceso.
~~~java
Thread thread = ...
thread.start(); // inicia el hilo

System.out.println("Do something useful");
thread.join(); // espera que el hilo termine
System.out.println("Do something else");

// la version sobrecargada toma un tiempo de espera en milisegundos
thread.join(2000L);
~~~
Consideremos otro ejemplo. La clase `Worker` esta desarrollada para resolver "una tarea dificil" simulada por `sleep()`.
~~~java
class Worker extends Thread {

    @Override
    public void run() {
        try {
            System.out.println("Starting a task");
            Thread.sleep(2000L); // resuelve una tarea dificil
            System.out.println("The task is finished");
        } catch (Exception ignored) {}
    }
}

// metodo main donde se espera la finalizacion de worker
public class JoiningExample {
    public static void main(String []args) throws InterruptedException {
        Thread worker = new Worker();
        worker.start(); // inicia el worker
       
        Thread.sleep(100L);
        System.out.println("Do something useful");
        
        worker.join(3000L);  // esperando por el worker
        System.out.println("The program stopped");
    }
}
~~~
El hilo principal espera `worker` y no puede imprimir el mensaje `The program stopped` hasta que el worker termina o se excede el tiempo de espera. Solo sabemos exactemente que `Starting a task` precede `The task is finished` y `Do something useful` precede `The program stopped`. Hay varias salidas posibles.  
Primera salida posible (la tarea se completa antes de que se exceda el tiempo maximo de espera).
~~~java
Starting a task
Do something useful
The task is finished
The program stopped
~~~
Segunda salida posible (la tarea se completa antes de que se exceda el tiempo de espera).
~~~java
Do something useful
Starting a task
The task is finished
The program stopped
~~~
Tercera salida posible:
~~~java
Do something useful
Starting a task
The program stopped
The task is finished
~~~
Cuarta salida posible.
~~~java
Starting a task
Do something useful
The program stopped
The task is finished
~~~
---

## Ejercicios
1. Tienes una clase que extiende `Thread`, tienes dos aplicaciones que usan esta clase. Seleccion las declaraciones correctas.
~~~java
class Worker extends Thread {
    private final String str;

    public Worker(String str) {
        this.str = str;
    }

    @Override
    public void run() {
        System.out.println(str);
    }
}

// primera aplicacion
public class Application1 {
    public static void main(String args[]) throws InterruptedException {
        Thread t1 = new Worker("Hello from t1");
        Thread t2 = new Worker("Hello from t2");
        Thread t3 = new Worker("Hello from t3");
        
        t1.start();
        t2.start();
        t3.start();
        
        t1.join();
        t2.join();
        t3.join();
    }
}

// seguna aplicacion
public class Application2 {
    public static void main(String args[]) throws InterruptedException {
        Thread t1 = new Worker("Hello from t1");
        Thread t2 = new Worker("Hello from t2");
        Thread t3 = new Worker("Hello from t3");
        
        t1.start();
        t1.join();
        
        t2.start();
        t2.join();
        
        t3.start();
        t3.join();
    }
}

// La segunda aplicacion siempre genera el texto en el mismo orden
// En ambas aplicaciones, el hilo principal espera t1, t2 y t3
// En la primera aplicacion, los subprocesos t1, t2 y t3 pueden funcionar en paralelo
~~~
2. Cual es el resultado de invocar `Thread.sleep(10)`.
- El hilo actual se detendra por 10 milisegundos.
3. Ha decidido cocinar una pizza vegana y ahora necesita realizar los pasos en orden correcto, corrige el error.
~~~java
class Pizza {

    //Fix this method
    public static void cookVeganPizza() throws InterruptedException {
        Base base = new Base();
        Tomatoes tomatoes = new Tomatoes();
        Tofu tofu = new Tofu();
        Bake bake = new Bake();
        java.util.List<Thread> stepOfCook = new java.util.ArrayList<>();
        stepOfCook.add(base);
        stepOfCook.add(tomatoes);
        stepOfCook.add(tofu);
        stepOfCook.add(bake);
        for (Thread step : stepOfCook) {
            step.start();
            step.join();
        }
    }

    static class Base extends Thread {
        @Override
        public void run() {
            System.out.println("cook base");
        }
    }

    static class Tomatoes extends Thread {
        @Override
        public void run() {
            for (int i = 3; i >= 1; i--) {
                System.out.println("slice tomatoes " + i);
            }
        }
    }

    static class Tofu extends Thread {
        @Override
        public void run() {
            System.out.println("fry tofu");
        }
    }

    static class Bake extends Thread {
        @Override
        public void run() {
            for (int i = 4; i >= 0; i--) {
                if (i == 0) {
                    System.out.println("Your pizza is ready!");
                    break;
                }
                System.out.println("to bake..." + i + " min");

            }
        }
    }

    public static void main(String[] args) throws InterruptedException {
        cookVeganPizza();
    }
}

/*
- Sample Input:
cook pizza
- Sample output:
cook base
slice tomatoes 3
slice tomatoes 2
slice tomatoes 1
fry tofu
to bake...4 min
to bake...3 min
to bake...2 min
to bake...1 min
Your pizza is ready!
*/
~~~
4. Imagina que tienes un objeto `thread` que es una instancia de una clase que extiende `Thread`. Cual es el resultado de `thread.join()`.
- El subproceso actual espera la finalizacion del **subproceso**.
5. Aqui hay una clase que extiende `Thread` y anula `run()`, seleccione los ordenes posibles de impresion.
~~~java
class MyThread extends Thread {

    @Override
    public void run() {
        System.out.println("Hello from " + getName());
    }
}

MyThread t1 = new MyThread();
t1.setName("My-thread-1");
t1.start();

t1.join();

MyThread t2 = new MyThread();
t2.setName("My-thread-2");
t2.start();
        
System.out.println("output-1");
        
t2.join();
        
System.out.println("output-2");

/*
- Output case:
Hello from My-thread-1
output-1
Hello from My-thread-2
output-2

- Output case:
Hello from My-thread-1
Hello from My-thread-2
output-1
output-2
~~~
6. Codigo que lee una cadena y calcula la cantidad de caracteres distintos en la cadena. Arregla el codigo.
~~~java
public class Main {

    private static final long mainThreadId = Thread.currentThread().getId();

    public static void main(String[] args) throws InterruptedException {

        Scanner scanner = new Scanner(System.in);
        String str = scanner.nextLine();
        SlowStringProcessor processor = new SlowStringProcessor(str);
        processor.start();
        processor.join();
        System.out.println(processor.getNumberOfUniqueCharacters());
    }

    static class SlowStringProcessor extends Thread {

        private final String s;
        private volatile long numberOfUniqueCharacters = 0;

        public SlowStringProcessor(String s) {
            this.s = s;
        }

        @Override
        public void run() {

            final long currentId = Thread.currentThread().getId();

            if (currentId == mainThreadId) {
                throw new RuntimeException("You must start a new thread!");
            }

            try {
                Thread.sleep(2000);
            } catch (Exception e) {
                throw new RuntimeException("Do not interrupt the processor", e);
            }

            this.numberOfUniqueCharacters = Arrays.stream(s.split("")).distinct().count();
        }

        public long getNumberOfUniqueCharacters() {
            return numberOfUniqueCharacters;
        }
    }
}
~~~
7. Imagina que tienes una intancia de `Thread` llamada `thread`, cual metodo debe ser llamado para forzar al hilo actual esperar 5 segundos.
- `thread.join(5000);`
8. Arregla el metodos que suma los numeros pasados como un rango.
~~~java
public class Main {

    private static long mainThreadId = Thread.currentThread().getId();

    public static void main(String[] args) throws InterruptedException {
        Scanner scanner = new Scanner(System.in);

        int from1Incl = scanner.nextInt(); // left limit of the first range
        int to1Incl = scanner.nextInt();   // right limit of the first range

        int from2Incl = scanner.nextInt(); // left limit of the second range
        int to2Incl = scanner.nextInt();   // right limit of the second range

        RangeSummator summator1 = new RangeSummator(from1Incl, to1Incl); // first summator
        RangeSummator summator2 = new RangeSummator(from2Incl, to2Incl); // second summator

        summator1.start();
        summator1.join();
        
        summator2.start();
        summator2.join();

        long partialSum1 = summator1.getResult();
        long partialSum2 = summator2.getResult();

        long sum = partialSum1 + partialSum2;

        System.out.println(sum);
    }

    static class RangeSummator extends Thread {

        int fromIncl;
        int toIncl;

        private volatile long result = 0;

        public RangeSummator(int fromIncl, int toIncl) {
            this.fromIncl = fromIncl;
            this.toIncl = toIncl;
        }

        @Override
        public void run() {
            final long currentId = Thread.currentThread().getId();

            if (currentId == mainThreadId) {
                throw new RuntimeException("You must start a new thread!");
            }

            long sum = 0;
            for (int i = fromIncl; i <= toIncl; i++) {
                sum += i;
            }

            result = sum;
        }

        public long getResult() {
            return result;
        }
    }
}
~~~
---

# Excepciones en Hilos
Si uno de los subprocesos de su programa arroja una excepcion que no es detectada por ningun metodo dentro de la pila de invocacion, el subproceso se terminara. Si se produce una excepcion de este tipo en un programa de un solo subproceso, todo el programa se detendra, porque JVM finaliza el programa en ejecucion tan pronto como no queden mas subprocesos **no-daemon**.
~~~java
public class SingleThreadProgram {
    public static void main(String[] args) {
        System.out.println(2 / 0);
    }
}

/*
- Este programa produce
Exception in thread "main" java.lang.ArithmeticException: / by zero
  at org.example.multithreading.exceptions.SingleThreadProgram.main(SingleThreadProgram.java:6)

Process finished with exit code 1
*/
~~~
El codigo `1` significa que el proceso se termino con un error. Si ocurre un error dentro del nuevo hilo que hemos cerrado, no se detendra todo el proceso.
~~~java
public class ExceptionInThreadExample {
    public static void main(String[] args) throws InterruptedException {
        Thread thread = new CustomThread();
        thread.start();
        thread.join(); // espera que el hilo con excepcion termine
        System.out.printn("I am printed"); // esta linea se imprime
    }
}

class CustomThread extends Thread {
    @Override
    public void run() {
        System.out.println(2 / 0);
    }
}
~~~
A pesar de la excepcion no detectada, el programa se completara con exito.
~~~java
Exception in thread "Thread-0" java.lang.ArithmeticException: / by zero  at org.example.multithreading.exceptions.CustomThread.run(ExceptionInThreadExample.java:15)
I am printed!

Process finished with exit code 0
~~~
El codigo `0` significa que le proceso finalizo con exito.  
Las excepciones en diferentes subprocesos se manejan de forma independiente. Si bien el proceso tiene un subproceso activo no es **daemon**, no se detendra en caso de que ocurra una excepcion no detectada. Aun asi, la buena practica es manejar excepciones en hilos.
---

## Ejercicios
1. Supongamos que se produce una excepcion no detectada en el hilo **main**. Que pasara con los otros hilos y el proceso entero?.
- Solo el hilo **main** se detendra, pero eventualmente el proceso finalizara con el codigo `1` (error).
2. Terminar cuales hilos lleva a la terminacion del programa entero?
- Todos los hilos no-daemon
3. Que pasara luego de la ejecucion de este programa.
~~~java
public class Main {
    public static void main(String[] args) throws Exception {
        Thread thread = new Thread(() -> {
            String str = null;
            System.out.println("Length is " + str.length());
        }, "secondary");
        thread.start();

        while (true) {
            // do nothing
        }
    }
}

// El hilo secundario se terminara con una excepcion, el hilo principal continuara su progreso
~~~
4. Suponga que tiene un objeto `Thread` dentro del metodo main. Que pasara si un excepcion ocurre dentro de `thread`?
~~~java
CustomThread thread = new CustomThread();
thread.start();
thread.join();

// un monton de declaraciones

// - Solo thread se terminara
~~~
5. Que pasara despues de lanzar este codigo.
~~~java
public class DivisionByZero {
    public static void main(String[] args) {
        int a = 42;
        int b = 0;
        System.out.println(a / b);
    }
}

// java.lang.ArithmeticException es arrojado, el programa se completa con codigo 1
~~~
