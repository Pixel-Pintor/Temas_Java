# Argumentos de la linea de comandos
Como ya sabes, el metodo `main` es un punto de entrada a cada programa Java
~~~java
public static void main(String[] args) { }
~~~
Como funcion como cualquier metodo, puede recibir algunos parametros a traves de `String[] args`, se llaman **argumentos de la linea de comandos**.  
Hay diferentes forma de proporcionar argumentos de linea de comando a un programa. Por ejemplo, puede definirlos con sus herramientas IDE o escribirlos manualmente como parametros de comando de ejecucion del programa:
~~~
java Main myArg
~~~
Podemos pasar un numero diferente de parametros separandolos con espacio y obtener los valores en `main(...)` como miembros de la matriz:
~~~java
public class Main {
    public static void main(String[] args) {
        System.out.println(args[0]);
        System.out.println(args[1]);
    }
}
~~~
Ahora bien, si compilamos `Main` con:
~~~
javac Main.java
~~~
Y luego llamamos los dos argumentos obtendremos el resultado esperado:
~~~
java Main myFirstArgument mySecondArgument
myFirstArgument
mySecondArgument
~~~
Si queremos pasar un argumento que consta de multiples palabras separadas por un espacio en blanco, debemos poner dicho argumento entre comillas dobles:
~~~
java Main myFirstArg "multiple word arg"
myFirstArg
multiple word arg
~~~
Los argumentos de la linea de comandos siempre se proporcionan como una matriz de `String`. Todavia puede pasar valores numericos, pero seran aceptados como `String`. La conversion a otro tipo debe hacerse manualmente en el programa.
~~~java
public class Main {
    public class void main(String[] args) {
        int i = Integer.parseInt(args[0]);
        System.out.println("Provider number: " + i);
    }
}
~~~
Ahora si lo probamos con un valor entero correcto, se ve asi:
~~~
java Main 5
Provided number: 5
~~~
Que pasar, si hay dos diferentes metodos `main`: con y sin argumentos.
~~~java
public class Main {
    
    public static void main(String[] args) {
        System.out.println("Method with arguments called!");
        System.out.println(args[0]);
    }

    public static void main() {
        System.out.println("No arguments method called!");
    }
    
}
~~~
Si vuelve a compilar e intenta llamar al programa con una lista de argumentos vacia y luego con cualquier argumento, el primer metodo se llamara en ambas ocasiones, eso significa que el metodo `main` siempre tiene que aceptar argumentos, pero no siempre tiene que proporcionarlos. Siempre tiene que definir un `String[] args`.