# Map y FlatMap

Una de las tareas más comunes en la programación es convertir colecciones de objetos en otras nuevas aplicando la misma función a cada elemento de la colección. Esta tarea se puede resolver con un enfoque funcional. Como ya conoce la API Java Stream, estamos listos para analizar más de cerca las operaciones intermedias **map** y **flatMap**.

## La operacion Map

**map** es un método de la clase **Stream** que toma una función de un argumento como parámetro. El propósito principal de la operación **map** es aplicar esa función a cada elemento de la secuencia y devolver una secuencia resultante que tenga la misma cantidad de elementos.

1. Uno de los usos mas comunes de **map** es aplicar una funcion dada a cada elementos de un stream de valores. Considere la siguiente lista de numeros.

    ~~~java
    List<Double> numbers = List.of(6.28, 5.42, 84.0, 26.0);
    ~~~

    Dividamos cada numeros por 2. Para eso, podemos *asignar* cada elemento de la secuencia al elemento dividido por 2 y *recopilarlo* en la nueva lista:

    ~~~java
    List<Double> famousNumbers = numbers.stream()
            .map(number -> number / 2)
            .collect(Collectors.toList());

    // Lista resultante: [3.14, 2.71, 42.0, 13.0]
    ~~~

2. **map** se usa a menudo para obtener un stream de propiedades de un stream de objetos. Por ejemplo, dada una clase **Job**.

    ~~~java
    public class Job {
        private String title;
        private String description;
        private double salary;

        // getters and setters
    }
    ~~~

    Podemos obtener la lista de titulos de trabajo de una lista dada de trabajos usando el metodo **map**:

    ~~~java
    List<String> titles = jobs.stream()
            .map(Job::getTitle)
            .collect(Collectors.toList());
    ~~~

3. Otro caso de uso comun es obtener una lista de algunos objetos de la lista de otros objetos. Supongamos que tenemos las siguientes clases.

    ~~~java
    class User {
        private long id;
        private String firstName;
        private String lastName;
    }

    class Account {
        private long id;
        private boolean isLocked;
        private User owner;
    }

    class AccountInfo {
        private long id;
        private String ownerFullName;
    }
    ~~~

    Y nos gustaria obtener una lista de objetos **AccountInfo** de una lista de objetos **Account**. Podemos hacerlo usando el metodo **map**:

    ~~~java
    List<AccountInfo> infoList = accounts.stream()
            .map(acc -> {
                    AccountInfo info = new AccountInfo();
                    info.setId(acc.getId());
                    String ownerFirstName = acc.getOwner().getFirstName();
                    String ownerLastName = acc.getOwner().getLastName();
                    info.setOwnerFullName(ownerFirstName + " " + ownerLastName);
                    return info;
            }).collect(Collectors.toList());
    ~~~

    Este codigo *mapeara** cada **Account**, lo convertira un un objeto **AccountInfo** y lo recopilara en una lista.

## Tipos primitivos-especializados de la operacion Map

Stream especializadas primitivas como **IntStream**, **LongStream**, o **DoubleStream** tambien tiene el mismo metodo **map** que asigna el valor primitivo a otro primitivo del mismo tipo. Sin embargo, es util tener una forma de asignar un valor primitivo a un objeto. Para eso, las corrientes primitivo-especializadas tiene el metodo **mapToObj**.  
Consideremos un ejemplo donde una clase **Planet** es dada:

~~~java
class Planet {

    private String name;
    private int orderFromSun;

    public Planet(int orderFromSun) {
        this.orderFromSun = orderFromSun;
    }
}
~~~

Ahora podemos mapear elementos **int** del flujo **IntStream** usando **mapToObj** y recopilar el stream resultante en la lista de objetos **Planet**:

~~~java
List<Planet> planets = IntStream.of(1, 2, 3, 4, 5, 6, 7, 8)
        .mapToObj(Planet::new)
        .collect(Collectors.toList());
~~~

## La operacion flatMap

