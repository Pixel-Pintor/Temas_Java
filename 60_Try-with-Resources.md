# Try with resources
Hemos mencionado que los **flujos de entrada** debe cerrarse despues de que se hayan utilizado. Hablemos de lo que sucede cuando se trabaja con recursos externos: como se puede realizar el cierre y por que es importante.  
Cuando se crea un flujo de entrada, la JVM notifica al sistema operativo sobre su intencion de trabajar con un archivo. Si el proceso JVM tiene suficientes permisos y todo esta bien, el sistema operativo devuelve un **descriptor de archivo**, un indicador especial utilizado por un proceso para acceder al archivo. El problema es que el numero de descriptores de archivos es limitado. Esta es la tazon por la que es importante notificar al sistema operativo que el trabajo esta hecho y que el descriptor de archivo retenido se puede liberar para su posterior reutilizacion. En ejemplos anteriores, invocamos el metodo `close()` para este proposito. Una vez que se llama, la JVM libera todos los recursos del sistema asociados con la transmision.  

## Trampas
La liberacion de recursos funciona si la JVM llama al metodo `close()`, pero es posible que el metodo no se llame en absoluto. Mira el ejemplo:
~~~java
Reader reader = new FileReader("file.txt");
// codigo que podria arrojar una excepcion
reader.close();
~~~
Supongamos que algo sale mal antes de cerrar el flujo y se lanza una excepcion. Conduce a una situacion en la que nunca se llama al metodo de cierre y no se liberan los recursos del sistema. Es posible resolver el problema usando **try-catch-finally**:
~~~java
Reader reader = null;
try {
    reader = new FileReader("file.txt");
    // codigo que podria arrojar una excepcion
} finally {
    reader.close();
}
~~~
Las excepciones lanzadas no pueden afectar la invocacion del metodo `close()` ahora.  
Desafortunadamente, esta solucion todavia tiene algunos problemas. Eso es por que el metodo `close()` puede generar excepciones por si mismo. Supongamos que ahora hay dos excepciones: la primera se planteo dentro de la seccion `try`, y el segundo fue lanzado por la seccion `finally`. Conduce a la perdida de la primera excepcion. Veamos por que sucede esto.
~~~java
void readFile() throws IOException {
    Reader reader = null;
    try {
        reader = new FileReader("file.txt");
        throw new RuntimeException("Exception1");
    } finally {
        reader.close(); // throws new RuntimeException("Exception2")
    }
}
~~~
Primero el bloque `try` lanza una excepcion. Como sabemos, el bloque `finally` se invoca de todos modos. En nuestro ejemplo, ahora el metodo `close()` lanza una excepcion. Cuando occurren dos excepciones, la segunda excepcion se lanza fuera del metodo `Exception2`. Significa que nunca sabremos que el bloque `try` planteo una excepcion en lo absoluto.  
Tratemos de razonar y arreglar esto. Ok, no queremos perder la primera excepcion, entonces actualizamos un poco el codigo y manejamos `Exception2` justo despues de que fue lanzada.
~~~java
void readFile() throws IOException {
    Reader reader = null;
    try {
        reader = new FileReader("file.txt");
        throw new RuntimeException("Exception1");
    } finally {
        try {
            reader.close(); // throws new RuntimeException("Exception2")
        } catch (Exception e) {
            // maneja la Excepcion2
        }
    }
}
~~~
Ahora, la pieza de codigo arroja `Exception1`. Puede que sea correcto, pero seguimos sin guardar informacion de ambas excepciones, y en ocasiones no queremos perderlas. Asi que ahora, veamos como podemos manejar bien esta situacion.  

