# Procesamiento funcional de datos con Streams

Ya sabe cómo procesar secuencias de objetos utilizando bucles y colecciones. Sin embargo, este enfoque es bastante propenso a errores debido a las variables mutables y la lógica de bucle compleja. También puede hacer que su código sea menos legible, lo que genera complicaciones con su desarrollo posterior.  
Java 8 nos dio una nueva herramienta llamada **Stream API** que brinda un *enfoque funcional* para procesar colecciones de objetos. Al usar Stream API, un programador no necesita escribir bucles explícitos, ya que cada flujo tiene un bucle optimizado interno. Las secuencias nos permiten centrarnos en la pregunta " ¿qué debe hacer el código?" en lugar de " ¿cómo debería hacerlo el código?" . Además, este enfoque facilita la paralelización.  

## El concepto basico de Streams

En cierto sentido, un **stream** puede recordarnos una coleccion. Pero en realidad no almacena elementos. En su lugar, transmite elemento de una **fuente**, como una coleccion, una funcion de generador, un archivo, un canal I/O, otra secuencia o algo mas, y luego procesa los elementos mediante el uso de una secuenciacion de operaciones predefinidas combinadas en una sola canalizacion.  
Hay tres etapas en el trabajo con un Stream:

1. Obtener el flujo de la fuente.
2. Realizacion de operaciones intermedias con el flujo para procesar datos.
3. Realizar una operacion terminal para producir un resultado.

## Ejemplo de un bucle frente a un flujo

Todas las clases asociadas con los *streams* se encuentran en el paquete **java.util.stream**. Hay varias clases de flujo comunes: **Stream<T>**, **IntStream**, **LongStream** y **DoubleStream**. Mientras que el flujo genérico funciona con tipos de referencia, otros funcionan con los tipos primitivos correspondientes. En este tema, solo consideraremos la transmisión genérica.  
Consideremos un ejemplo simplre. Supongamos que tenemos una lista de numeros y nos gustaria contar los numeros que son mayores que 5:

~~~java
List<Integer> numbers = List.of(1, 4, 7, 6, 2, 9, 7, 8);
~~~

Una forma "tradicional2 de hacerlo es escribir un bucle como el siguiente.

~~~java
long count = 0;
for (int number : numbers) {
    if (number > 5) {
        count++;
    }
}
System.out.println(count), // 5
~~~

Este odigo impriem "5" porque la lista inicial contiene solo cinco numeros mayores que 5.  
Un bucle con una condicion de filtrado es una construccion de uso comun en la programacion. Es posible simplificar este codigo reescribiendolo usando un **stream**.

~~~java
long count = numbers.stream()
        .filter(number -> number > 5)
        .count(); // 5
~~~

Aquí obtenemos un **stream** de la lista **number**, luego filtre sus elementos usando una expresión lambda de predicado y luego cuente los números que satisfacen la condición. Aunque este código produce el mismo resultado, es más fácil de leer y modificar. Por ejemplo, podemos cambiarlo fácilmente para omitir los primeros cuatro números de la lista.

~~~java
long count = numbers.stream()
        .skip(4)  // skip 1, 4, 7, 6
        .filter(number -> number > 5)
        .count();  // 3
~~~

El procesamiento de un flujo se realiza como una cadena de llamadas a métodos separadas por puntos con una sola operación de terminal. Para mejorar la legibilidad, se recomienda colocar cada llamada en una nueva línea si la transmisión contiene más de una operación.

## Creando Streams

Hay muchas formas de crear una transmisión, incluido el uso de una lista, un conjunto, una cadena, una matriz, etc. como fuente.

1. La forma más común de crear un stream es tomarla de una colección. Cualquier colección tiene el metodo **stream()** para este propósito.

    ~~~java
    List<Integer> famousNumbers = List.of(0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55);
    Stream<Integer> numbersStream = famousNumbers.stream();

    Set<String> usefulConcepts = Set.of("functions", "lazy", "immutability");
    Stream<String> conceptsStream = usefulConcepts.stream();
    ~~~

2. Tambien es posible obtener un stream de una matriz.

    ~~~java
    Stream<Double> doubleStream = Arrays.stream(new Double[]{ 1.01, 1d, 0.99, 1.02, 1d, 0.99 });
    ~~~

3. O directamente de algunos valores.

    ~~~java
    Stream<String> persons = Stream.of("John", "Demetra", "Cleopatra");
    ~~~

4. O concatenar otros streams juntos.

    ~~~java
    Stream<String> stream1 = Stream.of(/* some values */);
    Stream<String> stream2 = Stream.of(/* some values */);
    Stream<String> resultStream = Stream.concat(stream1, stream2);
    ~~~

5. Hay algunas posibilidades de crear streams vacios (que se pueden usar como valores de retorno de los metodos).

    ~~~java
    Stream<Integer> empty1 = Stream.of();
    Stream<Integer> empty2 = Stream.empty();
    ~~~

También existen otros métodos para crear secuencias desde diferentes fuentes: desde un archivo, desde una secuencia de E/S, etc.

## Grupos de operacion de Stream

Todas las operaciones con streams se dividen en dos grupos: **intermedias** y **terminales**.

- **Las operaciones intermedias** no se evaluan inmediatamente al invocarlas. Simplemente devuelven nuevos flujos para llamar a las proximas operaciones en ellos. Estas operaciones se conocen como **lazy** porque en realidad no hacen nada util.
- **Las operaciones de terminal** comienzan todas las evaluaciones con el stream para producir un resultado o un efecto secundario. Como mencionamos antes, una secuencia siempre tiene una sola operacion terminal.

