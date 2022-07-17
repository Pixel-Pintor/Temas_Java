# Streams Paralelos

Una de las mejores caracteristicas de *Stream API* y la programacion funcional en general es la capacidad de escribir facilmente codigo claro para el procesamiento de datos en paralelo. No es necesario crear streams manualmente, compruebe si el codigo esta bien sincronizado e invoque los metodos **wait/notify**. Todas estas cosas se realizan dentro de los streams paralelos automaticamente.

## Creando Streams paralelos

Hay varias formas sencillas de crear streams paralelos.

- Para invocar el metodo **parallelStream()** de una coleccion en lugar de **stream()**:

    ~~~java
    List<String> languages = List.of("java", "scala", "kotlin", "C#");

    List<String> jvmLanguages = languages.parallelStream()
            .filter(lang -> !Objects.equals(lang, "C#"))
            .collect(Collectors.toList());

    System.out.println(jvmLanguages); // [java, scala, kotlin]
    ~~~

- Para transformar una stream existente en un stream paralelo usando el metodo **parallel()**:

    ~~~java
    long sum = LongStream
            .rangeCloses(1, 1_000_000)
            .parallel()
            .sum();

    System.out.println(sum); // 500000500000
    ~~~

Como puede ver, a pesar de que usamos streams paralelos, todo el codigo para trabajar con ellos sigue siendo el mismo que antes.  
Existen dos metodos adicionales para trabajar con streams paralelos:

- **isParallel()** retorna **true** si el stream es paralelo y **false** si no
- **sequential()** retorna un stream secuencial equivalente.

Es importante que cualquier stream sea *paralela* o *secuencial*. Un modo mixto es imposible. Si un stream pipeline llama ambos metodos **parallel** y **sequential**, la ultima llamada gana.

## Rendimiento de streams paralelos

Es muy facil hacer un stream paralelo. Pero, ¿deberiamos hacer esto siempre? realmente no. Un *stream paralelo* no siempre es mas rapido que el stream secuencial equivalente.  
Hay una serie de factores que afectan significativamente el rendimiento de un stream paralelo.

- *Tamaño de los datos*. A mayor tamaño de datos -> mayor aceleracion
- *Empaquetado/Desempaquetado*. Los valores primitivos se procesan mas rapido que los valores empaquetados.
- *El numero de nucleos estan disponibles en tiempo de ejecucion*. Cuantos mas nucleos disponibles -> mayor aceleracion.
- *Costo de procesamiento de elementos*. Cuanto mas tiempo se procesa cada elemento -> mas eficiente es la paralelizacion. Pero no se recomienda utilizar stream paralelo para realizar operaciones demasiado largas (por ejemplo, conexiones de red). Asi que es una especie de compensacion.
- *El tipo de fuente de datos*. Por lo general, la fuente de datos inicial es una coleccion. Cuanto mas facil se divide en partes -> mayor aceleracion. Por ejemplo, arreglos regulares, **ArrayList** y **IntStream.range** son buenas fuentes para la division de datos, ya que admite el acceso aleatorio. Otros, como **LinkedList**, **Stream.iterate** son malas fuentes para la division de datos.
- *Tipo de operaciones: stateless y stateful*. Operaciones sin estado (por ejemplo, **filter** y **map**) son mejores para el procesamiento paralelo que las operaciones con estado (por ejemplo, **distinct**, **sorted**, **limit**). Las operaciones que se basan en el orden de los elementos son especialmente dificiles de paralelizar. Pero no siempre es posible evitarlos.

## El metodo reduce y su valor inicial

Supongamos que desea calcular la suma de numeros y sumar 100 al resultado. Al usar un stream secuencial, puede configurar 100 como el valor inicial (semilla) del metodo **reduce()**:

~~~java
int result = numbers.stream().reduce(100, Integer::sum);

// Este codigo produce el mismo resultado que el siguiente:
int result = numbers.stream().reduce(0, Integer::sum) + 100;
~~~

Pero cuando se usa un stream paralelo, el primer codigo producira un *resultado extraño*. El motivo es que su conjunto de datos se dividira en algunas partes y se agregara el valor 100 a cada una de ellas.

## El metodo forEach y el orden los elementos

Dada una lista ordenara de numeros **1, 2, ..., 10**. Nos gustaria procesar e imprimir cada numero de la lista usando secuencias. Aqui hay un stream secuencial.

~~~java
sortedNumbers.stream()
        .map(Function.identity())
        .forEach(n -> System.out.print(n + " "));

// la salida es:
1 2 3 4 5 6 7 8 9 10
~~~

Aqui hay un stream paralelo.

~~~java
sortedNumbers.parallelStream()
        .map(Function.identity())
        .forEach(n -> System.out.print(n + " "));

// la salida es 
6 7 9 10 8 3 4 1 5 2 
~~~

El metodo **forEach** rompe el orden cuando se usa con stream paralelos. Si reescribimos esto usando el metodo **forEachOrdered**, el codigo funcionara como esperabamos:

~~~java
sortedNumbers.parallelStream()
        .map(Function.identity())
        .forEachOrdered(n -> System.out.print(n + " "));

// la salida es:
1 2 3 4 5 6 7 8 9 10
~~~

## Listas vacias y el orden de los elementos

Puede que hayas pensado que ya no hay problemas con el orden de los elementos. Pero no es cierto: el orden y la paralelizacion no son amigos.  
Supongamos que nos gustaria obtener los tres primeros numeros pares de un stream paralelo de dos streams concatenados.

~~~java
// Crear una lista llena de enteros
List<Integer> numbers = List.of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

