# Metodos de Reduccion

Al trabajar con colecciones en Java, a menudo puede enfrentar el desafío de reducir todos los elementos de la colección a un solo resultado. El ejemplo de tal problema es encontrar una cuenta bancaria con la cantidad máxima de dinero entre la colección de cuentas o reducir los valores de transacción de la cuenta a la cantidad total de dinero transferido. Java Stream API proporciona varios metodos *terminal* que nos permiten resolver este problema de manera funcional. Estas operaciones combinan elementos de stream y devuelven un solo valor.

## La operacion reduce

**reduce** es un metodo de la clase **Stream** que combina elementos de un stream en un solo *valor*. El resultado puede ser un valor de un tipo primitivo o un objeto complejo.  
Tenga en cuenta que **reduce** es una operacion terminal, lo que significa que comienza todas las evaluaciones con la secuencia y produce un resultado final.  
Consideremos dos de los usos mas comunes de **reduce**.

1. En el caso mas simple el metodo **reduce** acepta un *acumulador* de dos argumentos. El primero es un resultado parcial de la reduccion, el segundo es el siguiente elemento de un stream. El acumulador debe devolver un valor de reduccion que se asignara al resultado parcial. Consideremos la siguiente lista de valores transaccionales.

    ~~~java
    List<Integer> transactions = List.of(20, 40, -60, 5);
    ~~~

    Ahora sumamos todos los valores de transaccion para obtener la cantidad total de dinero transferido usando la operacion **reduce**:

    ~~~java
    transactions.stream().reduce((sum, transaction) -> sum + transaction);
    ~~~
    
    En la primera iteracion de la reduccion, el argument **sum** es igual al primero elemento de la secuencia cuyo valor es 20. El argumento **transaction** representa el siguiente elemento de la secuencia cuyo valor es 40. Despues de la primera iteracion, el **sum** acumula el valor actual del argumento **transaction** y sera igual a 20 + 40 = 60.  
    La operacion **reduce** que acepta la funcion de acumulador retorna un **Optional**. En el ejemplo anterior el metodo **reduce** retorna un contenedor **Optional<Integer>** que contiene el valor **Integer** de 5.
    
2. Otra implementacion de **reduce** tiene un parametro adicional: valor de identidad y semilla. La *identidad* representa el valor inicial de la operacion de reduccion. Reescribamos nuestro ejemplo anterior usando el argumento **identity**:

    ~~~java
    transactions.stream().reduce(0, (sum, transaction) -> sum + transaction);
    ~~~

    Ahora, el valor inicial del resultado parcial **sum** es 0 y el valor inicial de la **transaction** del elemento es 20.  
    Tenga en cuenta que si un **reduce** acepta tanto el valor de identidad como la funcion de acumulador, devolvera un tipo primitivo o un objeto, pero no un **Optional**. Si el stream esta vacia, la operacion **reduce** retorna el valor de identidad.

## Otras operaciones de reduccion

Mientras que **reduce** es una operación de propósito general, Stream API proporciona muchas operaciones de reducción específicas como **sum**, **min**, **max**, etc. Similar a la operación **reduce** son operaciones terminales que producen un solo valor. Consideremos un ejemplo con un stream genérico, asumiendo que necesitamos encontrar el valor máximo de la lista de saldos dados. Podemos hacerlo usando la operación de reducción:

~~~java
transactions.stream().reduce((t1, t2) -> t2 > t1 ? t2 : t1)
~~~

El código anterior compara un resultado parcial. **t1** con el siguiente elemento del stream **t2** y asigna el valor de **t2** a **t1** si **t2** es numéricamente mayor que **t1**. Podemos obtener el mismo resultado de una manera más elegante usando el método **max**:

~~~java
transactions.stream().max(Integer::compareTo);
~~~

Como puede notar, el método **max** de la clase **Stream<T>** acepta una función de comparación para encontrar un elemento máximo entre los elementos de flujo, ya que necesitamos especificar cómo comparar exactamente el tipo genérico. A diferencia de los flujos genéricos, los flujos primitivos especializados como **IntStream** o **LongStream** ya "sabe" el tipo de sus elementos. Por eso sus funciones **max** y **min** no requieren ninguna función de comparación:

~~~java
IntStream.of(20, 40, -60, 5).max();
~~~

Debido a la conciencia de tipo, los flujos primitivos especializados tienen funciones numéricamente especializadas como **average** y **sum**, que no pueden proporcionar los flujos genéricos.

---

## Ejercicios

1. Calcular el factorial de numero usando stream.

    ~~~java
    class Main {

        public static long factorial(long n) {
            return LongStream.rangeClosed(1, n).reduce(1, (a, b) -> a * b);
        }

        public static void main(String[] args) {

            Scanner scanner = new Scanner(System.in);
            long n = Integer.parseInt(scanner.nextLine().trim());
            System.out.println(factorial(n));
        }
    }
    ~~~

2. Implemente un metodo que toma limites de un rango y calcula la suma de los cuadrados de los elementos que pertenecen al rango.

    ~~~java
    class QuadraticSum {
        public static long rangeQuadraticSum(int fromIncl, int toExcl) {
            return LongStream.range(fromIncl, toExcl)
                    .reduce(0, (sum, number) -> sum + (number * number));
        }
    }
    ~~~

3. Conecte las variables con sus descripciones.

    ~~~java
    stream.reduce(a, (b, c) -> d);
    // a -> el valor por defecto si no hay valores en el stream
    // b -> la suma de todos enteros precedentes hasta ahora
    // c -> El siguiente elemento en el stream
    // d -> El resultado de la reduccion si el stream tiene elementos
    ~~~

4. Cual persona retornara el codigo.

    ~~~java
    List<Person> persons = Arrays.asList(
            new Person("Mary", 18),
            new Person("John", 21),
            new Person("Andrew", 31),
            new Person("Julia", 19)
    );

    Person person = persons.stream()
        .reduce(new Person("DEFAULT", 0), (p1, p2) -> p1.getAge() > p2.getAge() ? p1 : p2);

    // Andrew
    ~~~

5. Cual es el resultado del codigo.

    ~~~java
    long result = Stream.of(1, 2, 3, 4, 5).reduce(1, (x, y) -> x * y);

    // result: 5
    ~~~

6. Seleccione las operaciones de reduccion que combinan los elementos dentro de uno solo.

    - **max**
    - **average**
    - **min**
    - **sum**

7. Para un rango dado de **a** y **b** ambos inclusive, contar la suma de numeros que son divisibles por **n** o **m**.

    ~~~java
    public class Main {

        public static int sum(int a, int b, int n, int m) {
            return IntStream.rangeClosed(a, b)
                    .filter(x -> x % n == 0 || x % m == 0)
                    .sum();
        }

        public static void main(String[] args) {
            Scanner scanner = new Scanner(System.in);
            int a = scanner.nextInt();
            int b = scanner.nextInt();
            int n = scanner.nextInt();
            int m = scanner.nextInt();

            System.out.println(sum(a, b, n, m));
        }
    }
    ~~~

8. Un stream tiene la operacion **count** que retorna el numero de elementos en la secuencia. Como seria su implementacion con un **stream**.

    ~~~java
    stream.reduce(0, (acc, next) -> acc + 1);
    ~~~

9. Seleccione las declaraciones correctas sobre la operacion de reduccion.

    - La operacion de reduccion combina elementos de un stream en un solo valor
    - El primer argumentos (identidad) es tanto el valor inicial de la reduccion como el resultado predeterminado si no hay elementos en el stream.
    - Es una operacion terminal
    - La operacion reduccion se puede usar para implementar la operacion de suma.
