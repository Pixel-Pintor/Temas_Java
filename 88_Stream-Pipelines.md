# Stream Pipelines

En este tema, ya ha aprendido diferentes tipos de operaciones de transmisión, como **filter**, **map** y **reduce**. Ahora es el momento de comenzar a usar estas operaciones juntas y obtener más detalles sobre las canalizaciones de stream real. En cierto sentido, la idea de este tema es consolidar su conocimiento sobre corrientes y guiarlo a través de ejercicios de práctica más complejos.

## Mas informacion sobre las operaciones de canalizaciones de stream

Por lo general, los streams listos para produccion contienen varias operaciones a la vez. Es posible distinguir los siguientes tipos de operaciones.

- Filtrado: usando **filter** u otros metodos para omitir algunos de los elementos como **skip**, **limit**, **takeWhile** y asi.
- Mapear o modificar elementos de stream: por ejemplo, clasificar o eliminar duplicados.
- Reduciendo o combinando: **reduce**, **max**, **min**, **collect**, **count**, **findAny** y asi.

Esto no es una coincidencia. Estos grupos de operaciones componen una canalizacion de datos estandar en muchos sistemas de informacion y los streams son adecuados para simularlos.  
Consideremos un ejempl de un stream con varias operaciones. Supongamos que hay una lista de String llamadas **words**. Nos gustaria contar el numero total de palabras que comienzan con **"JA"**. El caso no es importante: **"ja"**, **"jA"** y **"Ja"** son adecuados tambien.  
Aqui esta nuestra solucion con las operaciones **map**, **filter** y **count**.

~~~java
List<String> words = List.of("JAR", "Java", "Kotlin", "JDK", "jakarta");

long numberOfWords = words.stream()
        .map(String::toUpperCase)         // las convierte a mayuscula
        .filter(s -> s.startsWith("JA"))  // filtra usando un prefijo
        .count();                         // cuenta las palabras elegidas

System.out.println(numberOfWords); // 3
~~~

## El orden de ejecucion

Pero también hay una cosa menos obvia en el caso del ejemplo anterior: el orden de las operaciones en este stream. Parece que la operación **filter** solo se llama después de la **map** ha convertido todos los elementos a mayúsculas. Pero eso no siempre es cierto. Podemos verlo por nosotros mismos agregando la operación **peek** para imprimir los elementos intermedios del stream.  
Despues de agregar la operacion **peek** antes y despues de **filter**, el stream se vera asi.

~~~java
long numberOfWords = words.stream()
        .map(String::toUpperCase)
        .peek(System.out::println)
        .filter(s -> s.startsWith("JA"))
        .peek(System.out::println)
        .count();

// salida
JAR
JAR
JAVA
JAVA
KOTLIN
JDK
JAKARTA
JAKARTA
~~~

Esta salida en realidad significa que la operacion **filter** se aplica a un elemento justo despues de mapear el elemento.

## Stream con clases personalizadas

En situaciones reales, los streams a menudo procesan clases personalizadas diseñadas especificamente para el programa.  
Supongamos que tenemos una clase **Event** que representa un evento publico, como una conferencia, el estreno de una pelicula o un concierto. Tiene dos campos:

- **beginning** (**LocalDate**) es una fecha en la que sucede el evento.
- **name** (**String**) que es el nombre del evento (por ejemplo, **"JavaOne - 2017"**).

Ademas, la clase tiene getters y setters para cada campo con los nombres correspondientes.  
Tambien tenemos una lista de instancias nombradas **events**.  
Busquemos todos los nombres de los eventos que ocurriran del *30 al 31 de diciembre de 2017* (inclusive):

~~~java
LocalDate after = LocalDate.of(2017, 12, 29);
LocalDate before = LocalDate.of(2018, 1, 1);

List<String> suitableEvents = events.stream()
        .filter(e -> e.getBeginning().isAfter(after) && e.getBeggining().isBefore(before))
        .map(Event::getName)
        .collect(Collectors.toList());
~~~

El codigo anterior encuentra los nombres de todos los eventos adecuados y los recopila en una nueva lista de String. Los metodos de mapear nos permiten hacer la transicion de objetos **Event** a objetos **String**.

## Funciones de Mapeo y Reduccion

Dado que las funciones se presentan como objetos de ciertas clases, podemos usar **map** y **reduce** de forma similar a los streams regulares.  
Por ejemplo, tenemos una coleccion de predicados enteros. Neguemos cada predicado usando un operador de mapa y luego combinemos todos los predicados en uno usando un operador de reduccion.

~~~java
public static IntPredicate negateEachAndConjunctAll(Collection<IntPredicate> predicates) {
    return predicates.stream()
            .map(IntPredicate::negate)
            .reduce(n -> true, IntPredicate::and);
}
~~~

En este ejmplo, **map** niega cada predicado en el stream y entonces **reduce** reune todos los predicados dentro de uno. El *valor inicial* (seed) de reducir es un predicado que siempre es **true**, a causa su valor neutral para la conjuncion.  
Asi que, el predicado introducido **P1(x), P2(x), ..., Pn(x)** sera reducido dentro de un predicado **Q(x) = not P1(x) and not P2(x) ... and not Pn(x)**. Por supuesto, esta no es la manera mas frequente de aplicar streams, pero es util saber que tal uso es tambien posible.

