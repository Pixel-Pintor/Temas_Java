# Expresiones Lambda

Java es principalmente un lenguaje de programacion orientada a objetos. Admite clases, metodos, campos y otros conceptos de este paradigma. Aqui, los metodos son la forma principal de representar el comportamiento de objetos, clases y programas completos. Puede escribir absolutamente cualquier codigo dentro de sus cuerpos y luego invocar este codigo desde otras partes de su programa usando los nombres de los metodos. Este enfoque permite a los desarrolladores crear programas muy estructurados y faciles leer, pero a veces no es suficiente y debemos usar otras formas de representar el comportamiento en lugar de los metodos.  

## Expresiones Lambda

Por **expresion lambda**, nos referimos a una funcion que no esta vinculada a su numbre (una funcion anonima) pero que puede asignarse a una variable.  
La forma mas general de una expresion lambda se ve asi: `(parameters) -> { body };`. Aqui, la parte anterior `->` es la lista de parametros (como en los metodos), y la parte posterior es el cuerpo que puede devolver un valor. Los parentesis `{}` son necesarios solo para expresiones lambda de varias lineas.  
Otra cosa importante: como un objeto Java normal, una expresion lambda siempre tiene un tipo especial. Hay muchos tipos presentados en la biblioteca estandar de Java. En este tema, solo mencionaremos dos de ellos: `Function` y `BiFunction`. Consideremos una expresion lambda de una sola linea que simplemente multiplicad sus dos parametros.

~~~java
BiFunction<Integer, Integer, Integer> mult = (x, y) -> x * y;
~~~

La expresion tiene el tipo `BiFunction<Integer, Integer, Integer>` lo que significa que se necesitan dos valores `Integer` y devuelve un `Integer`.

~~~java
// si solo tiene un argumento "()" son opcionales
Function<Integer, Integer> adder1 = x -> x + 1;

// con inferencia de tipo
Function<Integer, Integer> mult2 = (Integer x) -> x * 2;

// con multiples declaraciones
Function<Integer, Integer> adder5 = (x) -> {
    x += 2;
    x += 3;
    return x;
};
~~~

## Invocar expresiones lambda

Una vez que se crea una expresion lambda, se puede usar en otros lugares de su programa como objeto Java normal. Puede invocar el cuerpo de una expresion usando metodos especiales como `apply` tantas veces como necesites. El nombre del metodo depende del tipo de expresion lambda.

~~~java
int result2x5 = mult.apply(2, 5); // 10
int result3x1 = mult.apply(3, 1); // 3
~~~

Entonces, podemos invocar una expresion lambda como un metodo regular pasando argumentos y obteniendo resultados.  

## Pasar expresiones lambda a metodos

Uno de los casos mas populares es pasar una expresion lambda a un metodo y luego llamarlo alli.  
Mira el metodo a continuacion. Toma un objeto del generico estandar.

~~~java
private static void printResultOfLambda(Function<String, Integer> function) {
    System.out.println(function.apply("HAPPY NEW YEAR 3000!"));
}
~~~

Esta funcion puede tomar un argumento `String` y devolver un resultado `Integer`. Para probar el metodo, vamos a crear un objeto y pasarlo al metodo.

~~~java
// retorna la longitud de una cadena
Function<String, Integer> f = s -> s.length();
printResultOfLambda(f); // imprime 20;
~~~

Tambien puede pasar una expresion lambda al metodo directamente sin una referencia intermedia.

~~~java
// pasando sin una referencia
printResultOfLambda(s -> s.length()); // el resultado es el mismo: 20
~~~

Como puede ver, podemos pasar nuestra funcion presentada por un objeto a un metodo como su argumento, si el metodo toma un objeto de una tipo adecuado. Luego, dentro del metodo, se invocara la funcion dada.

~~~java
// imprime el numero de digitos: 4
printResultOfLambda(s -> {
    int count = 0;
    for (char c : s.toCharArray()) {
        if (Character.isDigit(c)) {
            count++;
        }
    }
    return count;
});
~~~

