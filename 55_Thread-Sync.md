# Sincronizacion de subprocesos
Trabajar simultaneamente con datos compartidos de varios subprocesos puede provocar un comportamiento inesperado o erroneo. Afortunadamente, Java proporciona un mecanismo para controlar el acceso de varios subprocesos a un recurso compartido de cualquier tipo. El mecanismo se conoce como **sincronizacion de subprocesos**.  
1. Una **seccion critica** es una region del codigo que accede a recursos compartidos y no deberia ser ejecutada por mas de un hilo a la vez. Un recurso compartido puede ser una variable, un archivo, puerto input/output, base de datos o algo mas.
~~~java
public static long counter = 0;
~~~
Dos hilos incrementan el campo 10.000.000 concurrentemente. El valor finla deberia ser 20.000.000, Pero como discutimos, el resultado podria estar equivocado, por ejemplo, 10.999.843.  
Esto pasa por que algunas veces a hilo no ve los cambios de los datos compartidos por otro hilo, y a veces un hilo puede ver un valor intermedio de las operaciones no atomicas. Esos son problemas de visibilidad y atomicidad que manejamos mientras trabajamos con datos compartidos. Esta es la razon por la que aumentar el valor mediante multiples subprocesos es una **seccion critica**. Por supuesto, este ejemplo es muy simple, una seccion critica puede ser mucho mas complicada.  
2. El **monitor** es un mecanismo especial para cotrolar el acceso simultaneo a un objeto. En Java, cada objeto y clase tiene un monitor implicito asociado. Un hilo puede adquirir un monitor, luego otros hilos no pueden adquirir este monitor al mismo tiempo. Esperaran hasta que el propietario (el hilo que adquirio el monitor) lo libere.  
Por lo tanto, un hilo puede ser bloqueado por el **monitor** de un objeto y esperar su liberacion. Este mecanismo permite a los programadores proteger las **secciones criticas** del acceso de varios hilos al mismo tiempo.  

## La palabra synchronized
La forma "clasica" y mas sencilla de proteger el codigo para que no accedan varios subprocesos al mismo tiempo es usar la palabra clave **synchronized**. Se utiliza en dos formas diferentes:
- Metodo sincronizado (un metodo estatico o de instancia)
- Bloques o sentencias sincronizadas (dentro de un metodo estatico o de instancia)  
Un metodo o bloque sincronizado necesita un objeto para bloquear hilos. El monitor asociado con este objeto controla el accceso simultaneo a la seccion critica especificada. Solo un hilo puede ejecutar el codigo en un bloque o metodo sincronizado al mismo tiempo. Otro hilos se bloquean hasta que el hilo dentro del bloque o metodo sincronizado sale de el.  

## Metodos sincronizados estaticos
Cuando sincronizamos m??todos est??ticos usando la palabra **synchronized**, el monitor es la clase misma. Solo un hilo puede ejecutar el cuerpo de un m??todo est??tico sincronizado al mismo tiempo. Esto se puede resumir como *"un hilo por clase"*.  
Aqui hay un ejemplo de una clase con un solo metodo estatico sincronizado llamado `doSomething`.
~~~java
class SomeClass {

    public static synchronized void doSomething() {
        
        String threadName = Thread.currentThread().getName();
        System.out.println(String.format("%s entered the method", threadName));
        System.out.println(String.format("%s leaves the method", threadName));
    }
}
~~~
El m??todo `doSomething` se declara como sincronizado. Solo se puede invocar desde un hilo al mismo tiempo. El m??todo se sincroniza en la clase a la que pertenece el m??todo est??tico (el **monitor** es `SomeClass`). Si llamamos al metodo desde dos subprocesos al mismo tiempo. El resultado siempre sera similar a:
~~~java
Thread-0 entered the method
Thread-0 leaves the method
Thread-1 entered the method
Thread-1 leaves the method
~~~
Es imposible que m??s de un hilo ejecute c??digo dentro del m??todo al mismo tiempo.  

## Metodos sincronizados de instancia
Los m??todos de instancia se sincronizan en la instancia (objeto). El monitor es el objeto actual (`this`) que posee el m??todo. Si tenemos dos instancias de una clase, cada instancia tiene un monitor para sincronizar.  
Solo un hilo puede ejecutar c??digo en un m??todo de instancia sincronizada de una instancia en particular. Pero diferentes hilos pueden ejecutar m??todos de diferentes objetos al mismo tiempo. Esto se puede resumir como *"un hilo por instancia"*.  
Aqu?? hay un ejemplo de una clase con un ??nico m??todo de instancia sincronizado llamado `doSomething`. La clase tambi??n tiene un constructor para distinguir instancias.
~~~java
class SomeClass {

    private String name;

    public SomeClass(String name) {
        this.name = name;
    }

