# Throwing Exceptions
Ahora sabe que son las excepciones y en que situaciones ocurren. Ahora es el momento de explorarlos mas a fondo para comprender cuando y como debe lanzar excepciones usted mismo.

## La palabra clave Throwable
Cualquier objeto de la clase `Throwable` y todas sus subclases pueden lanzar usando la palabra `throw`. La forma general del enunciado consiste en la palabra `throw` y un objeto para ser lanzado. En el siguiente ejemplo, creamos y arrojamos un objeto de la clase `RuntimeException` que extiende `Throwable`.
~~~java
public class Main {
    public static void main(String[] args) {
        RuntimeException exception = new RuntimeException("Something's bad.");
        throw exception;
    }
}
~~~
Consideremos el codigo anterior. Primero creamos un objeto con el mensaje especificado como argumento del constructor. Entonces, lanzamos esta excepcion usando la palabra `throw`. Simplemente crear un objeto no es suficiente para lanzar una excepcion.  
El programa se detiene e imprime el error con el mensaje que proporcionamos.
~~~java
Exception in thread "main" java.lang.RuntimeException: Something's bad.
    at Main.main(Main.java:3)
~~~
La practica comun es crear y lanzar una excepcion en una sola linea.
- lanzando una instancia de `Throwable`
~~~java
throw new Throwable("Something's bad.");
~~~
- lanzando una instancia de `Exception`
~~~java
throw new Exception("An exception occurs");
~~~
- Lanzando una instancia de `NullPointerException`
~~~java
throw new NullPointerException("The field is null");
~~~
Solo es posible lanzar un objeto de la clase `Throwable` o una clase que la extiende. Por ejemplo, la linea `throw new Long(10L)` no compila.  

## Lanzar excepciones comprobadas
Por ejemplo, echemos un vistazo al siguiente metodo que lee texto de un archivo. En caso de que no se encuentre el archivo, el metodo lanza un `IOException`.
~~~java
public static String readTextFromFile(String path) throw IOException {
    // encuentra un archivo especificado en la ruta
    if (!found) {
        throw new IOException("The file " + path + "is not found");
    }
    // lee y retorna el texto del archivo
}
~~~
Aqui solo podemos ver una parte del metodo. La palabra `throws` se requiere despues de los parametros, ya que `IOException` es una excepcion comprobada.  
Si un metodo arroja una excepcion verificada, el tipo de excepcion debe especificarse en la declaracion del metodo despues de `throws`. De lo contrario, el codigo no se compilara.  
Si un metodo arroja dos o mas excepciones verificadas, deben estar separadas por una coma en la declaracion.
~~~java
public static void method() throws ExceptionType1, ExceptionType2, ExceptionType3
~~~
Si se declara que un metodo arroja una excepcion (es decir, `BaseExceptionType`), tambien puede lanzar cualquier subclase de la excepcion especificada (es decir, `SubClassExceptionType`):
~~~java
public static void method() throws BaseExceptionType
~~~

## Lanzar excepciones no verificadas
Veamos como se lanzan las excepciones no verificadas en un ejemplo mas real. La clase `Account` contiene el metodo `deposit`, que suma la cantidad especificada al saldo actual. Si la cantidad no es positiva o excede el limite, el metodo arroja un `IllegalArgumentoException`.
~~~java
class Account {
    private long balance = 0;

    public void deposit(long amount) {
        if (amount <= 0) {
            throw new IllegalArgumentException("Incorrect sum " + amount);
        }

        if (amount >= 100_000_000L) {
            throw new IllegalArgumentException("Too large amount");
        }

        balance += amount;
    }

    public long getBalance() {
        return balance;
    }
}
~~~
El metodo `deposit` no se declara como lanzando un `IllegalArgumentException`. Lo mismo es cierto para todas las demas excepciones no verificadas.  
Si un metodo arroja un excepcion no verificada, la palabra clave `throws` no se requiere en la declaracion del metodo, pero aun debe usar `throw`.  

