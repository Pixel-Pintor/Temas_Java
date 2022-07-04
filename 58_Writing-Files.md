# Writing Files
Ahora que hemos aprendido como crear y administrar archivos, analicemos como escribir texto en un archivo. Java proporciona diferentes formas de hacerlo.

## La clase FileWriter
La clase `FileWriter` tiene un conjunto de constructores para escribir caracteres y cadenas en un archivo especifico.
- `FileWriter(String fileName)`
- `FileWriter(String fileName, boolean append)`
- `FileWriter(File file)`
- `FileWriter(File file, boolean append)`  
Dos constructores toman un parametro adicional `append` que indicar si agregar (`true`) o sobrescribir (`false`) un archivo existente.  
Todos los constructores pueden lanzar un `IOException` por varias razones.
- si el archivo mencionado existe pero es un directorio
- si un archivo no existe y no se puede crear
- si existe un archivo pero no se pude abrir
En esta leccion, a veces omitiremos el mecanismo de manejo de excepciones para simplificar nuestros ejemplos. Consideremos el siguiente codigo:
~~~java
File file = new File("/home/username/path/to/your/file.txt");
FileWriter writer = new FileWriter(file); // sobrescribe el archivo

writer.write("Hello");
writer.write("Java");

writer.close();
~~~
Si el archivo especificado no existe, se creara despues de ejecutar este codigo. Si el archivo ya existe, este codigo sobrescribe los datos. El archivo contendra el texto **HelloJava**.  
Si desea agregar algunos datos nuevos, debe especificar el segundo argumento como verdadero.
~~~java
File file = new File("/home/username/path/to/your/file.txt");
FileWriter writer = new FileWriter(file, true); // agrega texto al archivo 

writer.write("Hello, World\n");
writer.close();
~~~
Este codigo agrega una nueva linea al archivo. Ejecutelo varias veces para ver que sucede. Tenga en cuenta que aqui estamos usando saltos de linea.   

## Cerrar un FileWriter
Es importante cerrar un `FileWriter` despues de usarlos para evitar un fuga de recursos. Esto se hace invocando el metodo `close()`.
~~~java
writer.close();
~~~
Desde Java 7, una forma conveniente de cerrar un objeto `FileWriter` es usar un **try-with-resources**.
~~~java
File file = new File("/home/username/path/to/your/file.txt");

try (FileWriter writer = new FileWriter(file)) {
    writer.write("Hello, World");
} catch (IOException e) {
    System.out.printf("An exception occurred %s", e.getMessage());
}
~~~
Cerrar el Writer automaticamente.

## La clase PrintWriter
La clase `PrintWriter` le permite escribir datos formateados en un archivo. Puede generar cadenas, tipos primitivos e incluso una matriz de caracteres. La clase proporciona varios metodos sobrecargados: `print`, `println` y `printf`.
~~~java
File file = new File("/home/art/Documents/file.txt");
try (PrintWriter printWriter = new PrintWriter(file)) {
    printWriter.print("Hello"); // imprime una cadena
    printWriter.println("Java"); // imprime una cadena y termina la linea
    printWriter.println(123); // imprime un numero
    printWriter.printf("You have %d %s", 400, "gold coins"); // imprime una cadena formateada
} catch (IOException e) {
    System.out.printf("An exception occurred %s", e.getMessage());
}
~~~
Este ejemplo primera crea una instancia de `File` y, en segundo lugar, un `PrintWriter` en **try-with-resources** para cerrarla correctamente. Escribe `"Hello"` y `"Java"` en la misma linea, y luego `123` en una nueva linea. Este ejemplo tambien llama al metodo `printf` para que puede formatear un texto usando `%d`, `%s` y asi. Finalmente, cierra `PrintWriter`. El resultado contiene:
~~~java
HelloJava
123
You have 400 gold coins
~~~
La clase tiene varios constructores. Algunos de ellos son similares a los constructores de `FileWriter`:
- `PrintWriter(String fileName)`
- `PrintWriter(File file)`
Otros dejan pasar `FileWriter` como una clase que extiende la  clase abstracta `Writer`:
- `PrintWriter(Writer writer)`