    public synchronized void doSomething() {

        String threadName = Thread.currentThread().getName();
        System.out.println(String.format("%s entered the method of %s", threadName, name));
        System.out.println(String.format("%s leaves the method of %s", threadName, name));
    }
}
~~~
Vamos a crear dos instancias de la clase y tres hilos invocando `doSomething`. El primer y el segundo hilo toman la misma instancia de la clase, y el tercer hilo toma otro.
~~~java
SomeClass instance1 = new SomeClass("instance-1");
SomeClass instance2 = new SomeClass("instance-2");

MyThread first = new MyThread(instance1);
MyThread second = new MyThread(instance1);
MyThread third = new MyThread(instance2);

first.start();
second.start();
third.start();

// resultado
Thread-0 entered the method of instance-1
Thread-2 entered the method of instance-2
Thread-0 leaves the method of instance-1
Thread-1 entered the method of instance-1
Thread-2 leaves the method of instance-2
Thread-1 leaves the method of instance-1
~~~
Como puede ver, no hay hilos que ejecuten el c??digo en `doSomething` del `instance-1` al mismo tiempo. Intenta ejecutarlo muchas veces.  

## Bloque sincronizados (declaraciones)
A veces necesita sincronizar solo una parte de un m??todo. Esto es posible mediante el uso de bloques sincronizados (sentencias). Deben especificar un objeto para bloquear subprocesos.  
Aqu?? hay una clase con un m??todo est??tico y uno de instancia. Ambos m??todos no est??n sincronizados pero tienen partes sincronizadas en su interior.
~~~java
class SomeClass {

    public static void staticMethod() {

        // unsynchronized code
                
        synchronized (SomeClass.class) { // synchronization on the class
            // synchronized code
        }
    }

    public void instanceMethod() {

        // unsynchronized code

        synchronized (this) { // synchronization on this instance
            // synchronized code
        }
    }
}
~~~
El bloque interior `staticMethod` est?? sincronizado en la clase, lo que significa que solo un hilo puede ejecutar c??digo en este bloque.  
El bloque interior `instanceMethod` est?? sincronizado en instancia usando `this`, lo que significa que solo un hilo puede ejecutar el bloque de la instancia. Pero alg??n otro hilo puede ejecutar el bloque de diferentes instancias al mismo tiempo.  
Los bloques sincronizados pueden parecerse a los m??todos sincronizados, pero permiten a los programadores sincronizar solo las partes necesarias de los m??todos.  

## Sincronizacion y visibilidad de cambios
La *especificaci??n del lenguaje Java* garantiza que los cambios realizados por un hilo sean visibles para otros hilos si est??n sincronizados en el mismo **monitor**. M??s precisamente, si un hilo ha cambiado los datos compartidos (por ejemplo, una variable) dentro de un bloque sincronizado o un m??todo y ha liberado el monitor, otros hilos pueden ver todos los cambios despu??s de adquirir el mismo **monitor**.  

## Ejemplo: un contador sincronizado
Aqu?? hay un ejemplo. Es un contador sincronizado con dos m??todos de instancia sincronizados: `increment` y `getValue`.
~~~java
class SynchronizedCounter {

    private int count = 0;

    public synchronized void increment() {
        count++;
    }

    public synchronized int getValue() {
        return count;
    }
}
~~~
Cuando m??ltiples hilos invocan `increment` en la misma instancia, no surge ning??n problema porque la sincronizada protege el campo compartido. Solo un hilo puede cambiar el campo. Otros hilos esperar??n hasta que el hilo libere el **monitor**. Todos los cambios de la variable `count` son visibles.  
El m??todo `getValue` no modifica el campo. Solo lee el valor actual. El m??todo est?? sincronizado para que el hilo de lectura siempre lea el valor real; de lo contrario, no hay garant??a de que el hilo de lectura vea el `count` como es despu??s de que se cambia.  
Aqu?? hay una clase llamada `Worker` que extiende `Thread`. La clase toma una instancia de `SynchronizedCounter` y llama al m??todo `increment` 10 000 000 veces.
~~~java
class Worker extends Thread {

    private final SynchronizedCounter counter;

    public Worker(SynchronizedCounter counter) {
        this.counter = counter;
    }

    @Override
    public void run() {
        for (int i = 0; i < 10_000_000; i++) {
            counter.increment();
        }
    }
}
~~~
El siguiente c??digo crea una instancia de `SynchronizedCounter`, inicia subprocesos e imprime el resultado. 
~~~java
SynchronizedCounter counter = new SynchronizedCounter();

Worker worker1 = new Worker(counter);
Worker worker2 = new Worker(counter);

worker1.start();
worker2.start();

worker1.join();
worker2.join();