La operación de mapa funciona muy bien para flujos de primitivos y objetos, pero la entrada también puede ser un flujo de colecciones. Por ejemplo, el método **stream()** de un **List<List<String>>** devuelve un **Stream<List<String>>**. En ese caso, a menudo necesitamos aplanar un flujo de colecciones a un flujo de elementos de estas colecciones.  
En tales casos, un método **flatMap** puede ser útil. Toma y aplica una función de un argumento para transformar cada elemento de flujo en un nuevo flujo y concatenar estos flujos juntos.  
Consideremos un ejemplo con libros de Java. Cada libro tiene un título, un año de publicación y una lista de autores:

~~~java
List<Book> javaBooks = List.of(
        new Book("Java EE 7 Essentials", 2013, List.of("Arun Gupta")),
        new Book("Algorithms", 2011, List.of("Robert Sedgewick", "Kevin Wayne")),
        new Book("Clean code", 2014, List.of("Robert Martin"))
);
~~~

Ahora podemos obtener una lista de todos los autores de la lista de libros de Java usando **flatMap**:

~~~java
List<String> authors = javaBooks.stream()
        .flatMap(book -> book.getAuthors().stream())
        .collect(Collectors.toList());

// Lista resultante
["Arun Gupta", "Robert Sedgewick", "Kevin Wayne", "Robert Martin"]
~~~

---

## Ejercicios

1. Encuentra el maximo valor absoluto de un array de numeros.

    ~~~java
    public class Main {

        public static int maxAbsValue(String[] numbers) {

            return Arrays.stream(numbers)
                    .mapToInt(Integer::parseInt)
                    .map(Math::abs)
                    .max()
                    .orElse(0);
        }

       public static void main(String[] args) {
            Scanner scanner = new Scanner(System.in);
            System.out.println(maxAbsValue(scanner.nextLine().split("\\s+")));
        }
    }
    ~~~

2. Escribe un metodo que retorna un array de los valores absolutos de un array ordenado en orden ascendente.

    ~~~java
    public class Main {

        public static int[] sortedAbsNumbers(String[] numbers) {

            return Arrays.stream(numbers)
                    .mapToInt(Integer::parseInt)
                    .map(Math::abs)
                    .sorted()
                    .toArray();
        }

        public static void main(String[] args) {
            Scanner scanner = new Scanner(System.in);
            System.out.println(Arrays.stream(sortedAbsNumbers(scanner.nextLine().split("\\s+")))
                    .mapToObj(String::valueOf)
                    .collect(Collectors.joining(" "))
            );
        }
    }
    ~~~

3. Cuente la cantidad de palabras unicas ignorando mayusculas y minusculas.

    ~~~java
    public class Main {

        public static long count(int n, List<List<String>> lines) {
        
            return lines.stream()
                    .flatMap(Collection::stream)
                    .map(String::toLowerCase)
                    .distinct()
                    .count();
        }

        public static void main(String[] args) {
            Scanner scanner = new Scanner(System.in);
            int n = Integer.parseInt(scanner.nextLine());

            List<List<String>> lines = Stream.generate(scanner::nextLine).limit(n)
                    .map(s -> Arrays.asList(s.split("\\s+")))
                    .collect(Collectors.toList());

            System.out.println(count(n, lines));
        }
    }
    ~~~

4. Dada una lista de paises, escriba el resultado del codigo.

    ~~~java
    List<String> countries = Arrays.asList("Costa Rica", "Hungary", "Saint Kitts and Nevis", "Norway");

    List<Integer> numbers = countries.stream()
        .map(country -> country.split("\\s+"))
        .map(country -> country.length)
        .collect(Collectors.toList());

    // resultado: 2 1 4 1
    ~~~

5. Conecte estas operaciones con las descripciones adecuadas.

    - **map** -> Transforma los elementos del stream en otros (cualquier tipo).
    - **mapToInt** -> transforma los elemntos del stream en enteros primitivos.
    - **flatMap** -> Transforma cada elemento de un stream en otro stream y concatena todos los streams en uno solo.
    - **mapToObj** -> Transforma un stream de valores primitivos en un stream generico.

