# Streams de Primitivos

La clase generica **Stream<T>** se utiliza para procesar objetos que siempre están representados por tipos de referencia. Por ejemplo, para trabajar con números enteros, es posible crear **Stream<Integer>** que envuelve un primitivo **int** que está en la clase **Integer**. Pero esta no es una forma eficiente de trabajar con números enteros, ya que necesita objetos de envoltura adicionales. Afortunadamente, hay tres tipos primitivos especializados llamados **IntStream**, **LongStream**, y **DoubleStream** que puede procesar efectivamente valores primitivos sin encajonamiento adicional.

## Creando streams de tipos primitivos

Hay muchas formas de crear flujos de tipos primitivos. Algunas de las formas son adecuadas para todas las transmisiones, mientras otras no.

- Pasando elementos en el metodo **of**.

    ~~~java
    IntStream inst = IntStream.of(1, 2, 3);
    LongStream longs = LongStream.of(1, 2, 3);
    DoubleStream doubles = DoubleStream.of(12.2, 18.1);
    ~~~

    Est se parace bastante a las colecciones. Tambien es posible crear un flojo vacion invocando **IntStream.of()** o **IntStream.empty()**.

- A partir de una matriz de primitivos.

    ~~~java
    IntStream numbers = Arrays.stream(new int[]{1, 2, 3});
    ~~~

    De esta manera funciona para todos los tipos de streams especializados primitivos. Tambien es posible especificar **start** (incluido) y en **end** posiciones (exclusivo) para crear un stream a partir de un subarreglo.

- Para **IntStream** y **LongStream** es posible invocar **range()** y **rangeClosed()** para crear secuencias a partir de rangos.

    ~~~java
    IntStream numbers = IntStream.range(10, 15); // desde 10 (incl) a 15 (excl)
    LongStream longs = LongStream.rangeClosed(1_000_000, 2_000_000); // incluye ambos bordes
    ~~~

    La diferencia es el metodo **rangeClosed()** incluye el limite superior mientra que **range** no.

- Consiguiendo **IntStream** de un String.

    ~~~java
    IntStream stream = "aibohphobia".chars(); // returna un IntStream!
    ~~~

    Solo funciona para **IntStream** ya que los caracteres se pueden representar con **int*.  

Otras formas de crear streams de tipo primitivo son las mismas que para los streams genericos: **generate**, **iterate** y **concat**. Aqui hay un ejemplo de generacion de un **DoubleStream** con diez numeros aleatorios e imprimiendolo.

~~~java
DoubleStream.generate(Math::random)
        .limit(10)
        .forEach(System.out::println);
~~~

## Operaciones adicionales

Los streams primitivos tienen todos los mismos metodos que los streams genericos, pero sus metodos aceptan funciones especializadas primitivas como argumentos. Por ejemplo, el metodo **forEach** de **IntStream** toma **IntConsumer**, pero no **Consumer<Integer>**. Afortunadamente, esto no afecta las posibilidades de los streams.  
Hay algunas operaciones de agregacion adicionales, como **min**, **max**, **average** y **sum**. Las tres primeras retornan un objeto opcional que representa un resultado o nada, ya que el stream inicial puede estar vacio.  
Tenga en cuenta, en realidad **Stream<T>** tambien provee **min** y **max** pero sus metodos necesitan un comparador como argumento. El siguiente codigo demuesta los metodos.

~~~java
int[] numbers = { 10, 11, 25, 14, 22, 21, 18 };

int max = IntStream.of(numbers).max().getAsInt();
System.out.println(max); // 25

int min = IntStream.of(numbers).min().getAsInt();
System.out.println(min); // 10

double avg = IntStream.of(numbers).average().orElse(0.0);
System.out.println(avg); // 17.2857...

int sum = IntStream.of(numbers).sum();
System.out.println(sum); // 121
~~~

Tambien es posible calcular estos agregados a la vez usando una sola invocacion del metodo **summaryStatistics**.

~~~java
IntSummaryStatistics stat = IntStream.rangeClosed(1, 55_555).summaryStatistics();

