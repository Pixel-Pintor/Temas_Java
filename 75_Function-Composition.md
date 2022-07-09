# Function Composition

En este tema, aprenderá una nueva técnica para trabajar con funciones llamada **composición**. Es un mecanismo de combinación de funciones para obtener funciones más complicadas que proviene originalmente de las matemáticas. En cierto sentido, puede considerarse como un patrón de diseño en la programación funcional. Puede usar este patrón para componer funciones estándar, operadores, predicados y consumidores (pero no proveedores). Echemos un vistazo a los ejemplos.

## Componer funciones

La interfaz funcional **Function<T, R>** tiene dos métodos predeterminados **compose** y **andThen** para componer nuevas funciones. La principal diferencia entre estos métodos radica en el orden de ejecución.  
En general, **f.compose(g).apply(x)** es lo mismo que **f(g(x))**, y **f.andThen(g).apply(x)** es lo mismo que **g(f(x))**.  
Aqui hay un ejemplo con dos funciones: **adder** y **multiplier**.

~~~java
Function<Integer, Integer> adder = x -> x + 10;
Function<Integer, Integer> multiplier = x -> x * 5;

// compose: adder(multiplier(5))
System.out.println("result: " + adder.compose(multiplier).apply(5));

// andThen: multiplier(adder(5))
System.out.println("result: " + adder.andThen(multiplier).apply(5));
~~~

En este caso, el método **compose** devuelve una función compuesta que aplica primero **multiplier** a su entrada, y luego aplica **adder** al resultado. El metodo **andThen** devuelve una función compuesta que aplica primero **adder** a su entrada, y luego aplica **multiplier** al resultado. Esta es la salida.

~~~java
result: 35
result: 75
~~~

Los operadores se pueden utilizar de la misma manera que las funciones. Los metodos **compose** y **andThen** no modifican las funciones que se combinan. En su lugar, devuelven nuevas funciones. Esto es cierto para todos los siguientes ejemplos.

## Componer resultados

Todas las interfaces funcionales que representan predicados (**Predicate<T>**, **IntPredicate** y otros) tienen tres metodos para componer nuevos predicados: **and**, **or** y **negate**.  
Hay dos predicados en el siguiente ejemplo: **idOdd** y **lessThan11**.

~~~java
IntPredicate idOdd = n -> n % 2 != 0; // true para los numeros impares

System.out.println(isOdd.test(10)); // false
System.out.println(isOdd.test(11)); // true

IntPredicate lessThan11 = n -> n < 11; // true para todos los numeros < 11

System.out.println(lessThan11.test(10)); // true
System.out.println(lessThan11.test(11)); // false
~~~

Neguemos el primer predicado:

~~~java
IntPredicate isEven = isOdd.negate(); // true para los numeros pares
System.out.println(isEven.test(10)); // true
System.out.println(isEven.test(11)); // false
~~~

Aqui tenemos un nuevo predicado que comprueba si el valor es par en lugar de impar.  
Ahora combinemos ambos predicados **isOdd** y **lessThan11** juntos usando los metodos **or** y **and**:

~~~java
IntPredicate isOddOrLessThan11 = isOdd.or(lessThan11);

System.out.println(isOddLessThan11.test(10)); // true
System.out.println(isOddLessThan11.test(11)); // true
System.out.println(isOddLessThan11.test(12)); // false
System.out.println(isOddLessThan11.test(13)); // true

IntPredicate isOddAndLessThan11 = isOdd.and(lessThan11);


System.out.println(isOddAndLessThan11.test(8));  // false
System.out.println(isOddAndLessThan11.test(9));  // true
System.out.println(isOddAndLessThan11.test(10)); // false
System.out.println(isOddAndLessThan11.test(11)); // false
~~~

Como puede ver, estos metodos son equivalentes a los operadores logicos **&&** y **||**, pero trabajan con funciones en lugar de sus valores.

## Composicion de Consumers

Puede resultar un poco sorprendente, pero tambien es posible combinar consumidores utilizando el metodo **andThen**. Simplemente devuelve un nuevo consumidor que consume el valor dado varias veces en una cadena.  
En el siguiente ejemplo, usamos **andThen** para imprimir un valor dos veces, pero es posible hacerlo mas veces.

~~~java
Consumer<String> consumer = System.out::println;
Consumer<String> doubleConsumer = consumer.andThen(System.out::println);
doubleConsumer.accept("Hi!");
~~~

Aqui esta la salida.

~~~java
Hi!
Hi!
~~~

