# Interfaces Funcionales Estandar

Ya ha aprendido a crear y utilizar interfaces funcionales. Sin embargo, no tiene que crear su propia interfaz funcional cada vez que la necesite para un caso de uso común. En su lugar, podría usar interfaces funcionales integradas que se presentaron en Java 8 y se pueden encontrar en el paquete **java.util.function**. En este tema, aprenderá sobre las interfaces funcionales integradas, sus tipos y convenciones de nomenclatura, y cómo usarlas.

## Grupos de interfaces funcionales

Todas las interfaces funcionales que se presentan en el paquete **java.util.function** se pueden dividir en cinco grupos:

- **funciones** que aceptan argumentos y producen resultados
- **operadores** que producen resultados del mismo tipo que sus argumentos (un caso especial de funcion)
- **predicados** que aceptan argumentos y devuelven valores booleanos (funciones con valores booleanos)
- **proveedores** que no aceptan nada y devuelven valores
- **consumidores** que aceptan argumentos y no devuelven ningun resultado.

Las interfaces funcionales del mismo grupo pueden diferir en el numero de argumentos y ser genericas y no genericas.  

## Convenio de denominacion

Gracias a las convenciones de nomenclatura de las interfaces funcionales en el paquete **java.util.function**, puede comprender fácilmente una característica de la interfaz con solo mirar el prefijo de su nombre, que puede indicar lo siguiente:

- Una serie de parámetros que son aceptados por una operación. El prefijo *Bi* indica que una función, un predicado o un consumidor acepta dos parámetros. De manera similar al prefijo *Bi* Unario y Binario indican que un operador acepta uno y dos parámetros respectivamente.
- Un tipo de parámetros de entrada. *Double* , *Long* , *Int* y *Obj* indican el tipo de valor de entrada. por ejemplo, la interfaz **IntPredicate** representa un predicado que acepta el valor de un **int**.
- Un tipo de parametro de salida. Los prefijos *ToDouble*, *ToLong* y *ToInt* indican el tipo de valor de salida. Por ejemplo, la interfaz **ToIntFunction<T>** representa una funcion que devuelve el valor de un **int**.

Tenga en cuenta que algunas interfaces funcionales tienen prefijos combinados. Simplemente significa que debe aplicar las reglas enumeradas teniendo en cuenta cada prefijo. Por ejemplo, mirando los prefijos de la interfaz **DoubleIntFunction**, podemos ver que acepta un valor de tipo **double** y retorna un valor de tipo **int**.

## Interfaces funcionales estandar con ejemplos

Veamos ejemplos para cada uno de los cinco grupos de interfaces funcionales estandar.

1. Functions
    Cada funcion acepta un valor como parametro y retorna un unico valor. Por ejemplo, la interfaz **Function<T, R>** es una interfaz generica que representa una funcion que acepta un valor de tipo **T** y produce un resultado de tipo **R**.

    ~~~java
    // String to Integer function
    Function<String, Integer> converter = Integer::parseInt;
    converter.apply("1000"); // the result is 1000 (Integer)

    // String to int function
    ToIntFunction<String> anotherConverter = Integer::parseInt;
    anotherConverter.applyAsInt("2000"); // the result is 2000 (int)

    // (Integer, Integer) to Integer function
    BiFunction<Integer, Integer, Integer> sumFunction = (a, b) -> a + b;
    sumFunction.apply(2, 3); // it returns 5 (Integer)
    ~~~

2. Operators
    Cada operador toma y devuelve los valores del mismo tipo. Por ejemplo, **UnaryOperator<T>** representa un operador que acepta un valor de tipo **T** y produce un resultado del mismo tipo **T**.

    ~~~java
    // Long to Long multiplier
    UnaryOperator<Long> longMultiplier = val -> 100_000 * val;
    longMultiplier.apply(2L); // the result is 200_000L (Long)

    // int to int operator
    IntUnaryOperator intMultiplier = val -> 100 * val;
    intMultiplier.applyAsInt(10); // the result is 1000 (int)

    // (String, String) to String operator
    BinaryOperator<String> appender = (str1, str2) -> str1 + str2;
    appender.apply("str1", "str2"); // the result is "str1str2"
    ~~~