## Conclusion
`FileWriter` y `PrintWriter` ambos extienden la clase abstracta `Writer` y tienen muchas similitudes. Sin embargo, `PrintWriter` es de mas alto nivel y proporciona varios metodos utiles. Entre ellos se encuentran los metodos de formateo y los metodos de impresion sobrecargados para escribir tipos primitivos.
---

## Ejercicios
1. El siguiente codigo imprime para archivar la linea `Loren ipsum`
~~~java
File file = new File("file.txt"); // algun archivo
try (FileWriter writer = new FileWriter(file, true)) {
    writer.write("Loren ipsum");
}
~~~
Antes de la invocacion del metodo, nuestro archivo contendra una sola linea sin espacios finales: `before lating`. Escriba la linea que representa el contenido del archivo despues de la finalizacion del metodo.
~~~java
before latinLorem ipsum
~~~
2. ¿Que clase existente puede formatear su salida al escribir en archivos?
- `PrintWriter`
3. Suponga que tiene un archivo `file.txt` que contiene una linea `before latin`, como el siguiente codigo cambiara el contenido del archivo?
~~~java
File file = new File("file.txt");

try (FileWriter fileWriter = new FileWriter(file)) {
    fileWriter.write("Loren ipsum");
    fileWriter.write(" dolor sit amet");
}

// texto resultante: loren ipsum dolor sit amet
~~~
4. Cuales metodos son usados por `java.io.PrintWriter` para escribir en un archivo?
- `print`, `println` y `printf`
5. ¿Que imprimira la siguiente linea de codigo?
~~~java
File file = new File("file.txt"); 

try (PrintWriter printWriter = new PrintWriter(file)) {
    printWriter.printf("%s dolor sit %s", "Lorem", "ipsum", "amet")
}

// imprime: Lorem dolor sit ipsum
~~~
6. Cual metodo es usado por `java.io.FileWriter` para escribir en un archivo?
- `write()`
7. Tenemos un metodo que imprime un rango de numeros en un archivo, invocamos el metodo tres veces, cual seria la descripcion de la salida del codigo.
~~~java
public static void printRangeToFile(String file, boolean append, int fromIncl, int toExcl) {
    try (FileWriter writer = new FileWriter(file, append)) {
        for (int i = fromIncl; i < toExcl; i++) {
            writer.write(i + " ");
        }
    } catch (IOException e) {
        System.out.printf("An exception occurred %s", e.getMessage());
    }
}

String filepath = "file.txt"; // ruta relativa al archivo
printRangeToFile(filepath, false, 0, 10);
printRangeToFile(filepath, true, 10, 20);
printRangeToFile(filepath, false, 20, 30);

// Imprime los numeros de 20 a 29
~~~
8. Cuales instancias de `java.io.FileWriter` agregaran texto al final del archivo en lugar de sobre escribirlo.
~~~java
FileWriter writer = new FileWriter("/path/to/file", true);
FileWriter writer = new FileWriter(file, true);
~~~
9. Seleccione todas las declaraciones que escribiran `Loren ipsum dolor sit amet` en el archivo. Asume que `file.txt` esta vacio, y que `fileWriter` y `printWriter` se han inicializado.
~~~java
File file = new File("file.txt"); 
FileWriter fileWriter = new FileWriter(file);
PrintWriter printWriter = new PrintWriter(file);

// forma 1
printWriter.print("Loren ipsum");
printWriter.print("dolor sit amet");

// forma 2
fileWriter.write("Lorem ipsum dolor sit amet");

// forma 3
printWriter.printf("%s", "Loren ipsum dolor sit amet");

// forma 4
printWriter.printf("Lorem %s dolor %s", "ipsum", "sit amet");
~~~