En una situacion real, podria usarlo para enviar una valor a diferentes destinos, como una base de datos o un registrador. Aunque es posible combinar **consumidores**, no es posible combinar **proveedores**.

---

## Ejercicios

1. Escribe el metodo **disjunctAll** que acepta una lista de objetos **IntPredicate** y devuelve un unico **IntPredicate**. El predicado de resultado es una **disyuncion** de todos los predicados de entrada. La **disyuncion** significa que si alguno de los predicados regresara **true**, el predicado compuesto deberia devolver **true** tambien. Si la lista de entrada esta vacia, el predicado resultante debe devolver **false** para cualquier valor entero.

    ~~~java
    class CombiningPredicates {

        public static IntPredicate disjunctAll(List<IntPredicate> predicates) {
    
            // solucion 1
            IntPredicate myPredicate = n -> false;
            for (IntPredicate predicate : predicates) {
                myPredicate = myPredicate.or(predicate);
            }
            return myPredicate;

            // solucion 2
            return predicates.stream().reduce(
                    IntPredicate::or
            ).orElse(i -> false);

            // solucion 3
            if (predicates.isEmpty()) {
                return n -> false;
            }
            return predicates.stream().reduce(IntPredicate::or).get();

            // solucion 4
            return predicates.stream()
                    .reduce((p1, p2) -> p1.or(p2))
                    .orElse(p -> false);
        }

        public static void main(String[] args) {

            Scanner scanner = new Scanner(System.in);

            String[] strings = scanner.nextLine().split(" ");

            List<Boolean> values = Arrays.stream(strings)
                    .map(Boolean::parseBoolean)
                    .collect(Collectors.toList());

            List<IntPredicate> predicates = new ArrayList<>();
            values.forEach(v -> predicates.add(x -> v));

            System.out.println(disjunctAll(predicates).test(0));
        }
    }
    ~~~

2. Que imprimira el codigo.

    ~~~java
    Consumer<Integer> printer = System.out::println;
    Consumer<Integer> devNull = (val) -> { int v = val * 2; };

    Consumer<Integer> combinedConsumer = devNull.andThen(devNull.andThen(printer));
    combinedConsumer.accept(100);

    // 100
    ~~~

3. Dadas tres funciones, estas funciones se componen dentro de una funcion, en el sentido matematico esta composicion es equivalente a que?

    ~~~java
    Function<Double, Double> f = ...
    Function<Double, Double> h = ...
    Function<Double, Double> g = ...

    Function<Double, Double> composed = f.andThen(g).compose(h);

    // Equivale a: g(f(h(x)))
    ~~~

4. Cual es el resultado del siguiente codigo.

    ~~~java
    IntUnaryOperator mult2 = num -> num * 2;
    IntUnaryOperator add3 = num -> num + 3;

    IntUnaryOperator combinedOperator = add3.compose(mult2.andThen(add3)).andThen(mult2);
    int result = combinedOperator.applyAsInt(5);
    System.out.println(result); // 32
    ~~~

5. Cual funcion se ejecuta primero despues de llamar la aplicacion de **bigFunction**.

    ~~~java
    Function<Integer, Integer> next = n -> n + 1;
    Function<Integer, Integer> previous = n -> n - 1;
    Function<Integer, Integer> square = n -> n * n;
    Function<Integer, Integer> mult3 = n -> n * 3;

    Function<Integer, Integer> bigFunction = 
        next.compose(next.compose(square).andThen(previous)).andThen(mult3);

    // Respuesta: square
    ~~~

6. Cual es el resultado del codigo.

    ~~~java
    IntUnaryOperator next = x -> x + 1;
    IntUnaryOperator square = x -> x * x;

    IntUnaryOperator combinedOperator = next.compose(square);
    int result = combinedOperator.applyAsInt(5);
    System.out.println(result); // 26
    ~~~

7. Componer los codigos para que de el codigo de resultado.

    ~~~java
    Predicate<Character> isLetter = Character::isLetter;
    Predicate<Character> isUpperCase = Character::isUpperCase;
    Predicate<Character> predicate = isLetter.and(isUpperCase.negate());

    // Codigo de resultado
    predicate.test('1'); // false
    predicate.test('3'); // false
    predicate.test('c'); // true
    predicate.test('D'); // false
    predicate.test('e'); // true
    predicate.test('Q'); // false
    ~~~

8. Escriba el resultado del codigo.

    ~~~java
    Function<String, String> operator1 = s -> s + s;
    Function<String, String> operator2 = s -> String.valueOf(s.length());
    Function<String, String> resultOperator = operator1.compose(operator2);

    System.out.println(resultOperator.apply("test")); // 44
    ~~~