3. Predicates
    Cada predicado acepta un valor como parametro y devuelve **true** o **false**. Por ejemplo, la interfaz **Predicate<T>** es uan interfaz generica que representa un predicado que acepta un valor de tipo **T** y produce un resultado de valor boolean.

    ~~~java
    // Character to boolean predicate
    Predicate<Character> isDigit = Character::isDigit;
    isDigit.test('h'); // the result is false (boolean)

    // int to boolean predicate
    IntPredicate isEven = val -> val % 2 == 0;
    isEven.test(10); // the result is true (boolean)
    ~~~

4. Suppliers
    Cada proveedor no acepta parametrs y devuelve un unico valor. Por ejempl, **Supplier<T>** representa un proveedor que no acepta argumentos y devuelve un valor del tipo **T**.

    ~~~java
    Supplier<String> stringSupplier = () -> "Hello";
    stringSupplier.get(); // the result is "Hello" (String)

    BooleanSupplier booleanSupplier = () -> true;
    booleanSupplier.getAsBoolean(); // the result is true (boolean)

    IntSupplier intSupplier = () -> 33;
    intSupplier.getAsInt(); // the result is 33 (int)
    ~~~

5. Consumers
    Cada consumidor acepta un valor como parametro y no devuelve ningun resultado. Por ejemplo, la interfaz **Consumer<T>** representa un consumidor que acepta un valor de tipo **T** y no devuelve ningun resultado.

    ~~~java
    // it prints a given string
    Consumer<String> printer = System.out::println;
    printer.accept("!!!"); // It prints "!!!"
    ~~~

## Conclusion

En este tema, hemos observado un conjunto de interfaces funcionales integradas que se pueden dividir en cinco grupos. Cada uno de ellos se puede utilizar como una expresión lambda o una referencia de método. Es importante que los parámetros de entrada y el valor devuelto de una expresión lambda (o una referencia de método) se correspondan con los parámetros de entrada y el valor devuelto de un único método abstracto presentado en la interfaz funcional. Además, hemos observado las convenciones de nomenclatura de la interfaz y hemos descubierto cómo los prefijos de los nombres indican la cantidad de parámetros de entrada y el tipo de valores de entrada y salida. Además, observaremos cómo usar juntas las interfaces funcionales estándar genéricas y especializadas en primitivos.  

---

## Ejercicios

1. Haga coincidor las interfaces funcionales con expresiones lambda adecuadas o referencias de metodos.

    ~~~java
    IntSupplier = () -> 3
    Consumer<String> = System.out::println
    BiPredicate<Integer, Integer> = (x, y) -> x % y == 0
    DoubleUnaryOperator = Math::sin
    Function<Double, String> = (x) -> String.valueOf(x * x)
    ~~~

2. Tienes una funcion, describela.

    ~~~java
    // Toma dos argumentos (String y Double) y retorna un valor del tipo long primitivo
    ToLongBiFunction<String, Double> f = ...;
    ~~~

3. Haga coincidir cada expresion lambda con un metodo que pueda aceptar esta expresion lambda.

    ~~~java

    DoubleFunction<Double> f = (x) -> {
        return x + 10;
    }

    BiPredicate<Integer, Double> p = (x, y) -> {
        (x > y) && (x % 2 == 0);
    }

    Supplier<String> s = () -> {
        Long.valueOf(10_000L).toString();
    }

    BiConsumer<Integer, String> c = (x, y) -> { }

    UnaryOperator<Lis<String>> o = (x) -> {
        x.subList(0, x.size());
    }
    ~~~

4. Cual es el nombre del grupo de interfaces funcionales cuyos metodos abstractos individuales no toman argumentos y devuelven un valor?

    - Suppliers

5. Implementar el metodo **ternaryOperator** que acepta un predicado **condition** y dos funciones **ifTrue** y **ifFalse** y devuelve una funcion. La funcion de retorno toma un argumento, comprueva si el predicado **condition** es **true** para este argumento, y si lo es, se aplica **ifTrue** al argumento, de lo contrario se aplica **ifFalse**.

    ~~~java
    /*
    Ejemplo:
    En este ejemplo, la funcion resultando devuelve la longitud de una cadena si la referncia a la cadena no es null, de lo contrario, devuelve 0
    */
    Predicate<Object> condition = Objects::isNull;
    Function<Object, Integer> ifTrue = obj -> 0;
    Function<CharSequence, Integer> ifFalse = CharSequence::length;
    Function<String, Integer> safeStringLength = ternaryOperator(condition, ifTrue, ifFalse);

    class Operator {

        public static <T, U> Function<T, U> ternaryOperator(
                Predicate<? super T> condition,
                Function<? super T, ? extends U> ifTrue,
                Function<? super T, ? extends U> ifFalse) {

            return t -> condition.test(t) ? ifTrue.apply(t) : ifFalse.apply(t);
        }
    }
    ~~~

