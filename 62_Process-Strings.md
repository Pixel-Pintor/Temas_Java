# Processing Strings
Como ya sabes, una cadena es una secuencia de caracteres con un solo tipo de datos, al igual que una matriz. Las cadenas son similares a las matrices en muchos aspectos: en cierto sentido, una cadena parece una matriz de caracteres. Ademas, puede iterar sobre cadenas y matrices. A veces, es posible que necesite procesar una cadena y convertirla en una matriz. 

## Strings y Arrays
Es posible convertir entre cadenas y matrices de caracteres usando metodos especiales como `valueOf()` y `toCharArray()`.
~~~java
char[] chars = { 'A', 'B', 'C', 'D', 'E', 'F' };

String stringFromChars = String.valueOf(chars); // "ABCDEF"

char[] charsFromString = stringFromChars.toCharArray(); 
// { 'A', 'B', 'C', 'D', 'E', 'F' }

String theSameString = new String(charsFromString); // "ABCDEF"

// Using the split() method
String text = "Hello";
String[] parts = text.split(""); // {"H", "e", "l", "l", "o"}
~~~

## Dividiendo String
Una cadena se puede dividir en una matriz de cadenas en funcion de un delimitador. Para realizar esto, llamamos al metodo `split`, que divide en subcadenas mediante un **separador**. En el ejemplo anterior, usamos el delimitador `""`, que divide automaticamente una cadena en elementos mas pequeños (subcadenas) que constan de un caracter.  
Si se especifica un delimitador, el metodo devuelve una matriz de todas las subcadenas. Tenga en cuenta que el delimitador en si no esta incluido en ninguna de las subcadenas.
~~~java
String sentence = "a long text";
String[] words = sentence.spli(" "); // {"a", "long", "string"}
~~~
Intentemos dividir un numero de telefono estadounidense en el codigo del pais, el codigo de area, el codigo de la oficina central y otros digitos restantes.
~~~java
String number = "+1-213-345-6789";
String[] parts = number.split("-"); // {"+1", "213", "345", "6789"}
~~~
Elija sabiamente su delimitador, de lo contrario podria obtener algunas oraciones que comienzan con un espacio.
~~~java
String text = "That's one small step for a man, one giant leap for mankind.";
String[] parts = text.split(","); 
// {"That's one small step for a man", " one giant leap for mankind."}
~~~
Puede elegir cualquier delimitador que prefiera, incluida una combinacion de espacios y palabras.
~~~java
String text = "I'm gonna be a programmer";
String[] parts = text.split(" gonna be "); // {"I'm", "a programmer"}
~~~
Como puede ver aqui, el metodo `spli()` es una buena herramienta para deshacerse de algo que no necesita o no quiere usar.

## Iterando sobre un String
Es posibles iterar sobre los caracteres de un String usando un bucle **(while, do-while, for-loop)**. Vea el siguiente ejemplo.
~~~java
String scientistName = "Isaac Newton";
for (int i = 0; i < scientistName.length(); i++) {
    System.out.print(scientistName.charAt(i) + " ");
}
~~~
En las cadenas, como en los arreglos, la indexacion comienza desde 0. En nuestro ejemplo, el ciclo for itera sobre la cadena `"Isaac Newton"`. Con cada iteracion, el metodo `charAt` devuelve el caracter actual en el indice, y ese caracter se imprime en la consola, seguido de un espacio en blanco.
---

## Ejercicios
1. Escriba una programa que calcula el porcentage de caracteres G y caracteres G en una cadena introducida, la respuesta debe ser tipo **double**.
~~~java
class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String line = scanner.nextLine();
        String[] lineArray = line.split("");
        int number = 0;
        for (String letter : lineArray) {
            if (Objects.equals(letter.toUpperCase(), "G") ||
            Objects.equals(letter.toUpperCase(), "C")) {
                number++;
            }
        }
        double percentage = ((double) number / lineArray.length) * 100.0;
        System.out.println(percentage);
    }
}
~~~
2. Obtener todos los parametros disponibles en una url, luego imprimelos en un formato **"key : value"**.
~~~java
class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String url = scanner.nextLine();
        String[] dataOne = url.split("\\?");
        String[] dataTwo = dataOne[1].split("&");
        ArrayList<String> dataThree = new ArrayList<>();
        String password = "";
        boolean pass = false;
        for (String data : dataTwo) {
            String[] dataLine = data.split("=");
            if (dataLine.length == 2) {
                dataThree.add(dataLine[0] + " : " + dataLine[1]);
                if (Objects.equals(dataLine[0], "pass")) {
                    password = "password : " + dataLine[1];
                    pass = true;
                }
            } else {
                dataThree.add(dataLine[0] + " : not found");
            }
        }
        if (pass) {
            dataThree.add(password);
        }
        for (String data : dataThree) {
            System.out.println(data);
        }
    }
}
~~~
3. Escribe un programa que lea una cadena e imprima otra con los caracteres dobles.
~~~java
class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String input = scanner.nextLine();
        char[] inputChar = input.toCharArray();
        StringBuilder sb = new StringBuilder();
        for (Character ch : inputChar) {
            sb.append(ch);
            sb.append(ch);
        }
        System.out.println(sb);
    }
}
~~~
4. Escribe un programa que encuentre las ocurrencias de una subcadena en una cadena. Las subcadenas no se pueden sobreponer.
~~~java
class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String string = scanner.nextLine();
        String subString = scanner.nextLine();

        final Pattern pattern = Pattern.compile(subString, Pattern.CASE_INSENSITIVE);
        final Matcher matcher = pattern.matcher(string);

        System.out.println(matcher.results().count());
    }
}
~~~