System.out.println(String.format("Count: %d, Min: %d, Max: %d, Avg: %.1f",
        stat.getCount(), stat.getMin(), stat.getMax(), stat.getAverage()));

// resultado
Count: 55555, Min: 1, Max: 55555, Avg: 27778.0
~~~

## Transformando Streams

Puede realizar varias transformacion de flujos de tipo primitivo.

- *Transformar un stream de un tipo a otro* usando **asDoubleStream()** para **IntStream** y **LongStream**, **asLongStream()** solo para un **IntStream**.

    Aqui hay un ejemplo que convierte una secuencia de enteros en una secuencia de dobles.

    ~~~java
    IntStream.of(1, 2, 3, 4)
            .adDoubleStream()
            .forEach(System.out::println); // imprime doubles 1.0, 2.0, ...
    ~~~
- *Transformar un stream de tipo primitivo en el stream generalizado* usando el metodo **boxed()** (es decir **IntStream** -> **Stream<Integer>**). Todos los streams de tipo primitivo tienen esto.

    ~~~java
    Stream<Integer> streamOfNumbers = IntStream.range(1, 10).boxed();

- *Transformar un stream generalizado en un stream de primitivos* se puede hacer invocando uno de los metodo **mapToInt()**, **mapToLong()** o **mapToDouble()** con la expresion lambda **i -> i** como argumento.

    ~~~java
    List<Integer> numbers = List.of(1, 5, 9);
    int sum = numbers.stream().mapToInt(i -> i).sum(); // 15
    ~~~

---

## Ejercicios

1. Seleccione las lineas que producen streams primitivos.

    - **IntStream.of(1, 2, 3);**
    - **Stream.of(1, 2, 3).mapToLong(n -> n);**

2. Que numeros imprimira el codigo.

    ~~~java
    IntStream.rangeClosed(100, 103).forEach(System.out::println);

    // 100, 101, 102, 103
    ~~~

3. De que tipo sera el siguiente stream.

    ~~~java
    var stream = "Hello, World!".chars();

    // tipo IntStream
    ~~~

4. Implemente un metodo que calcule el salario promedio de una lista de salarios.

    ~~~java
    class CalculateAverageSalary {

        private static double calcAverageSalary(Collection<Integer> salaries) {

            return salaries.stream()
                    .mapToDouble(digit -> digit)
                    .average()
                    .getAsDouble();
        }

        public static void main(String[] args) {
            Scanner scanner = new Scanner(System.in);

            List<Integer> salaries = scanner.tokens()
                    .map(Integer::parseInt)
                    .collect(Collectors.toList());

            System.out.println(calcAverageSalary(salaries));
        }
    }
    ~~~

5. Selecciona la manera correcta de crear un **Stream<Integer>** que represente el rango desde el inicio hasta el final incluyendo los bordes.

    - **IntStream.rangeClosed(start, end).boxed();**

6. Que imprimira el codigo.

    ~~~java
    boolean result = LongStream
            .rangeClosed(1, 100_000)
            .anyMathc(val -> val == 100_000);

    System.out.println(result); // true
    ~~~

7. Debe retornar un **LongStream** que consista en todos los numeros desde **-n** a **n** inclusivo, pero saltando el valor **0**.

    ~~~java
    class StreamOfPrimitives {

        public static LongStream getLongStream(int n) {
            return LongStream.rangeClosed(-n, n).filter(num -> num != 0);
        }

        public static void main(String[] args) {
            Scanner scanner = new Scanner(System.in);

            int n = scanner.nextInt();

            String result = getLongStream(n).mapToObj(e -> e)
                    .map(Object::toString)
                    .collect(Collectors.joining(" "));

            System.out.println(result);
        }
    }
    ~~~

8. Suponga que tiene un **IntStream** vacio, que valores de **IntSummaryStatistics** seran iguales a cero?

    ~~~java
    IntStream.empty().summaryStatistics().toString().lines().forEach(System.out::println);

    // IntSummaryStatistics{count=0, sum=0, min=2147483647, average=0.000000, max=-2147483648}
    ~~~
