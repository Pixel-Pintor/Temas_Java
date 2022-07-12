# Stream Filtering

El filtrado es una operación importante que nos permite obtener solo aquellos elementos de la colección que cumplen una condición específica. Por ejemplo, para conseguir canciones de más de siete minutos entre toda tu biblioteca musical, podemos filtrar las canciones por su duración. En este tema, descubriremos cómo usar la operación de filtro intermedio de Java Stream API para hacer frente a tales desafíos.

## El metodo Filter

Para filtrar elementos, los streams proporcionan el metodo **filter**. Devuelve un stream que consta solo de aquellos elementos que coinciden con predicado dado.  
Como ejemplo, aqui hay una lista de numeros primos (un numero primo es un numero entero mayor que 1 cuyos unicos factores son 1 y el mismo):

~~~java
List<Integer> primeNumbers = Arrays.asList(2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31);
~~~

Nos gustaria crear una nueva lista que consista en numeros primos que pertenecen al rango de 11 a 23 (inclusive):

~~~java
List<Integer> filteredPrimeNumbers = primeNumbers.stream() // crea un stream de la lista
        .filter(n -> n >= 11 && n <= 23) // filtra los elementos
        .collect(Collectors.toList());   // colecciona los elementos en una nueva lista 

// Lista resultante
[11, 13, 17, 19, 23]
~~~

Desde el metodo **filter** toma un predicado, es posible instanciar un objeto directamente y pasarlo al metodo.

~~~java
Predicate<Integer> between11and23 = n -> n >= 11 && n <= 23; // intancia del predicado

List<Integer> filteredPrimeNumbers = primeNumbers.stream() // crea un stream de la lista
        .filter(between11and23)        // pasa el predicado al filter
        .collect(Collectors.toList()); // recolecta los elementos en una nueva lista

// Lista resultante
[11, 13, 17, 19, 23]
~~~

## Uso de varios Filter

A veces, dos o mas filtros se utilizan juntos. Por ejemplo:

- Separar una logica compleja de un solo filtro.
- Para filtrar el flujo, luego procesarlo por otros metodos y luego filtrar nuevamente.

Consideremos un ejemplo. Dada una lista de lenguajes de programacion con cadenas vacias.

~~~java
List<String> programmingLanguages = Arrays.asList("Java", "", "scala", "Kotlin", "", "clojure");
~~~

Nos gustaria contar cuantos lenguages de programacion comienzan con una letra mayuscula ignorando todas las cadenas vacias.

~~~java
long count = programmingLanguages.stream()
        .filter(lang -> lang.length() > 0)
        .filter(lang -> Character.isUpperCase(lang.charAt(0)))
        .count();

// la cuenta es 2 ("Java", "Kotlin")
~~~

Estas dos operaciones de filtro se pueden reemplazar con una sola operacion que toma un predicado complejo.

~~~java
filter(lang -> lang.length() > 0 && Character.isUpperCase(lang.charAt(0)))
~~~

---

## Ejercicios

1. Tienes dos **IntStream** el primero tiene numeros pares y el segundo tiene impares, implemente un metodo que retorne un tercer stream que contiene los numeros que son divisibles por 3 y 5, sin los dos primeros numeros.

    ~~~java
    class EvenAndOddFilter {

        public static IntStream createFilteringStream(IntStream evenStream, IntStream oddStream) {
            IntPredicate predicate = n -> n % 3 == 0 && n % 5 == 0;
            return IntStream.concat(evenStream, oddStream)
                    .filter(predicate)
                    .sorted()
                    .skip(2);
        }

        public static void main(String[] args) {

            Scanner scanner = new Scanner(System.in);

            String[] values = scanner.nextLine().split(" ");

            List<Integer> numbers = Arrays.stream(values)
                    .map(Integer::parseInt)
                    .collect(Collectors.toList());

            int[] evenNumbers = numbers.stream()
                    .filter(n -> n % 2 == 0)
                    .mapToInt(x -> x)
                    .toArray();

            int[] oddNumbers = numbers.stream()
                    .filter(n -> n % 2 == 1)
                    .mapToInt(x -> x)
                    .toArray();

            IntStream filteringStream = createFilteringStream(
                    Arrays.stream(evenNumbers),
                    Arrays.stream(oddNumbers));

            System.out.println(filteringStream.boxed().collect(Collectors.toList()));
        }
    }
    ~~~