System.out.println(counter.getValue()); // the result is 20_000_000
~~~
A veces, sin embargo, no es necesario sincronizar m??todos que solo leen datos compartidos (incluidos los captadores): 
- Si tenemos una garant??a de que el hilo de lectura regresa con ??xito de `join` en todos los hilos de escritura cuando lee un campo. Eso es cierto sobre el c??digo anterior y podemos eliminar la palabra clave `synchronized` de la declaraci??n de `getValue`.
- Si se declara un campo compartido con la palabra clave `volatile`. En ese caso, siempre veremos el valor real de este campo.  
Tenga mucho cuidado cuando decida no sincronizar los m??todos de lectura.  

## Un monitor y multiples metodos y bloques sincronizados
**Importante:** un objeto o una clase que tiene solo un monitor y solo un hilo puede ejecutar c??digo sincronizado en el mismo monitor.  
Significa que si una clase tiene varios m??todos de instancia sincronizados y un hilo invoca uno de ellos, otros hilos no pueden ejecutar ninguno de estos m??todos en la misma instancia hasta que el primer hilo libere el monitor de la instancia.  
Aqu?? hay un ejemplo: una clase con tres m??todos de instancia. Dos m??todos est??n sincronizados y el tercero tiene un bloque sincronizado interno. Ambos m??todos y el bloque est??n sincronizados en el monitor de instancia `this`.
~~~java
class SomeClass {

    public synchronized void method1() {
        // do something useful
    }

    public synchronized void method2() {
        // do something useful
    }
    
    public void method3() {
        synchronized (this) {
            // do something useful
        }
    }
}
~~~
Si un hilo invoca `method1` y ejecuta sentencias dentro del m??todo, ning??n otro subproceso puede ejecutar sentencias dentro method2o en el bloque sincronizado en `method3` porque el monitor `this` ya est?? adquirido. Los subprocesos esperar??n la liberaci??n del monitor. El mismo comportamiento es correcto cuando se utiliza un monitor de clase.  

## Sincronizacion reentrante
Un hilo no puede adquirir un bloqueo de propiedad de otro hilo. Pero un hilo puede adquirir un bloqueo que ya posee. Este comportamiento se llama **sincronizaci??n reentrante**.  
Echa un vistazo al siguiente ejemplo.
~~~java
class SomeClass {

    public static synchronized void method1() {
        method2(); // legal invocation because a thread has acquired monitor of SomeClass
    }

    public static synchronized void method2() {
        // do something useful
    }
}
~~~
El c??digo de arriba es correcto. Cuando un hilo est?? dentro `method1` puede invocar `method2` porque ambos m??todos est??n sincronizados en el mismo objeto (`SomeClass`).  

## Sincronizacion de grano fino
A veces, una clase tiene varios campos que nunca se usan juntos. Es posible proteger estos campos usando el mismo monitor, pero en este caso, solo un hilo podr?? acceder a uno de estos campos, a pesar de su independencia. Para mejorar la tasa de concurrencia, es posible usar un modismo con objetos adicionales como monitores.  
He aqu?? un ejemplo: una clase con dos m??todos. La clase almacena el n??mero de llamadas a cada m??todo en un campo especial. 
~~~java
class SomeClass {

    private int numberOfCallingMethod1 = 0;
    private int numberOfCallingMethod2 = 0;

    final Object lock1 = new Object(); // an object for locking
    final Object lock2 = new Object(); // another object for locking

    public void method1() {
        System.out.println("method1...");

        synchronized (lock1) {
            numberOfCallingMethod1++;
        }
    }

    public void method2() {
        System.out.println("method2...");
        
        synchronized (lock2) {
            numberOfCallingMethod2++;
        }
    }
}
~~~
Como puede ver, la clase tiene dos campos adicionales que son los candados para separar monitores para cada secci??n cr??tica.  
Si tenemos una instancia de la clase, un hilo puede funcionar dentro del bloque sincronizado del primer metodo y, al mismo tiempo, otro hilo puede funcionar dentro del bloque sincronizado del segundo metodo.  

## Sincronizacion y ejecucion de programas
Recuerde, el codigo protegido por el mecanismo de sincronizacion solo puede ser ejecutador por su hilo al mismo tiempo. Reduce el paralelismo y la capacidad de respuesta del programa.  
No sincronices todo tu codigo. Intente usar la sincronizacion solo cuando sea realmente necesario. Determine peque??as partes del codigo que se van a sincronizar. A veces es mejor usar un bloque de sincronizacion en lugar de sincronizar un metodo completo (si el metodo es complejo).  
---

## Ejercicios
1. Aqui hay una clase llamada `SomeClass`, con dos instancias. Seleccione dos declaraciones correctas.
~~~java
public class SomeClass {

    public synchronized void method1() {

        // do something useful

        method2();
    }

    public synchronized void method2() {

        // do something useful
    }
}

SomeClass instance1 = new SomeClass();
SomeClass instance2 = new SomeClass();
~~~