List<Integer> emptyList = List.of();
~~~

Aqui hay una concatenacion y procesamiento de dos listas.

~~~java
Stream.concat(numbers.stream(), emptyList.stream())
        .parallel()
        .filter(x -> x % 2 == 0)
        .limit(3)
        .forEachOrdered((n) -> System.out.print(n + " "));

// la salida es:
2 4 6
~~~

Esta bien. Pero si creamos una lista vacia usando **Collections.emptyList()**, entonces siempre tendremos una lista de salida diferente como **2 4 10**.  
La razon es que **Collections.emptyList()** no informa sobre su orden y el stream no puede usar el metodo **forEachOrdered** correctamente.

## ¿Debo usar streams paralelos?

*Stream API* hace que sea muy fácil comenzar a usar transmisiones paralelas. Pero no siempre son más rápidos que los flujos secuenciales equivalentes, ya que su rendimiento depende de muchos factores, incluido el volumen de datos, el hardware y las operaciones utilizadas. Al mismo tiempo, es bastante difícil predecir la aceleración sin realizar mediciones en la realidad. Además, existen algunas posibles advertencias al usar una secuencia paralela, especialmente relacionadas con el orden de sus elementos.  
Por lo tanto, debe estar absolutamente seguro de por qué necesita flujos paralelos en su caso. Si hay suficientes recursos o las operaciones realizadas son simples, puede ser mejor usar flujos secuenciales. Pero si ha logrado una aceleración estable medible, puede intentar usar secuencias paralelas.

---

## Ejercicios

1. Cual de las siguiente razones acelaran la velociad de un stream paralelo?

    - Operaciones sin estado
    - Base de datos facil de dividir
    - Mas nucleos disponibles

2. Cuales declaraciones producirian 16 lineas como resultado?

    - **Stream.of(1, 2, 3, 4, 5).reduce(1,Integer::sum);**
    - **Stream.of(1, 2, 3, 4, 5).parallel().reduce(0, Integer::sum) + 1;**

3. Debe implementar un metodo que invierta el estado de cada flujo de la lista dada (paralelo -> secuencial) y visceversa. El metodo deberia retornar una nueva lista de streams que estan invertidos.

    ~~~java
    public class Main {

        private static List<LongStream> invertedStreams(List<LongStream> streams) {
            return streams.stream()
                    .map(str -> str.isParallel() ? str.sequential() : str.parallel())
                    .collect(Collectors.toList());
        }

        public static void main(String[] args) {
            Scanner scanner = new Scanner(System.in);

            List<Boolean> parallelFlags = Arrays
                    .stream(scanner.nextLine().split("\\s+"))
                    .map(Boolean::parseBoolean)
                    .collect(Collectors.toList());

        // :)
            List<LongStream> streams = Stream
                    .iterate(0,
                            i -> i < parallelFlags.size(),
                            i -> i + 1)
                    .map(i -> {
                        var stream = LongStream.of();
                        if (parallelFlags.get(i)) {
                            stream = stream.parallel();
                        }
                        return stream;
                    }).collect(Collectors.toList());

            List<String> invertedParallelFlagsAsStrings =
                    invertedStreams(streams).stream()
                            .map(LongStream::isParallel)
                            .map(Object::toString)
                            .collect(Collectors.toList());

            System.out.println(String.join(" ", invertedParallelFlagsAsStrings));
        }
    }
    ~~~

4. Necesitas crear stream paralelo para filtrar numeros primos en el rango dado. Despues de llamar al metodo **count()** debe retornar el recuento de numeros primos en el rango dado. El metodo estatico **NumberUtils.isPrime(someLong)** ya esta disponible para verificar cada numero.

    ~~~java
    class ParallelFilteringStream {

        public static LongStream createPrimesFilteringStream(long start, long end) {
            return LongStream.rangeClosed(start, end)
                    .parallel()
                    .filter(NumberUtils::isPrime);
        }

        public static void main(String[] args) {
            final Scanner scanner = new Scanner(System.in);
            final String[] vals = scanner.nextLine().split(" ");

            final long left = Long.valueOf(vals[0]);
            final long right = Long.valueOf(vals[1]);

            final LongStream stream = createPrimesFilteringStream(left, right);

            if (!stream.isParallel()) {
                throw new RuntimeException(
                    "You need to write a parallel stream, not sequential!");
            }

            final Long count = stream.boxed().count();

            System.out.println(count);
        }

        public static class NumberUtils {

            public static boolean isPrime(long n) {
                return n > 1 && LongStream
                        .rangeClosed(2, n - 1)
                        .noneMatch(divisor -> n % divisor == 0);
            }
        }
    }
    ~~~

5. Cuales declaraciones organizarian los numeros de una lista.

    ~~~java
    public static void main(String[] args) {

        List<Integer> numbers = List.of(3, 5, 4, 8, 9, 2);
        numbers.stream().sorted().forEach(System.out::println);
        numbers.parallelStream().sorted().forEachOrdered(System.out::println);
        numbers.parallelStream().sorted().sequential().forEach(System.out::println);
    }
    ~~~

6. Arregla el codigo para que imprima los numeros en orden.

    ~~~java
    class ParallelMapping {
        private static final Scanner SCANNER = new Scanner(System.in);

        public static void main(String[] args) {
            Arrays.stream(SCANNER.nextLine().split("\\s+"))
                    .map(Integer::parseInt)
                    .sorted()
                    .map(n -> n * 2)
                    .parallel()
                    .forEachOrdered(System.out::println);
        }
    }
    ~~~