Una vez que se ha evaluado una operacion de terminal, es imposible reutilizar la secuencia nuevamente. Si intentas hacer eso, el programa lanzara **IllegalStateException**.  
Los flujos proporcionan una gran cantidad de operaciones para realizar en los elementos. La única forma de aprenderlos todos es con buenas prácticas. Por eso te damos solo una pequeña lista de operaciones para cada grupo. Otras operaciones se consideran en los siguientes temas.

### Operaciones intermedias

- **filter** devuelve una nueva secuencia que incluye los elementos que coinciden con un **predicado**.
- **limit** retorna una nueva secuencia que consta de los primeros **n** elemento de este stream.
- **skip** retorna un nuevo stream sin los primeros **n** elementos.
- **distinct** retornar una nueva secuencia que consta solo de los elementos unicos de acuerdo con el resultado de **equals**.
- **sorted** retornar una nueva secuencia que incluye elementos clasificados segun el orden natural o una **comparator**.
- **peek** retorna el mismo stream de elementos pero permite observar los elementos actuales del stream para la depuracion.
- **map** retorna un nuevo stream que consta de los elementos que se obtuvieron aplicando una funcion (es decir, transformando cada elemento).

### Operaciones terminales

- **count** retorna el numero de elementos en la secuencia como un valor **long**.
- **max/min** retorna un elemento **Optional** del stream, segun el comparador dado.
- **reduce** combina valores de la secuencia en un solo valor (un valor agregado).
- **findFirst/findAny** retorna el primer/cualquier elemento de la secuencia como un **Optional**.
- **anyMatch** retorna **true** si al menos un elemento coincide con un predicado (ver tambien: **allMatch**, **noneMatch**).
- **forEach** toma un **consumer** y lo aplica a cada elemento del stream (por ejemplo, imprimiendolo).
- **collect** retorna una coleccion de valores en la secuencia.
- **toArray** retorna una matriz de los valores en una secuencia.

Tales operaciones (metodos) como **filer**, **map**, **reduce**, **forEach**, **anyMatch** y algunas otras se llaman *funciones de orden superior* porque aceptan otras funciones como argumentos.

## Un ejemplo

Como ejemplo, usemos operaciones de stream para imprimir, todos los nombres de empresas sin duplicados, en mayusculas.

~~~java
List<String> companies = List.of(
        "Google", "Amazon", "Samsung",
        "GOOGLE", "amazon", "Oracle"
);

companies.stream()
        .map(String::toUpperCase) // transforma cada nombre a mayuscula
        .distinct() // operacion intermedia: mantiene solo las palabras unicas
        .forEach(System.out::println); // imprime cada compañia
~~~

Aqui usamos dos operaciones intermedias (**map** y **distinct**) y una operacion terminal **forEach**. El codigo imprime solo nombres de empresas unicos en mayuscula.

~~~java
GOOGLE
AMAZON
SAMSUNG
ORACLE
~~~

Usando referencias de métodos (como **String::toUpperCase** o **System.out::println**) hace que su código basado en secuencias sea aún más legible que usar expresiones lambda. Se recomienda usar de esta manera o expresiones lambda pequeñas de una sola línea en lugar de expresiones lambda complejas de cuerpo largo.

---

## Ejercicios

1. Seleccion todas las declaraciones correctas sobre las **operaciones de Stream**.

    - Algunas operaciones retornan Streams
    - Solo operaciones terminales inician evaluaciones
    - Un stream permite usar cualquier numero de operaciones intermedias

2. Como crear un stream desde una lista llamada **ls**.

    - **ls.stream()**

3. Implemente un metodo que retorne el numero de contraseñas de un stream.

    ~~~java
    public class Main {


        public static long countPasswords(Stream<String> passwordStream) {
            return passwordStream.count();
        }

        public static void main(String[] args) {
            Scanner scanner = new Scanner(System.in);
            System.out.println(countPasswords(Arrays.stream(scanner.nextLine().split("\\s+"))));
        }
    }
    ~~~

4. Implemente un metodo que imprima cada elemento de un stream, excepto por los dos primeros elementos.

    ~~~java
    public class Main {
        
        public static void printStream(Stream<Integer> stream) {
            stream.skip(2).forEach(System::out.println);
        }

        public static void main(String[] args) {
            Scanner scanner = new Scanner(System.in);
            printStream(Arrays.stream(scanner.nextLine().split("\\s+")).map(Integer::parseInt));
        }
    }
    ~~~

5. Seleccione las declaraciones correctas sobre **streams** en Java 8+.

    - Un stream puede ser creado desde una lista o un conjunto
    - La libreria Java tiene streams especializados, genericos y primitivos.

6. Que imprimira el codigo.

    ~~~java
    boolean result = !IntStream
            .generate(() -> 100)
            .limit(101)
            .allMatch(val -> val >= 100);

    System.out.println(result); // false
    ~~~

7. Implemente un metodo que imprima los elementos organizados de un stream se String.

    ~~~java
    public class Main {

    
        public static void sortAndPrint(Stream<String> wordStream) {
            wordStream.sorted()
                    .forEach(System.out::println);
        }

        public static void main(String[] args) {
            Scanner scanner = new Scanner(System.in);
            sortAndPrint(Arrays.stream(scanner.nextLine().split("\\s+")));
        }
    }
    ~~~

8. Implemente un metodo para encontrar el elemento maximo y minimo de un stream de acuerdo con un comparador.

    ~~~java
    class MinMax {

        public static <T> void findMinMax(
                Stream<? extends T> stream,
                Comparator<? super T> order,
                BiConsumer<? super T, ? super T> minMaxConsumer) {

            List<T> list = stream.sorted(order)
                    .collect(Collectors.toList());

            if (list.size() == 0) {
                minMaxConsumer.accept(null, null);
            } else {
                minMaxConsumer.accept(list.get(0), list.get(list.size() - 1));
            }
        }
    }
    ~~~