## Solucion
Una forma simple y confiable es **try with resources**.
~~~java
try (Reader reader = new FileReader("file.txt")) {
    // alguno codigo
} 
~~~
Esta construccion tiene dos partes encerradas por corchetes. Los corchetes contienen declaraciones que crean una instancia de flujo de entrada. Tambien es posible crear varios objetos. El siguiente codigo tambien esta bien.
~~~java
try (Reader reader1 = new FileReader("file1.txt");
     Reader reader2 = new FileReader("file2.txt")) {
    // some code
}
~~~
La segunda parte solo contiene algo de codigo para tratar con el objeto que se creo en la primera parte.  
Como ves, no hay llamadas explicitas al metodo `close()` en absoluto. Se invoca implicitamente para todos los objetos declarados en la primera parte. La construccion garantiza el cierre de todos los recursos de manera adecuada.  
Desde Java 9, puede inicializar un flujo de entrada fuera de la construccion y luego declararlo entre corchetes:
~~~java
Reader reader = new FileReader("file.txt");
try (reader) {
    // some code
}
~~~
La mejor practica es envolver cualquier codigo relacionado con los recursos del sistema mediante una construccion **try-with-resources**. Tambien puede usar probar con recursos como parte de **try-catch-finally** de esta manera.
~~~java
try (Reader reader = new FileReader("file.txt")) {
    // some code
} catch (IOException e) {
    ...
} finally {
    ...
}
~~~
Ahora volvamos a nuestro caso de dos excepciones. Si tanto el bloque `try` y el metodo `close()` lanzan excepciones:
~~~java
void readFile() throws IOException {
    try (Reader reader = new FileReader("file.txt")) {
        throw new RuntimeException("Exception1");
    }
}
~~~
El metodo lanza la exception resultante, que comprende informacion sobre ambas excepciones. Se parece a esto:
~~~java
Exception in thread "main" java.lang.RuntimeException: Exception1
    at ...
    Suppressed: java.lang.RuntimeException: Exception2
        at ...
~~~

## Recursos que se pueden cerrar
Hemos tratado con un flujo de entrada de archivos para demostrar como se usa **probar con recursos**. Sin embargo, no solo se deben liberar los recursos basados en archivos. El cierre es crucial para otras fuentes externas, como conexiones web o de base de datos. Las clases que los manejan tiene un metodo `close()`, por lo tanto, puede ser envuelto por la instruccion **try-with-resources**.  
Por ejemplo, consideremos `java.util.Scanner`. Antes usamos `Scanner` para leer datos de la entrada estandar, pero tambien puede leer datos de un archivo. `Scanner` tiene un metodo `close()` para liberar fuentes externas.  
Consideremos un ejemplo de un programa que lee dos enteros separadores por un espacio de una archivo y los imprime:
~~~java
try (Scanner scanner = new Scanner(new File("file.txt"))) {
    int first = scanner.nextInt();
    int second = scanner.nextInt();
    System.out.println("arguments: " + first + " " + second);
}
~~~
Supongamos que algo salio mal y el contenido del archivo es `123 not_number`, donde el segundo argumento es un `String`. Conduce a un `java.util.InputMismatchException` mientras analiza el segundo argumento. **Try-with-resources** garantiza que los recursos relacionados con archivos se liberen correctamente.
---

## Ejercicios
1. ¿Que excepcion puede ocurrir aqui que no podremos detectar mientras ejecutamos el codigo?
~~~java
try (Scanner scanner = new Scanner(new File("test.txt"))) {

    int number = scanner.nextInt();

    System.out.println(number * 100);

} catch (FileNotFoundException e) {
    e.printStackTrace();
} finally {
    System.out.println("Finally part");
}

// puede arrojar: InputMismatchException
~~~
2. El archivo `call_stat.txt` contiene informacion sobre llamadas de suscriptores moviles. Tiene la siguiente estructura: *subscriber_id1 subscriber_id2 duration_sec*. Encuentre todas las llamadas que duraron mas de 60 segundos e imprima el *subscriber_id1* en la consola. La duracion en el archivo se da en segundos.
~~~java
try (Scanner scanner = new Scanner(new File("call_stat.txt"))) {
    while (scanner.hasNext()) {
        String first = scanner.next();
        String second = scanner.next();
        int duration = scanner.nextInt();
        if (duration > 60) {
            System.out.println(first);
        }
    }
}
~~~
3. Cual es el objetivo de **try-with-resources**?
- Garantizar la liberacion de recursos del sistema
4. Cuantas clases pueden ser inicializadas y manejadas dentro de los corchetes de una construccion **try-with-resources**?
- Una o mas
5. Complete el codigo con las palabras faltantes.
~~~java
try (Scanner scanner = new Scanner(new File("test.txt"))) {
    while (scanner.hasNext()) {
        System.out.println(scanner.nextLine());
    }
} catch (FileNotFoundException e) {
    e.printStackTrace();
}
~~~
6. Elija los codigos que compilaran correctamente.
~~~java
// codigo 1
try (Scanner scanner1 = new Scanner(new File("test1.txt"));
     Scanner scanner2 = new Scanner(new File("test2.txt"))) {
        // some code
}

// codigo 2
Reader reader = new FileReader("file.txt");
try (reader) {
    // some code
}
~~~
