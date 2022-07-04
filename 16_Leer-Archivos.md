# Lectura de Archivos
Es posible usar `java.util.Scanner` para leer datos de archivos. Esta clase es un enfoque de alto nivel para leer datos de entrada. Permite leer tipos primitivos o cadenas mediante el uso de expresiones regulares.  
Supongamos que tiene una cadena llamada `pathToFile`. Mantiene la ruta a un archivo que contiene una secuencia de numeros separados por espacios. Vamos a crear una objeto llamado archivo y luego un escaner para leer datos del archivo.
~~~java
File file = new File(pathToFile);
Scanner scanner = new Scanner(file); // arroja FileNotFoundException
~~~
Cuando crear una instancia de `Scanner` al pasar un archivo, debe manejar la excepcion `FileNotFoundException`.  
Ahora, podemos usar metodos de `Scanner` para leer datos como cadenas, enteros, etc.
~~~java
// lee cada linea del archivo y la imprime
while (scanner.hasNext()) {
    System.out.print(scanner.nextLine());
}
~~~
Despues de usar una escaner, debemos cerrar el objeto. Una forma conveniente de cerrar los escaneres y manejar las excepiones es usar la **prueba con recursos**. Este es un ejemplo completo de `Scanner`.
~~~java
File file = new File(pathToFile);

try (Scanner scanner = new Scanner(file)) {
    while (scanner.hasNext()) {
        System.out.print(scanner.nextLine() + " ");
    }
} catch (FileNotFoundException e) {
    System.out.println("No file found: " + pathToFile);
}

/*
Para una archivo que contenga las siguientes lineas:
first line
second line
third line

El programa genera en consola lo siguiente:
first line second line third line
*/
~~~
En caso de que no haya nuevos datos disponibles, cualquiera de los metodos `next` retorna `java.util.NoSuchElementException`.  
El siguiente metodo devuelve todo el texto de un archivo especificando:
~~~java
import java.nio.file.Files;
import java.nio.file.Paths;

public static String readFileAsString(String fileName) throws IOException {
    return new String(Files.readAllBytes(Paths.get(fileName)));
}
~~~
Intentemos usar el metodo `readFileAsString` para leer el codigo fuente del archivo `HelloWorld.java` e imprimalo en la salida estandar.
~~~java
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;

public class ReadingFileDemo {
    public static String readFileAsString(String fileName) throws IOException {
        return new String(Files.readAllBytes(Paths.get(fileName)));
    }

    public static void main(String[] args) {
        String pathToHelloWorldJava = "/home/username/Projects/hello-world/HelloWorld.java";
        try {
            System.out.println(readFileAsString(pathToHelloWorldJava));
        } catch (IOException e) {
            System.out.println("Cannot read file: " + e.getMessage());
        }
    }
}

// imprime el codigo fuente
package org.hyperskill;

public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, world!");
    }
}
~~~
Tenga en cuenta ue no es dificil modificar el codigo anterior para que lea la ruta al archivo de destino desde la entrada estandar en lugar de codificarlo.
---
## Ejercicios
1. Que tipo de excepcion puede lanzar el codigo.
~~~java
Files.readAllBytes(Path.of(pathToFile));

// puede arroja java.io.IOException
~~~
2. Cual sera el resultado del siguiente programa.
~~~java
String pathToFile = "file.txt";
try (Scanner scanner = new Scanner(new File(pathToFile))) {
    scanner.nextLine();
    scanner.nextLine();
    System.out.println(scanner.nextLine());
}

// El archivo contiene
first
second

// arroja: java.util.NoSuchElementException
~~~
3. Leer el archivo con numeros e imprimir la summa de todos.
~~~java
String pathFile = "C:\\Users\\cprad\\OneDrive\\Escritorio\\Java_Backend\\01_Simple-Banking-System\\Simple Banking System\\task\\src\\banking\\dataset.txt";
int sum = 0;
try (Scanner scanner = new Scanner(new File(pathFile))) {
    while (scanner.hasNext()) {
        sum += scanner.nextInt();
    }
} catch (FileNotFoundException e) {
    System.out.println(e.getMessage());
}
System.out.println("Total sum: " + sum);

// la suma total: 25412
~~~
4. Leer la lista con numeros y encontar el numero mas grande.
~~~java
String pathFile = "C:\\Users\\cprad\\OneDrive\\Escritorio\\Java_Backend\\01_Simple-Banking-System\\Simple Banking System\\task\\src\\banking\\dataset.txt";
ArrayList<Integer> numbers = new ArrayList<>();
try (Scanner scanner = new Scanner(new File(pathFile))) {
    while (scanner.hasNext()) {
        numbers.add(scanner.nextInt());
    }
} catch (FileNotFoundException e) {
    System.out.println(e.getMessage());
}

int maxNum = 0;
for (int num : numbers) {
    if (num > maxNum) {
        maxNum = num;
    }
}

System.out.println("Max Number: " + maxNum);

// Max Number: 999990
~~~
5. Escribe un programa que cuente los numeros que son mayores o iguales a 9999 de un archivo.
~~~java
String pathFile = "C:\\Users\\cprad\\OneDrive\\Escritorio\\Java_Backend\\01_Simple-Banking-System\\Simple Banking System\\task\\src\\banking\\dataset2.txt";
int numbers = 0;
try (Scanner scanner = new Scanner(new File(pathFile))) {
    while (scanner.hasNext()) {
        if (scanner.nextInt() >= 9999) {
            numbers++;
        }
    }
} catch (FileNotFoundException e) {
    System.out.println(e.getMessage());
}

System.out.println("Total numbers: " + numbers);

// Total numbers: 9899
~~~
6. Que genera el siguiente codigo que lee un archivo.
~~~java
String pathToFile = "file.txt";
try (Scanner scanner = new Scanner(new File(pathToFile))) {
    System.out.println(scanner.nextInt());
}

// Contenido del archivo
start line
line two
end line

// Genera: stacktrace of java.util.InputMismatchException
~~~
7. Cuenta los numeros pares en el archivo. Debe dejar de contar si encuentra un 0 o si es el ultimo numero.
~~~java
String pathFile = "C:\\Users\\cprad\\OneDrive\\Escritorio\\Java_Backend\\01_Simple-Banking-System\\Simple Banking System\\task\\src\\banking\\dataset4.txt";
int even = 0;
try (Scanner scanner = new Scanner(new File(pathFile))) {
    while (scanner.hasNext()) {
        if (scanner.nextInt() == 0) {
            break;
        } else if (scanner.nextInt() % 2 == 0){
            even++;
        }
    }
 catch (FileNotFoundException e) {
    System.out.println(e.getMessage());
}

System.out.println("Even numbers: " + even);

// Even numbers: 106
~~~