## Cuando lanzar una excepcion?
Como puede ver, tecnicamente, lanzar una excepcion es una tarea bastante sencilla. Pero la pregunta es ¿Cuando necesitas hacer esto? La respuestas es que no siempre es obvio.  
La practica comun es lanzar una excepcion cuando y solo cuando se rompen las condiciones previas del metodo, es decir, cuando no se puede realizar en las condiciones actuales.  
Hay diferentes casos en los que le gustaria lanzar una excepcion. Imagine un metodo que analice la cadena de entrada en el formato dd-MM-yyyy para obtener un mes. Aqui, si la cadena no es valida, el metodo lanza un `InvalidArgumentException`. Otro ejemplo es leer un archivo inexistente que conducira a un `FileNotFoundException`.  
Despues de un tiempo de practica, identificar situaciones en las que necesita una excepcion se convertira en una tarea mas facil para usted. Se recomienda lanzar las excepciones que sean mas relevantes (especificas) para el problema: es mejor lanzar un objeto `InvalidArgumentException` que la excepcion base `Exception`.  
Otra pregunta es como elegir entre excepciones marcadas y no marcadas. Hay una breve guia. Si se puede esperar razonablemente que un cliente se recupere de una excepcion conviertalo en una excepcion marcada. Si un cliente no puede hacer nada para recuperarse, conviertalo en una excepcion sin marcar.  

## Conclusion
En este tema, ha aprendido como y cuando generar excepciones. Puedes lanzar cualquier objeto de la clase `Throwable` y todas sus subclases utilizando `throw` y un objeto para ser lanzado. Por lo general, se lanza un excepcion cuando y solo cuando se rompen las condiciones previas del metodo, y no se puede realizar en las condiciones actuales.  
---

## Ejercicios
1. Escribir un metodo que arroje una excepcion si el numero pasado como parametro no corresponde a un dia de la semana.
~~~java
public class Main {

    public static String getDayOfWeekName(int number) {
        List<Integer> numbers = List.of(1, 2, 3, 4, 5, 6, 7);
        List<String> days = List.of("Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun");
        if (!numbers.contains(number)) {
            throw new IllegalArgumentException();
        } else {
            return days.get(number - 1);
        }
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int dayNumber = scanner.nextInt();
        try {
            System.out.println(getDayOfWeekName(dayNumber));
        } catch (Exception e) {
            System.out.println(e.getClass().getName());
        }
    }
}
~~~
2. Modifique el metodo para que lance una `IOException`.
~~~java
public class Main {

    public static void method() throws IOException {
        throw new IOException();
    }

    public static void main(String[] args) {
        try {
            method();
        } catch (Exception e) {
            System.out.println(e.getClass());
        }
    }
}
~~~
3. Modifique el metodo para que arroje una excepcion no verificada.
~~~java
public class Main {

    public static void method() {
        throw new IllegalArgumentException();
    }

    /* Do not change code below */
    public static void main(String[] args) {
        try {
            method();
        } catch (RuntimeException e) {
            System.out.println("RuntimeException");
        } catch (Exception e) {
            System.out.println("Exception");
        }
    }
}
~~~
4. Seleccione todas las clases cuyos objetos pueden ser lanzados.
~~~java
java.lang.Exception
java.lang.Throwable
java.io.IOException
java.lang.RuntimeException
~~~
5. Implemente el metodo `sqrt` que calcula la raiz cuadrada de un numero dado. Si se pasa un numero negativo debe arrojar `java.lang.IllegalArgumentException` con el mensaje `"Expected non-negative number, got '"`.
~~~java
public class Main {

    public static double sqrt(double x) {
        if (x < 0.0) {
            throw new IllegalArgumentException("Expected non-negative number, got " + x);
        } else {
            return Math.sqrt(x);
        }
    }

    public static void main(String[] args) {
        String value = new Scanner(System.in).nextLine();
        try {
            double res = sqrt(Double.parseDouble(value));
            System.out.println(res);
        } catch (IllegalArgumentException e) {
            System.out.println(e.getMessage());
        }
    }
}
~~~
6. Seleccione la excepcion que no funcionara.
~~~java
throw new String("An exception occurs"); // NO FUNCIONA
throw new Exception("An exception occurs"); // FUNCIONA
throw new NullPointerException("NPE"); // FUNCION
throw new Throwable("An error occurs"); // FUNCION
~~~
7. Existe una declaracion en un metodo, seleccione la manera correcta de declarar el metodo que contiene esta declaracion.
~~~java
// declaracion dentro del metodo
throw new Exception();
// firma correcta del metodo
public static void method() throws Exception { ... }
~~~
8. Modifique el metodo para que arroje una excepcion marcada.
~~~java
public class Main {

    public static void method() throws Exception {
        throw new Exception();
    }

    /* Do not change code below */
    public static void main(String[] args) {
        try {
            method();
        } catch (RuntimeException e) {
            System.out.println("RuntimeException");
        } catch (Exception e) {
            System.out.println("Exception");
        }
    }
}
~~~