---

## Ejercicios

1. Digamos que esta desarrollando un sistema bancario. Hay dos clases en este sistema.

    - **Transaction** con los campos **uuid** (**String**), la enumeracion **State** (**CANCELED**, **FINISHED**, **PROCESSING**), **sum** (**Long**).
    - **Account** con los campos **number** (**String**), **balance** (**Long**) y **transactions** (**List<Transaction>**)

    Ambas clases tiene getters para todos los campos con los nombres correspondientes.  
    Usando Stream API, implemente un metodo que calcule la *suma total de transacciones canceladas* para todas las cuentas no vacias (**balance > 0**). Quizas, **flatMap** te puede ayudar.

    ~~~java
    class TransactionExample {

        public static long calcSumOfCanceledTransOnNonEmptyAccounts(List<Account> accounts) {
            return accounts.stream()
                    .filter(account -> account.getBalance() > 0)
                    .flatMap(account -> account.getTransactions().stream())
                    .filter(transaction -> transaction.getState().equals(State.CANCELED))
                    .map(Transaction::getSum)
                    .reduce(Long::sum).orElse(0L);
        }

        public static void main(String[] args) {

            Scanner scanner = new Scanner(System.in);

            int numberOfAccounts = Integer.parseInt(scanner.nextLine());
            List<Account> accounts = new ArrayList<>();

            for (int i = 0; i < numberOfAccounts; i++) {
                String[] accDesc = scanner.nextLine().split(" ");
                String number = accDesc[0];
                Long balance = Long.parseLong(accDesc[1]);

                int numberOfTransByAccount = Integer.parseInt(scanner.nextLine());
                List<Transaction> trans = new ArrayList<>();

                for (int j = 0; j < numberOfTransByAccount; j++) {
                    String[] transDesc = scanner.nextLine().split(" ");
                    String uuid = transDesc[0];
                    State state = convertStringToState(transDesc[1]);
                    Long sum = Long.parseLong(transDesc[2]);
                    trans.add(new Transaction(uuid, state, sum));
                }

                accounts.add(new Account(number, balance, trans));
            }

            System.out.println(calcSumOfCanceledTransOnNonEmptyAccounts(accounts));
        }

        private static State convertStringToState(String state) {
            switch (state) {
                case "c": return State.CANCELED;
                case "f": return State.FINISHED;
                case "p": return State.PROCESSING;
                default: throw new IllegalArgumentException("Unknown account state");
            }
        }

        enum State {
            FINISHED, CANCELED, PROCESSING
        }

        static class Transaction {

            private final String uuid;
            private final State state;
            private final Long sum;

            public Transaction(String uuid, State state, Long sum) {
                this.uuid = uuid;
                this.state = state;
                this.sum = sum;
            }

            public State getState() {
                return state;
            }

            public Long getSum() {
                return sum;
            }

            @Override
            public String toString() {
                return "Transaction{" +
                        "uuid='" + uuid + '\'' +
                        ", state=" + state +
                        ", sum=" + sum +
                        '}';
            }
        }

        static class Account {

            private final String number;
            private final Long balance;
            private final List<Transaction> transactions;

            public Account(String number, Long balance, List<Transaction> transactions) {
                this.number = number;
                this.balance = balance;
                this.transactions = transactions;
            }

            public Long getBalance() {
                return balance;
            }

            public List<Transaction> getTransactions() {
                return transactions;
            }

            @Override
            public String toString() {
                return "Account{" +
                        "number='" + number + '\'' +
                        ", balance=" + balance +
                        ", transactions=" + transactions +
                        '}';
            }
        }
    }
    ~~~

2. Supongamos que esta desarrollando un sistema de gestion de empleados para su empresa con dos clases **Employee** y **Department**. Ambas clases tienen getters para todos sus campos. Usando Stream API, implemente un metodo que *calcule el numero total de empleados* con un **salary >= threshold** para todos los departamente cuyo codigo comience con **"111-"**.

    ~~~java
    class EmployeesCounter {

        public static long calcNumberOfEmployees(List<Department> departments, long threshold) {
            return departments.stream()
                    .filter(department -> department.getCode().startsWith("111-"))
                    .flatMap(department -> department.getEmployees().stream())
                    .filter(employee -> employee.getSalary() >= threshold)
                    .count();
        }
    
        static class Employee {
            private final String name;
            private final Long salary;

            Employee(String name, Long salary) {
                this.name = name;
                this.salary = salary;
            }

            public String getName() {
                return name;
            }

            public Long getSalary() {
                return salary;
            }

            @Override
            public String toString() {
                return "Employee{" +
                        "name='" + name + '\'' +
                        ", salary=" + salary +
                        '}';
            }
        }

        static class Department {
            private final String name;
            private final String code;
            private final List<Employee> employees;

            Department(String name, String code, List<Employee> employees) {
                this.name = name;
                this.code = code;
                this.employees = employees;
            }

            public String getName() {
                return name;
            }

            public String getCode() {
                return code;
            }

            public List<Employee> getEmployees() {
                return employees;
            }

            @Override
            public String toString() {
                return "Department{" +
                        "name='" + name + '\'' +
                        ", code='" + code + '\'' +
                        ", employees=" + employees +
                        '}';
            }
        }
    }
    ~~~