¿Que es importante aqui? pasamos a `printResultOfLambda` no datos, sino alguno fragmento de codigo como datos. Entonces, podemos parametrizar el mismo metodos con un comportamiento diferente en tiempo de ejecucion. Asi es como se ven los usos tipicos de las expresiones lambda. Muchos metodos estandar pueden aceptar expresiones lambda. Una funcion que acepta o devuelve otra funcion se denomina **funcion de orden superior**. En terminos de Java, estamos hablanod de metodos o funciones que toman/devuelven `Function<T, R>`, `BiFunction<T, U, R>` u otros tipos.

## Cierres

Otro truco importante con las expresiones lambda es la posibilidad de capturar valores de un contexto donde se define la lambda y usar los valores dentro del cuerpo. Esta tecnica se llama **cierre**.  
La captura solo es posible si una variable de contexto tiene la palabra clave `final` o si es **efectivamente final**, es decir, la variable no se cambia en el codigo adicional. De lo contrario, ocurre un error.  
Veamos un ejemplo.

~~~java
final String hello = "Hello, ";
Function<String, String> helloFunction = (name) -> hello + name;

System.out.println(helloFunction.apply("Jhon"));
System.out.println(helloFunction.apply("Anastasia"));
~~~

La expresion lambda capturo la variable final `hello`. El resultado de este codigo es.

~~~java
Hello, Jhon
Hello, Anastasia
~~~

Consideremos el ejemplo con una variable efectivamente final.

~~~java
int constant = 100;
Function<Integer, Integer> adder100 = x -> x + constant;

System.out.println(adder100.apply(200));
System.out.println(adder100.apply(300));
~~~

La variable `constant` es efectivamente final y esta siendo capturada por la expresion lambda.

---

## Ejercicios

1. Escriba una expresion lambda que acepte un valor long y devuelve el siguiente numero par.

    ~~~java
    class Operator {

        // solucion 1
        public static LongUnaryOperator unaryOperator = (long x) -> {
            long val;
            if (x % 2 == 0) {
                val = x + 2;
            } else {
                val = x + 1;
            }
            return val;
        };

        // solucion 2
        public static LongUnaryOperator unaryOperator = (long x) -> x % 2 == 0 ? x + 2 : x + 1;
    }
    ~~~

2. Usando un cierre escribe una lambda que calcula la ecuacion.

    ~~~java
    class Operator {

        public static int a = 10;
        public static int b = 20;
        public static int c = 30;

        // solucion 1
        public static DoubleUnaryOperator unaryOperator = x -> a * x * x + b * x + c;

        // solucion 2
        public static DoubleUnaryOperator unaryOperator = x -> {
            return Math.por(x, 2) * a + b * x + c;
        }
    }
    ~~~

3. Implementar un metodo que aplique la funcin dada a todos los elementos del array introducido.

    ~~~java
    public class Main {

        public static <T> void applyFunction(T[] array, Function<T, T> func) {
            for (int i = 0; i < array.length; i++) {
                array[i] = func.apply(array[i]);
            }
        }

        public static void main(String[] args) {
            Scanner scanner = new Scanner(System.in);
            String method = scanner.nextLine();
            String[] array = scanner.nextLine().split(" ");
            applyFunction(array,
                "lower".equals(method) ? String::toLowerCase :
                "upper".equals(method) ? String::toUpperCase :
                "new".equals(method) ? String::new :
                "trim".equals(method) ? String::trim : String::intern);
            Arrays.stream(array).forEach(e -> System.out.print(e + " "));
        }
    }
    ~~~

4. Selecciona cuales de las funciones es una funcion de orden superior.

    ~~~java
    // una funcion que acepte o retorne otra funcion es una funcion de orden superior
    Function<T, R> method(int n)
    Function<T, R> method(Function<T, R> f, int n)
    void method(Function<T, R> f)
    ~~~

5. Escrina una lambda que retorne el mayor de dos numeros.

    ~~~java
    class Operator {

        public static IntBinaryOperator binaryOperator = (x, y) -> x > y ? x : y;
    }
    ~~~

6. Hay algunas partes del codigo con cierres. Encuentra el correcto.

    ~~~java
    int a = 3;
    Function<Integer, Integer> f = (x) -> a * x;
    ~~~