6. Seleccione los grupos que tengan un metodo abstracto que tome un valor o valores y retorne un resultado no **void**.

    - Functions
    - Predicates
    - Operators

7. Cree un **Supplier** que devuelve valores de enteros de 0 a infinito. Ademas, deberia ser posible utilizar proveedores separados simultaneamente.

    ~~~java
    // Ejemplo 1
    Supplier<Integer> sup = getInfiniteRange();
    for (int i = 0; i < 5; i++) {
        System.out.println(sup.ge() + " ");
    }
    // 0 1 2 3 4

    // Ejemplo 2
    Supplier<Integer> sup1 = getInfiniteRange();
    Supplier<Integer> sup2 = getInfiniteRange();
    for (int i = 0; i < 5; i++) {
        System.out.println(sup1.get() + " " + sup2.get() + " ");
    }

    class FunctionUtils {

        public static Supplier<Integer> getInfiniteRange() {

            return new Supplier<Integer>() {
                int x = 0;
                @Override
                public Integer get() {
                    return x++;
                }
            };
        }
    }
    ~~~

8. Dada la declaracion de **isPalindrome**, seleccione todas las interfaces estandar que funcionarian como ese metodo.

    ~~~java
    public static boolean isPalindrome(String s) { ... }

    // interfaz estandar 1
    Predicate<String>
    Function<String, Boolean>
    ~~~

9. La clase **Account** tiene metodos para recuperar los valores de sus campos correspondientes, escribir codigo para filtrar la lista de **accounts** usando el metodo filter de dos maneras:

    1. Obtener **todas las cuentas no vacias** y guardelas en la variable **nonEmptyAccounts**.
    2. Obtener cuentas **no bloqueadas** con **demasiado dinero** (**balance** >= 100_000_000) y guardelas en la variable **accountsWithTooMuchMoney**.

    ~~~java
    // Este codigo obtiene los numeros pares de una lista
    List<Integer> numbers = ...;
    List<Integer> evenNumbers = filter(nubers, number -> number % 2 == 0);

    class Main {

        public static void printFilteredAccounts(List<Account> accounts) {
            int max = 100_000_000;
            int min = 0;
            List<Account> nonEmptyAccounts = filter(accounts, account -> account.balance > min);
            List<Account> accountsWithTooMuchMoney = filter(accounts, account -> account.balance >= max && !account.isLocked());

              
            nonEmptyAccounts.forEach(a -> System.out.print(a.getNumber() + " "));
            accountsWithTooMuchMoney.forEach(a -> System.out.print(a.getNumber() + " "));
        }

        public static <T> List<T> filter(List<T> elems, Predicate<T> predicate) {
            return elems.stream()
                    .filter(predicate)
                    .collect(Collectors.toList());
        }

        public static void main(String[] args) {

            final Scanner scanner = new Scanner(System.in);
            final int n = Integer.parseInt(scanner.nextLine());
            final List<Account> accounts = new ArrayList<>();

            for (int i = 0; i < n; i++) {
                final String[] values = scanner.nextLine().split("\\s+");
                final Account acc = new Account(
                        values[0], Long.parseLong(values[1]), Boolean.parseBoolean(values[2])
                );
                accounts.add(acc);
            }

            printFilteredAccounts(accounts);
        }

        static class Account {
            private String number;
            private long balance;
            private boolean locked;

            public Account(String number, long balance, boolean locked) {
                this.number = number;
                this.balance = balance;
                this.locked = locked;
            }

            public long getBalance() {
                return balance;
            }

            public void setBalance(long balance) {
                this.balance = balance;
            }

            public boolean isLocked() {
                return locked;
            }

            public void setLocked(boolean locked) {
                this.locked = locked;
            }

            public String getNumber() {
                return number;
            }

            public void setNumber(String number) {
                this.number = number;
            }

            @Override
            public String toString() {
                return "Account{" +
                        "number='" + number + '\'' +
                        ", balance=" + balance +
                        ", isLocked=" + locked +
                        '}';
            }
        }
    }
    ~~~

10. Seleccione las declaraciones correctas sobre las interfaces funcionales del paquete **java.util.function**.

    - Algunas interfaces funcionales tienen metodos abstractos unicos que no devuelven ningun valor
    - No hay funciones estandar que tomen tres argumentos
    - Los predicados siempre devuelven verdadero o falso
