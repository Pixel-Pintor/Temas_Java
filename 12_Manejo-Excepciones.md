# Manejo de Excepciones
Una excepcion interrumple la ejecucion normal de un programa. Afortunadamente, es posible escribir algun codigo que maneje la excepcion sin detener todo el programa.  
Tecnicamente, una excepcion se puede manejar en el metodo donde ocurre o en el metodo de llamada. El mejor enfoque para manejar una excepcion es hacerlo en eun metodo que tenga suficiente informacion para tomar la decisiones correcta basada en esa excepcion.  
El bloque `try` se usa para envolver el codigo que puede generar una excepcion. Este bloque puede incluir todas las lineas de codigo, incluidas las llamadas a metodos.  
El bloque `catch` es un controlador para el tipo de excepcion especificado y todas sus subclases. Este bloque se ejecuta cuando se produce una excepcion del tipo correspondiente en el bloque `try`. El siguiente ejemple demuestra el flujo de ejecucion con `try` y `catch`.
~~~java
System.out.println("before the try-catch block"); // se imprime

try {
    System.out.println("inside the try block before an exception"); // se imprime

    System.out.println(2 / 0); // arroja ArithmeticException

    System.out.println("inside the try block after the exception"); // no se imprime
} catch (Exception e) {
    System.out.println("Division by zero!"); // se imprime
}

System.out.println("after the try-catch block"); // se imprime
~~~
Reemplazando `Exception` con `ArithmeticException` o `RuntimeException` en la declaracion `catch` no cambia el flujo de ejecucion del programa. Pero reemplazandolo con `NumberFormatException` hara que el controlador no sea adecuado para la excepcion y el programa fallara.  
El bloque `try-catch` puede manejar excepciones verificadas y no verificadas. Pero las **excepciones verificadas** deben envolverse con un bloque `try-catch`, mientras que las excepciones no verificadas no tiene que hacerlo.  
Cuando una excepcion es capturada por un bloque `catch`, es posibles obtener alguna informacion sobre el:
~~~java
try {
    double d = 2 / 0;
} catch(Exception e) {
    System.out.println(e.getMessage());
}

// este codigo imprime: An exception occured: / by zero
~~~
Java admite el uso de varios controladores dentro del mismo bloque `try`:
~~~java
try {
    // code that throws exceptions
} catch (IOException e) {
    // handling the IOException and its subclasses    
} catch (Exception e) {
    // handling the Exception and its subclasses
}
~~~
Cuando ocurre una excepcion, el sistema de tiempo de ejecucion determina el primer bloque adecuado segun el tipo de excepcion. El emparejamiento va de arriba hacia abajo. Los manipuladores mas especializados (como `IOException`) deben escribirse antes de los mas generales (como `Exception`). Desde Java 7, puede usar capturar varias excepciones juntas.
~~~java
try {
    // code that may throw exceptions
} catch (SQLException | IOException e) {
    // handling SQLException, IOException and their subclasses
    System.out.println(e.getMessage());
} catch (Exception e) {
    // handling any other exceptions
    System.out.println("Something goes wrong");
}
~~~
Hay otro bloque posible llamado `finally`. Todas las declaraciones presentes en este bloque siempre se ejecutaran independientemente de si se produce una excepcion o no.
~~~java
try {
    System.out.println("inside the try block");
    Integer.parseInt("101abc"); // throws NumberFormatException
} catch (Exception e) {
    System.out.println("inside the catch block");
} finally {
    System.out.println("inside the finally block");
}

System.out.println("after the try-catch-finally block");
~~~
Tambien es posible escribir `try` y `finally` sin un bloque `catch`.
~~~java
try {
    // codigo que podria generar una excepcion
} finally {
    // codigo que siempre se ejecuta
}
~~~
---
## Ejercicios
1. Imprime el numero introducido multiplicandolo por 10, si no se escribe un entero maneja la excepcion que genera, termina el ciclo si el usuario introduce un 0.
~~~java
class Main {

    private static final int MULTIPLIER = 10;

    public static void main(String[] args) {
        // put your code here
        Scanner scanner = new Scanner(System.in);
        boolean run = true;
        while (run) {
            String input = scanner.next();
            try {
                int num = Integer.parseInt(input);
                if (num == 0) {
                    run = false;
                } else {
                    System.out.println(num * MULTIPLIER);
                }
            } catch (NumberFormatException e) {
                System.out.println("Invalid user input: " + input);
            }
        }
    }
}
~~~
2. Su tarea es implementar el segundo metodo que debe llamar al primer metodo y detectar dos tipos de excepciones.
- `ArrayIndexOutOfBoundsException`
- `NumberFormatException`
~~~java
public class Main {

    private static String array = null;

    public static void methodCatchingSomeExceptions() {
        try {
            methodThrowingExceptions();
        } catch (ArrayIndexOutOfBoundsException | NumberFormatException e) {
            System.out.println(e.getClass().getSimpleName());
        }
    }

    public static void methodThrowingExceptions() {
        if (array == null) {
            return;
        }
        int[] integers = Arrays.stream(array.split(" "))
                .mapToInt(Integer::parseInt)
                .toArray();
        Object someValue = integers[integers[0]];
        if (integers[0] + integers[1] == integers[2]) {
            integers = null;
        }
        int sum = 0;
        for (int i : integers) {
            sum += i;
        }
        int meanValue = integers.length / sum;
        if (integers[2] == integers[3]) {
            String string = (String) someValue;
            System.out.print("Random value is " + string);
        }
        System.out.print("Mean is " + meanValue);
    }


    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        array = scanner.nextLine();
        try {
            methodCatchingSomeExceptions();
        } catch (Exception e) {
            System.out.println("Caught: " + e.getClass().getName());
        }
    }
}
~~~
3. Asume que en el bloque `try` se genera una `NumberFormatException`, seleccione los bloques que se ejecutaran.
~~~java
try {
    // it throws an exception
} catch (RuntimeException e) { // se ejecuta
    // ...
} catch (IOException e) { // no
    // ...
} catch (Exception e) { // no
    // ...
} finally { // se ejecuta
    // ...
}

/*
Se ejecuta RuntimeException por que NumberFormatException es su subclase, y el bloque finally siempre se ejecuta.
*/
~~~
4. Poner en orden los bloques para manejar una excepcion.
~~~java
try {
    // ...
} catch(IOException | SQLException e) {
    // ...
} catch(Exception e) {
    // ...
} finally {
    // ...
}

/*
Las excepciones mas especificas se ponen antes de la excepcion mas general que en este caos es Exception.
*/
~~~
5. Que imprimira el codigo siguiente.
~~~java
try {
    int a = 1, b = 0;
    int c = 5 / b;
    System.out.print("A");
}
catch (ArithmeticException e) {
    System.out.print("B");          
}
finally {
    System.out.print("C");
}

/* 
se genera una excepcion por division por 0, el bloque finally se imprime siempre: BC
*/
~~~
6. Cuales lineas se van a imprimir en el siguiente codigo.
~~~java
try {
    String s = null;
    char ch = s.charAt(10);
    System.out.println(ch);
} catch (RuntimeException e) { // imprime esta linea
    System.out.println("A runtime exception occurred");
} catch (Exception e) {
    System.out.println("An exception occurred");
} finally {
    System.out.println("Finally!"); // imprime esta linea
}
~~~
7. Maneja las excepciones de este metodo que convierte un `String` a un `double`.
~~~java
class Converter {

    public static double convertStringToDouble(String input) {
        try {
            return Double.parseDouble(input);
        } catch (RuntimeException e) {
            return 0;
        }
    }
}
~~~
