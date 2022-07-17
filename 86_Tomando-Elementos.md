# Tomando Elementos

Si desea tomar solo una cierta cantidad de elementos de una secuencia u omitir algunos de ellos, puede invocar los metodos **limit(n)** o **skip(m)**. Pero, ¿qué sucede si necesita tomar u omitir algunos elementos hasta que se cumpla alguna condición? A partir de Java 9, las secuencias tienen dos métodos convenientes para hacer esto. Pueden tomar o eliminar la subsecuencia contigua más larga de elementos de la secuencia en función del predicado dado.

## El metodo takeWhile

El metodo **takeWhile** toma elementos de la secuencia hasta que se encuentra el primero elemento inapropiado. Este elemento y todos los demas se descartan. El predicado que se pasa a este metodo determina que elemento es inapropiado. Si todos los elementos coinciden con el predicado, el resultado contendra los mismos elementos que la secuencia inicial.  
En ele siguiente ejemplo, creamos una secuencia de numeros y tomamos elementos mientras son mayores que cero.

~~~java
List<Integer> numbers =
        Stream.of(3, 5, 1, 2, 0, 4, 5)
                .takeWhile(n -> n > 0)
                .collect(Collectors.toList());

System.out.println(numbers); // [3, 5, 1, 2]
~~~

El metodo **takeWhile** se detiene despues de tomar el elemento 0, porque la condicion se convierte en **false**.

## El metodo dropWhile

Tambien hay un metodo opuesto llamado **dropWhile**. Omite los elementos que coinciden con el predicado dado hasta que el primer elemento no coincida. Este y todos los elementos restantes se incluyen en el resultado. Si todos los elementos coinciden con el predicado, el resultado sera una secuencia vacia.  
Aqui esta el mismo ejemplo que antes, pero con **dropWhile**.

~~~java
List<Integer> numbers =
        .Stream.of(3, 5, 1, 2, 0, 4, 5)
                .dropWhile(n -> n > 0)
                .collect(Collectors.toList());

System.out.println(numbers); // [0, 4, 5]
~~~

El metodo **dropWhile** deja de borrar elementos justo despues de tomar el elemento 0, porque la condicion se vuelve **false**, entonces los numeros 0, 4 y 5 permanecen en la lista.

## El caso de los stream desordenados

Ambos métodos **takeWhile** y **dropWhile** funcionan bien en caso de secuencias ordenadas. Dichos flujos se crean a partir de colecciones ordenadas (p. ej., listas) o se obtienen durante las operaciones (como la clasificación). Pero, ¿y si estamos tratando con una colección desordenada como un conjunto? Resulta que en este caso el comportamiento de ambas operaciones es no determinista.  
Como ejemplo, supongamos que tenemos un conjunto de conferencias de Java con una conferencia de Kotlin. Podemos tratar de seguir tomando nombres de conferencias siempre que comiencen con **"J"** y detenerse en la primera conferencia inapropiada.

~~~java
Set<String> conferences = Set.of(
        "JokerConf", "JavaZone",
        "KotlinConf", "JFokus"
);

conferences.stream()
        .takeWhile(word -> word.startsWith("J"))
        .forEach(System.out::println);
~~~

Como no sabemos el orden de los nombres en el conjunto, el resultado de este codigo siempre puede ser diferente, conteniendo de 0 a 3 nombres de conferencia. Esto definitivamente no es lo que queriamos aqui.  
Usar **takeWhile** y **dropWhile** con secuencias desordenadas conduce a un comportamiento no determinista y, como resultado, a errores.  
¿Como lograr resultados repetibles en este caso?

- Usar **List** en vez de **Set**
- Agregar el metodo **sorted()** antes de **takeWhile()** para mantener siempre el mismo orden de elementos dentro del stream.

---

## Ejercicios

1. Aplique su conocimiento de Stream API para extraer todos los codigos entre ellos excluyendo **#0000** y **#FFFF**.

    ~~~java
    public class Main {

        private static List<String> extractCodes(List<String> codes) {
            return codes.stream()
                    .dropWhile(Predicate.not("#0000"::equals))
                    .skip(1)
                    .takeWhile(Predicate.not("#FFFF"::equals))
                    .collect(Collectors.toList());
        }

        public static void main(String[] args) {
            Scanner scanner = new Scanner(System.in);

            List<String> codes = Arrays.stream(scanner. nextLine()
                    .split("\\s+"))
                    .collect(Collectors.toList());

            System.out.println(String.join(" ", extractCodes(codes)));
        }
    }
    ~~~

2. Conecte las declaraciones con el valor resultante.

    ~~~java
    public class Main {

        private static List<String> extractCodes(List<String> codes) {
            return codes.stream()
                    .dropWhile(Predicate.not("#0000"::equals))
                    .skip(1)
                    .takeWhile(Predicate.not("#FFFF"::equals))
                    .collect(Collectors.toList());
        }

        public static void main(String[] args) {
            List<River> rivers = List.of(
                    new River("Nile", 6650),
                    new River("Amazon", 6400),
                    new River("Yangtze", 6300),
                    new River("Mississippi", 6275),
                    new River("Yenisei", 5539),
                    new River("Huang He", 5464)
            );

            System.out.println(rivers.stream().takeWhile(r -> r.getLength() < 6000).count()); // 0
            System.out.println(rivers.stream().takeWhile(r -> r.getLength() > 6000).count()); // 4
            System.out.println(rivers.stream().dropWhile(r -> r.getLength() < 6000).count()); // 6
            System.out.println(rivers.stream().dropWhile(r -> r.getLength() > 6000).count()); // 2
        }
    }

    class River {
        private final String name;
        private final long length;
    
        public River(String name, long length) {
            this.name = name;
            this.length = length;
        }

        public String getName() {
            return name;
        }

        public long getLength() {
            return length;
        }
    }
    ~~~

3. Implemente un metodo que tome una coleccion de numeros, los organice y que salte los numeros que son menores que 10.

    ~~~java
    class ProcessNumbers {

        public static List<Integer> processNumbers(Collection<Integer> numbers) {
            return numbers
                    .stream()
                    .sorted(Comparator.naturalOrder())
                    .dropWhile(n -> n < 10)
                    .collect(Collectors.toList());
        }

        public static void main(String[] args) {
            Scanner scanner = new Scanner(System.in);

            Collection<Integer> numbers = Arrays.stream(scanner.nextLine().split("\\s+"))
                    .map(Integer::parseInt)
                    .collect(Collectors.toCollection(HashSet::new));

            String result = processNumbers(numbers).stream()
                    .map(String::valueOf)
                    .collect(Collectors.joining(" "));

            System.out.println(result);
        }
    }
    ~~~

4. Que secuencia de numeros imprimira el codigo.

    ~~~java
    List<Integer> fibonacci = List.of(0, 1, 1, 2, 3, 5, 8, 13);

    List<Integer> result = fibonacci.stream()
            .dropWhile(n -> n > 5)
            .collect(Collectors.toList());

    System.out.println(result);
    // [0, 1, 1, 2, 3, 5, 8, 13]
    ~~~