2. Implemente un metodo que verifique si un numero es primo.

	~~~java
 	class PrimeNumbers {

        private static boolean isPrime(long number) {

            return LongStream.rangeCloses(1, number)
                            .filter(n -> number % n == 0)
                            .allMatch(n -> n == 1 || n == number);
        }

		public static void main(String[] args) {

			Scanner scanner = new Scanner(System.in);
			String line = scanner.nextLine().trim();
			int n = Integer.parseInt();
			System.out.println(isPrime(n) ? "True" : "False");
		}
    }
	~~~

3. Implemente un metodo que retorne un stream preparado para detectar malas palabras. El metodo debe devolver un stream de todas las malas palabras unicas presentes en el texto, en orden lexicografico (como en un diccionario).

	~~~java
	class BadWordsDetector {

    	private static Stream<String> createBadWordsDetectingStream(String text,                                         List<String> badWords) {

        	String[] textWords = text.split(" ");
        	return Arrays.stream(textWords)
            	    .filter(badWords::contains)
                	.distinct()
                	.sorted();
    	}

    	public static void main(String[] args) {
        	Scanner scanner = new Scanner(System.in);
        	String[] parts = scanner.nextLine().split(";");

        	String text = parts[0];

        	List<String> dict = parts.length > 1 ?
            	    Arrays.asList(parts[1].split(" ")) :
                	Collections.singletonList("");

        	System.out.println(createBadWordsDetectingStream(text, dict).collect(Collectors.toList()));
    	}
	} 
	~~~

4. Implemente un metodo que toma una lista de cadenas y retorna una secuencia que consta de los elementos de una lista dada que tienen menos de cuatro caracteres.

	~~~java
	public class Main {

    	private static Stream<String> omitLongStrings(List<String> strings) {

        	return strings.stream()
            	    .filter(str -> str.length() < 4);
    	}

    	public static void main(String[] args) throws IOException {
        	BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));
        	String str = reader.readLine();
        	List<String> list = new ArrayList<>(Arrays.asList(str.split(" ")));
        	omitLongStrings(list).forEach(System.out::println);
    	}
	}
	~~~

5. Dada una secuencia de numeros enteros llamada **intStream**, se obtiene otro stream aplicando un filtro, como se pueden combinar estas operaciones para obtener el mismo resultado.

	~~~java
	IntStream resultStream = intStream.filter(n -> n > 10).filter(n -> n < 20).filter(n -> n != 15);

	// Respuesta
	intStream.filter(n -> n > 10 && n < 20 && n != 15);
	~~~

6. Cual sera el resultado del codigo.

	~~~java
	List<String> famousProgrammers = Arrays.asList(
		"Linus Torvalds",
		"John Carmack",
		"James Gosling",
		"Joshua Bloch"
		"Ken Thopson"
	);

	long result = famousProgrammers.stream()
			.filter(name -> name.startsWith("J"))
			.count();
	
	// result: 3
	~~~

7. Cual es el resultado de stream.

	~~~java
	List<String> names = List.of("Patrick Ross", "Kelly Wood", "James Moore", "Janice Coleman", "Mary Carter");

	names.stream()
			.filter(name -> name.length() < 12)
			.filter(name -> name.startsWith("J"))
			.count(); // 1
	~~~

8. Cuales elementos contiene la variable **filtered**.

	~~~java
	List<Integer> numbers = Arrays.asList(4, 5, 3, 8, 6, 2, 7);

	List<Integer> filtered = numbers.stream()
    	    .filter(n -> n * n < 36)
        	.collect(Collectors.toList());

	// filtered: 4, 5, 3, 2
	~~~
