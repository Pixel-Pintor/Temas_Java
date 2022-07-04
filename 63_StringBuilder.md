# StringBuilder

Como sabra, las cadenas en Java son **inmutables**. Significa que una vez creada, una cadena no se puede cambiar. Si queremos modificar el contenido de un objeto de cadena, debemos crear una nueva cadena. Afortunadamente, hay una clase especial llamada `StringBuilder` que se utiliza para crear objetos de cadena mutables. Un objeto de esta calse es similar a una cadena regular, excepto que se puede modificar. Como ejemplo, es mejor usar `StringBuilder` cuando se hacen muchas concatenaciones en tiempo de ejecucion.  
Es posible crear una objeot vacio de tipo `StringBuilder`:
~~~java
StringBuilder empty = new StringBuilder();
System.out.println(empty); // ""

// o pasarle una cadena
StringBuilder sb = new StringBuilder("Hello!");
System.out.println(sb); // "Hello!"
~~~

## Algunos metodos importantes
La clase `StringBuilder` proporciona un conjunto de metodos utiles para manipular objetos.
- `int length()` devuelve la longitud, como en las cadenas normales.
~~~java
StringBuilder sb = new StringBuilder("I use Java");
System.out.println(sb.length()); // 10
~~~
- `char charAt(int index)` devuelve un caracter ubicado en el indice especificado.
~~~java
StringBuilder sb = new StringBuilder("I use Java");
System.out.println(sb.charAt(0)); // 'I'
System.out.println(sb.charAt(6)); // 'J'
~~~
- `void setCharAt(int index, char ch)` establece un caracter ubicado en el indice especificado en **ch**.
~~~java
StringBuilder sb = new StringBuilder("start");
sb.setCharAt(1, 'm');
System.out.println(sb); // "smart"
~~~
- `StringBuilder deleteCharAt(int index)` elimina el caracter en la posicion especificada.
~~~java
StringBuilder sb = new StringBuilder("dessert");
sb.deleteCharAt(2);
System.out.println(sb); // "desert"
~~~
- `StringBuilder append(String str)` concatena la cadena dada al final de la invocacion del objeto `StringBuilder`.
~~~java
StringBuilder sb = new StringBuilder("abc");
sb.append("123");
System.out.println(sb); // "abc123"  
Tambien es posible invocar este metodo varias veces en el mismo objeto en la misma declaracion porque este metodo devuelve el mismo objeto modificado. 
~~~java
StringBuilder messageBuilder = new StringBuilder(); // empty

messageBuilder
        .append("From: Kate@gmail.com\n")
        .append("To: Max@gmail.com\n")
        .append("Text: I lost my keys.\n")
        .append("Please, open the door!");

System.out.println(messageBuilder);

// output
From: Kate@gmail.com
To: Max@gmail.com
Text: I lost my keys.
Please, open the door!
~~~
- `StringBuilder insert(int offset, String str)` inserta la cadena dada en el objeto existente. Este metodo tiene muchas sobrecargas para diferentes tipos.
~~~java
StringBuilder sb = new StringBuilder("I'm a programmer.");
sb.insert(6, "Java ");
System.out.println(sb); // I'm a Java programmer.
~~~
- `StringBuilder replace(int start, int end, String str)` reemplaza la cadena desde el indice de cadena especificado (inclusive) hasta el indice final (exclusivo) con una cadena determinada.
~~~java
StringBuilder sb = new StringBuilder("Let's use C#");
sb.replace(10,12,"Java");
System.out.println(sb); // Let's use Java
~~~
- `StringBuilder delete(int start, int end)` elimina la subcadena desde el indice inicial (inclusivo) hasta el indice final (exclusivo).
~~~java
StringBuilder sb = new StringBuilder("Welcome");
sb.delete(0,3);
System.out.println(sb); // "come"
~~~
- `StringBuilder reverse()` hace que esta secuencia de caracteres sea reemplazada por el reverso de la secuencias.
~~~java
StringBuilder sb = new StringBuilder("2 * 3 + 8 * 4");
sb.reverse();
System.out.println(sb); // "4 * 8 + 3 * 2"
~~~
Tenga en cuenta que un objeto `StringBuilder`, se puede convertir en `String` llamando al metodo `toString()`.  

## length() y capacity()
Hay dos metodos que no deben confundirse: `length` y `capacity`. El metodo `length` devuelve el numero real de caracteres mientras que `capacity` devuelve la cantidad de almacenamiento disponible para los caracteres recien insertados, mas all de la cual se producira una asginacion. La capacidad es una parte de la representacion interna de `StringBuilder`, y su valor cambiara dinamicamente.  
El siguiente ejemplo le ayudara a distinguir mejor estos metodos:
~~~java
StringBuilder sb = new StringBuilder(); // initial capacity is 16

System.out.println(sb.length());   // 0
System.out.println(sb.capacity()); // 16

sb.append("A very long string");

System.out.println(sb.length());   // 18
System.out.println(sb.capacity()); // 34
~~~
Es posible especificar la capacidad al crear una objeto `StringBuilder`, pero no se usa a menudo:
~~~java
StringBuilder sb = new StringBuilder(30);

System.out.println(sb.length()); // 0
System.out.println(sb.capacity()); // 30
~~~
---

## Ejercicios
1. Implemente un metodo para concatenar todas las cadenas de una matriz en una sola cadena. Debe omitir todos los digitos dentro de la cadena de entrada.
~~~java
class ConcatenateStringsProblem {

    public static String concatenateStringsWithoutDigits(String[] strings) {
        StringBuilder str = new StringBuilder();
        for (String string : strings) {
            str.append(string);
        }
        for (int i = 0; i < str.length(); i++) {
            if (Character.isDigit(str.charAt(i))) {
                str.deleteCharAt(i);
            }
        }
        return str.toString();
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String[] strings = scanner.nextLine().split("\\s+");
        String result = concatenateStringsWithoutDigits(strings);
        System.out.println(result);
    }
}
~~~