3. Dada una lista, seleccione todas las posibles combinaciones de metodos.

    ~~~java
    list.stream().map(...).collect(...);
    list.stream().filter(...).map(...).filter(...).reduce(...);
    list.stream().map(...).map(...).map(...).filter(...).collect(...);
    ~~~

4. Debe implementar un metodo que resuelva el problema de reusar un stream despues de mapearla y filtrarla. El metodo debe guardar el stream y crear un proveedor que pueda devolver este stream uno y otra vez.  
    Por ejemplo, este codigo deberia generar 4, 8, 2 lineas separadas.

    ~~~java
    Stream<Integer> stream = Arrays.stream(new Integer[]{1, 2, 3, 4, 5, 6, 7, 8});
    Supplier<Stream<Integer>> saved = saveStream(stream.filter(e -> e % 2 == 0));

    System.out.println(saved.get().count());
    System.out.println(saved.get().max(Integer::compareTo).get());
    System.out.println(saved.get().min(Integer::compareTo).get());

    class FunctionUtils {

        public static <T> Supplier<Stream<T>> saveStream(Stream<T> stream){
            List<T> list = stream.collect(Collectors.toList());
            return list::stream;
        }
    }
    ~~~

5. Seleccione las formas correctas de contar el numero de ocurrencias de la palabra **targetWord** en la lista de cadenas **words** ignorando el case.

    ~~~java
    // forma 1
    words.stream()
             .filter(w -> w.equalsIgnoreCase(targetWord))
             .count();

    // forma 2
    words.stream()
             .map(w -> w.toUpperCase())
             .filter(w -> w.equals(targetWord.toUpperCase()))
             .count();

    // forma 3
    words.stream()
             .filter(w -> w.equalsIgnoreCase(targetWord))
             .map(w -> 1L)
             .reduce(0L, (count, next) -> count + 1);
    ~~~

6. Escriba un programa que lea un texto (en UTF-8) de la entrada estandar. El programa debe contar la frecuencia de palabras en el texto e imprimir las 10 palabras mas frecuentes. Las palabras del conteo no deben distinguir entre mayuscular y minusculas. Si el texto tiene menos de 10 palabras unicas, imprima todas las que haya.

    ~~~java
    public class Main {
        public static void main(String[] args) {
            Scanner scanner = new Scanner(System.in);

            Stream.of(scanner.nextLine().split("\\W+"))
                    .map(String::toLowerCase)
                    .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()))
                    .entrySet().stream()
                    .sorted(Map.Entry.<String, Long>comparingByValue()
                            .reversed()
                            .thenComparing(Map.Entry::getKey))
                    .limit(10)
                    .map(Map.Entry::getKey)
                    .forEachOrdered(System.out::println);
        }
    }
    ~~~

7. Escriba un metodo que retorne la suma de todos los numeros impares de un intervalo dado.

    ~~~java
    class Main {
    
        public static long sumOfOddNumbersInRange(long start, long end) {
            
            // solucion 1
            List<Long> numbers = new ArrayList<>();
            for (long i = start; i <= end; i++) {
                numbers.add(i);
            }
            return numbers.stream()
                    .filter(n -> n % 2 == 1)
                    .reduce(Long::sum).orElse(0L);

            // solucion 2
            return LongStream.rangeClosed(start, end)
                    .filter(number -> number % 2 != 0)
                    .sum();
        }

        public static void main(String[] args) {

            Scanner scanner = new Scanner(System.in);

            String[] line = scanner.nextLine().trim().split(" ");

            long start = Integer.parseInt(line[0]);
            long end = Integer.parseInt(line[1]);

            System.out.println(sumOfOddNumbersInRange(start, end));
        }
    }
    ~~~

8. Cual sera el resultado del codigo.

    ~~~java
    List<Integer> numbers = Arrays.asList(0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55);

    long result = numbers.stream()
            .filter(n -> n % 2 == 0)
            .mapToInt(x -> x)
            .sum();

    // 44
    ~~~

9. Cuales palabras tendra la lista filtrada.

    ~~~java
    List<String> paradigms = Arrays.asList(
            "Aspect-oriented programming",
            "Functional programming",
            "Object-oriented programming",
            "Logical programming",
            "Procedural programming",
            "Automata-based programming"
    );

    List<String> filtered = paradigms.stream()
            .filter(paradigm -> paradigm.contains("oriented") || paradigm.contains("Functional"))
            .flatMap(paradigm -> Arrays.stream(paradigm.split("\\s+")))
            .distinct()
            .collect(Collectors.toList());

    // Aspect-oriented, Functional, Object-oriented, programming
    ~~~