6. Cual sera el resultado del codigo.

    ~~~java
    List<String> countries = Arrays.asList("Costa Rica", "Greece", "Malaysia", "Peru");

    List<Integer> numbers = countries.stream()
        .map(country -> country.split("\\s+"))
        .flatMap(Arrays::stream)
        .map(String::length)
        .collect(Collectors.toList());

    // 5 4 6 8 4
    ~~~

7. Dada una clase inmutable **Triple**, con una lista de enteros, cuales triples va a contener **set**.

    ~~~java
    final class Triple<L, M, R> {
        private final L left;
        private final M middle;
        private final R right;

        Triple(L left, M middle, R right) {
            this.left = left;
            this.middle = middle;
            this.right = right;
        }
    }

    List<Integer> numbers = Arrays.asList(5, 3, 4, 1);

    Set<Triple<Integer, Integer, String>> set = numbers.stream()
            .map(val -> new Triple<>(val, val * val, String.valueOf(val) + String.valueOf(val)))
            .collect(Collectors.toSet());

    // left=5, middle=25, right="55"
    // left=3, middle=9, right="33"
    // left=4, middle=16, right="44"
    // left=1, middle=1, right="11"
    ~~~

8. Que contendra la lista final.

    ~~~java
    List<String> strings = Arrays.asList("a", "abc", "aa", "ccbb")

    List<Integer> lengths = strings.stream()
                .map(String::length)
                .collect(Collectors.toList()),

    // [1, 3, 2 ,4]
    ~~~

9. Que elementos tendra la lista final.

    ~~~java
    List<List<Integer>> list = Arrays.asList(
                    Arrays.asList(1, 2, 3),
                    Arrays.asList(4, 5, 1),
                    Arrays.asList(8, 3, 2)
    );

    List<Integer> numbers = list.stream()
            .flatMap(Collection::stream)
            .collect(Collectors.toList());

    // [1, 2, 3, 4, 5, 1, 8, 3, 4]
    ~~~

10. Que elementos contendra la lista final.

    ~~~java
    List<Integer> numbers = Stream.of(3, 2, 4, 1).collect(Collectors.toList());

    List<Integer> generated = numbers.stream()
        .flatMap(elem -> Stream.concat(Stream.of(elem), Stream.of(elem)))
        .collect(Collectors.toList());

    // [3, 3, 2, 2, 4, 4, 1, 1]
    ~~~

11. Cual sera la cuenta final.

    ~~~java
    List<String> fruits = Arrays.asList("apple", "orange", "banana");

    long count = fruits.stream()
            .map(word -> word.substring(0, 1))
            .count();

    // 3
    ~~~

12. Conecte los codigos con las listas generadas.

    ~~~java
    List<Integer> numbers = Stream.of(1, 2, 3, 4, 5).collect(Collectors.toList());

    List<Integer> generated = numbers.stream()
            .flatMap(n -> Stream.generate(() -> n).limit(n))
            .collect(Collectors.toList());

    // [1, 2, 2, 3, 3, 3, 4, 4, 4, 4, 5, 5, 5, 5, 5]

    List<Integer> generated = numbers.stream()
            .flatMapToInt(n -> IntStream.rangeClosed(1, n))
            .boxed()
            .collect(Collectors.toList());

    // [1, 1, 2, 1, 2, 3, 1, 2, 3, 4, 1, 2, 3, 4, 5]

    List<Integer> generated = numbers.stream()
            .flatMapToInt(n -> IntStream.iterate(n, val -> val + 1).limit(n))
            .boxed()
            .collect(Collectors.toList());

    // [1, 2, 3, 3, 4, 5, 4, 5, 6, 7, 5, 6, 7, 8, 9]

    List<Integer> generated = numbers.stream()
            .flatMap(Stream::of)
            .collect(Collectors.toList());

    // [1, 2, 3, 4, 5]
    ~~~
