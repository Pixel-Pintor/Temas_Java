# Collectors

Hasta ahora sabemos cómo producir un valor único a partir de un flujo de elementos usando **reduce**. Sin embargo, recopilar elementos de stream en una colección como una **List** o un **Set** es un escenario mucho más popular que reducirlos a un solo valor. Para eso, Java Stream API proporciona una operación de terminal llamada **collect**. En combinación con una utilidad **stream.Collectors** clase que contiene muchas operaciones de reducción útiles, **collect** nos permite producir fácilmente colecciones a partir de streams, así como valores únicos como las operaciones **reduce** lo hacen.

## Produciendo colecciones

**collect** es una operación de reducción terminal que puede aceptar un objeto de la clase **Collector**. Pero en lugar de centrarse en el **Collector**, consideremos la clase **Collector** más de cerca. Es importante saber que la clase **Collector** contiene métodos estáticos que devuelven el **Collector** e implementar la funcionalidad para acumular elementos de stream en una colección, resumirlos, volver a empaquetarlos en una sola cadena, etc.  
Tenga en cuenta que el **collect** es una operación terminal, lo que significa que comienza todas las evaluaciones con la secuencia y produce un resultado final.  
Para ser mas concretos, consideremos un ejemplo donde se da la clase **Account**:

~~~java
public class Account {
    private long balance;
    private String number;

    // getters and setters
}
~~~

Queremos producir una lista de cuentas del stream de cuentas **Stream<Account> accountStream**. Para hacerlo, podemos acumular elementos del stream usando el metodo **Collectors.toList**.

~~~java
List<Account> accounts = accountStream.collect(Collectors.toList());
~~~

Como puedes ver, el metodo **Collectors.toList** hizo todo el trabajo por nosotros. De manera similar a producir un **List** de un stream, podemos producir un **Set**. Nuevamente, podemos delegar esa responsabilidad a la clase **Collector** y el uso del metodo **Collectors.toSet**.

~~~java
Set<Account> accounts = accountStream.collect(Collectors.toSet());
~~~

Si necesita mas control sobre la produccion de colecciones y desea acumular elementos de stream en una coleccion particular que no es un **List** o un **Set**, puede usar el metodo **Collectors.toCollection**.

~~~java
LinkedList<Account> accounts = accountStream.collect(Collectors.toCollection(LinkedList::new));
~~~

El metodo **Collectors.toCollection** acepta una funcion que genera una nueva coleccion de un tipo especificado. En el ejemplo anterior, hemos acumulado un stream de numeros de cuenta en el **LinkedList<Account>** proporcionando una referencia a su constructor predeterminado.

## Produciendo valores

De manera similar a la operación **reduce**, **collect** es capaz de acumular elementos de stream en un solo valor. Aquí puedes ver algunos métodos **Collectors** que producen un solo valor:

- **summingInt**, **summingLong**, **summingDouble**
- **averagingInt**, **averagingLong**, **averagingDouble**
- **maxBy**, **minBy**
- **counting**

Los nombres de los metodos se explican por si mismo con respecto a su proposito.  
Ahora vamos a resumir los saldos de las cuentas. Nosotros podemos usar el metodo **summingLong** para eso.

~~~java
long summary = accounts.stream()
        .collect(summingLong(Account::getBalance));
~~~

Ademas, podemos calcular el valor medio.

~~~java
double average = accounts.stream()
        .collect(averagingLong(Account::getBalance));
~~~

Si necesita realizar cálculos más específicos, puede utilizar **Collectors.reducing**. De manera similar a **reduce**, las implementaciones de métodos **Collectors.reducing** pueden aceptar una función de acumulador o el valor de identidad junto con un acumulador. Sin embargo, hay una implementación adicional que acepta la identidad, el *mapeador* y una función de acumulación.  
Cabe destacar que el mapeador es una función de mapeo que se aplica a los elementos del stream, mientras que la función del acumulador reductor reduce los valores mapeados de un stream. Consideremos un ejemplo.

~~~java
String megaNumber = accountStream.collect(Collectors.reducing("",
        account -> account.getNumber(),
        (numbers, number) -> numbers.concat(number)
));
~~~

El codigo anterior asigna cada cuenta a su numero y concatena todos los numeros de cuenta en un solo numero usando un recolector reductor.

---

## Ejercicios

1. El metodo **collect** puede aceptar un objeto del siguiente tipo.

    - **Collector**

2. Escribe un colector que evalue el producto de cuadrados de numeros enteros en un **Stream<Integer>**.

    ~~~java
    class CollectorProduct {

        public static void main(String[] args) {

            Scanner scanner = new Scanner(System.in);

            String[] values = scanner.nextLine().split(" ");

            List<Integer> numbers = new ArrayList<>();
            for (String val : values) {
                numbers.add(Integer.parseInt(val));
            }

            long val = numbers.stream().map(n -> n * n).reduce(1, (acc, n) -> acc * n);

            System.out.println(val);
        }
    }
    ~~~

3. Implemente el metodo que acepta un stream de elementos, los acumula en un **Set** y retorna el valor promedio de los numeros del conjunto.

    ~~~java
    class Main {

        public static Double avgOnSet(Stream<Integer> numbers) {
            return numbers.collect(Collectors.toSet()).stream().mapToDouble(a -> a).average().orElse(0.0);
        }

        public static void main(String[] args) {
            Scanner scanner = new Scanner(System.in);
            Stream<Integer> integerStream = Arrays.stream(scanner.nextLine()
                    .split("\\s+")).map(Integer::parseInt);
            System.out.println(avgOnSet(integerStream));
        }
    }
    ~~~

4. Escoge todos los colectores existentes.

    - **Collectors.maxBy()**
    - **Collectors.summingDouble()**
    - **Collectors.partitioningBy()**
    - **Collectors.reducing()**
    - **Collectors.groupingBy()**

5. Que clase contiene implementaciones de **Collector** que implementan varias operaciones de reduccion utiles.

    - **Collectors**

6. Seleccione las declaraciones correctas sobre la operacion **collect** y el estandar **collectors**.

    - Es posible recopilar elementos de una secuencia en una lista o una conjunto utilizando colectores estandar
    - El collect es una operacion terminal
