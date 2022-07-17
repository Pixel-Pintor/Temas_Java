# Agrupando Collectors

Hemos aprendido como acumular elementos de stream en una coleccione o un solo valor usando la operacion **collect** la clave **Collectors**. Sin embargo, ademas de eso, el metodo **collect** puede ofrecer otras operaciones utiles como dividir elementos de stream en dos o mas grupos o aplicar un colector al resultado de otro colector. En este tema, veremos como ordenar los elementos de un stream usando los metodos **Collectors.partitioningBy** y **Collectors.groupingBy**.

## Fraccionamiento

Imagine que queremos dividir una coleccion de cuentas en dos grupos: cuentas cuyo saldo es mayor o igual a 10000 y cuentas con un saldo inferior a 10000. En otras palabras, necesitamos dividir las cuentas en dos grupos en funcion de un valor especifico. Se hace posible mediante el uso de una *operacion de particion*.  
La operacion de particion es presentada por el metodo **Collectors.partitioningBy** que acepta un predicado. Divide los elementos de entrada en un **Map** de dos listas: una lista contiene elementos para los cuales el predicado es verdadero y la otra contiene elementos para los cuales es falso. Las laves del **Map** son del tipo **Boolean**.

~~~java
List<Account> accounts = List.of(
    new Account(3333, "530012"),
    new Account(15000, "771843"),
    new Account(0, "681891")
);
~~~

Y dividirlos en dos listas con un predicado **balance >= 10000**:

~~~java
Map<Boolean, List<Account>> accountsByBalance = accounts.stream().collect(Collectors.partitioningBy(account -> account.getBalance() >= 10000));
~~~

El mapa **accountsByBalance** tiene las siguientes entradas.

~~~java
{
    false=[Account{balance=3333, number='530012'}, Account{balance=0, number='681891'}], 
    true=[Account{balance=15000, number='771843'}]
}
~~~

## Agrupamiento

La operacion de agrupacion es similar a la particion. Sin embargo, en lugar de dividir los datos en dos grupos en funcion de un predicado, la operacion de agrupacion puede producir cualquier numero de grupos en funcion de una *funcion de clasificacion* que asigna elementos a alguna clave.  
La operacion de agrupacion es presentada por el metodo **Collectors.groupingBy** que acepta una funcion de clasificacion. El metodo **groupingBy** tambien produce un **Map**. Las llaves de **Map** son valores producidos al aplicar la funcion de clasificacion a los elementos de entrada. Los valores correspondientes del **Map** son listas que contienen elementos mapeados por la funcion de clasificacion.  
Vamos a crear el enumerador **Status** y agregar el campo **status** a la clase **Account**.

~~~java
enum Status {
    ACTIVE,
    BLOCKED,
    REMOVED
}

public class Account {
    private long balance;
    private String number;
    private Status status;

    // constructor
    // getters and setters
}
~~~

Ademas, actualicemos la lista de cuentas:

~~~java
List<Account> accounts = List.of(
        new Account(3333L, "530012", Status.REMOVED),
        new Account(15000L, "771843", Status.ACTIVE),
        new Account(0L, "681891", Status.BLOCKED)
);
~~~

Ahora, podemos dividir todas las cuentas en grupos por su **status**:

~~~java
Map<Status, List<Account>> accountsByStatus = accounts.stream()
        .collect(Collectors.groupingBy(Account::getStatus));

// contenido del mapa
{
    BLOCKED=[Account{balance=0, number='681891'}], 
    REMOVED=[Account{balance=3333, number='530012'}], 
    ACTIVE=[Account{balance=15000, number='771843'}]
}
~~~

La operacion de agrupacion produce entradas cuando es necesarios, lo que significa que el **Map** de resultado puede contener cualquier numero de entradas. Por ejemplo, si la entrada es un stream vacio, el **Map** no contendra entradas.

## Colectores Downstream

Ademas de un predicado o una funcion de clasificacion, los colectores **partitioningBy** y **groupingBy** pueden aceptar un *dowstream*. Dicho colector se aplica a los resultados de otros colector. Por ejemplo, el colector **groupingBy**, que acepta una funcion de clasificacion y un colector descendente, agrupa elementos de acuerdo con una funcion de clasificacion y luego aplica un colector descendente especifico a los valores asociados con una clave determinada.

~~~java
List<Account> accounts = List.of(
        new Account(3333L, "530012", Status.ACTIVE),
        new Account(15000L, "771843", Status.BLOCKED),
        new Account(15000L, "234465", Status.ACTIVE),
        new Account(8800L, "110011", Status.ACTIVE),
        new Account(45000L, "462181", Status.BLOCKED),
        new Account(0L, "681891", Status.REMOVED)
);
~~~

Y calcular los saldos totales de las cuentas **blocked**, **active** y **removed** que utilizan un recopilador descendente:

~~~java
Map<Status, Long> sumByStatuses = accounts.stream()
        .collect(Collectors.groupingBy(
                Account::getStatus, 
                Collectors.summingLong(Account::getBalance))
         );

// mapa resultante
{ REMOVED=0, ACTIVE=27133, BLOCKED=60000 }
~~~

El codigo anterior agrupa las cuentas por el campo **status** y aplica un stream descendente **summingLong** a la lista de valores creados por el operador **groupingBy**.  
El metodo **groupingBy** tambien puede aceptar que un proveedor proporcione un mapa vacio en el que se insertaran los resultados. Es util si desea especificar la implementacion exacta de la interfaz **Map**, por ejemplo:

~~~java
Map<Status, Long> sumByStatuses = accounts.stream()
        .collect(Collectors.groupingBy(
                Account::getStatus,
                LinkedHashMap::new,
                Collectors.summingLong(Account::getBalance))
         );
~~~

## Colector Teeing

Java 12 introdujo el colector **teeing** que acepta dos colectores *downstream* y una funcion de fusion como argumentos y combina los resultados de los colectores *downstream* en el resultado final utilizando la funcion de fusion proporcionada. Veamos como funciona en un ejemplo usando la misma lista de cuentas que en el parrafo anterior.  
Supongamos que queremos extraer la siguiente informacion de la coleccion de cuentas: el numero de cuentas bloqueadas y la cantidad total de saldos de las cuentas bloqueadas.  
Asi es como podemos usar el colector de *tees* para resolver nuestra tarea:

~~~java
long[] countAndSum = accounts
        .stream()
        .filter(account -> account.getStatus() == Status.BLOCKED)
        .collect(Collectors.teeing(
                Collectors.counting(),
                Collectors.summingLong(Account::getBalance),
                (count, sum) -> new long[]{count, sum})
        );
~~~

Primero, filtramos el stream para obtener cuentas con el estado **BLOCKED**. Entonces usamos el colector **teeing** y pasamos el colector **counting** y colector **summingLong** para obtener el recuento de los elementos filtrados y la suma de sus saldos. Despues de eso, colocamos los resultados de los recopiladores posteriores en una matriz, que despues de que se procese el stream contendra los siguientes valores: **[2, 60000]**.  
De manera similar, podemos usar el colector de *tees* para retornar dos valores de una sola secuencia.

---

## Ejercicios

1. Escriba un recopilador que divida todas las palabras de un stream en dos grupos: *palindromos(verdaderos)* y *palabras habituales(falso)*. El recopilador debe retornar **Map<Boolean, List<String>>**.

    ~~~java
    class CollectorPalindrome {

        static boolean isPalindrome(String word) {
            return new StringBuilder(word)
                    .reverse()
                    .toString()
                    .equals(word);
        }

        public static void main(String[] args) {

            Scanner scanner = new Scanner(System.in);
            String[] words = scanner.nextLine().split(" ");
            
            Map<Boolean, List<String>> palindromeOrNo = Arrays.stream(words)
                    .collect(
                            Collectors.partitioningBy(
                                    CollectorPalindrome::isPalindrome,
                                    Collectors.toList()
                            )
                    );

            palindromeOrNo = new linkedHashMap<>(palindromeOrNo);
            System.out.println(palindromeOrNo);
        }
    }
    ~~~

2. ¿De que manera se utilizan los colectores *downstream*?

    - Se aplican a los resultados de otros colectores

3. Cual colector divide los datos por un predicado dentro de un mapa con claves de tipo **boolean**?

    - **partitioningBy**

4. Escribe un recolector que calcula *la suma total de transacciones (tipo long, no entero) por cada cuenta (es decir, por numero de cuenta)*. El recolector se aplicara a un stream de transacciones.

    ~~~java
    class TransactionCollector {

        public static Map<String, Long> getAccountToTransSum(List<Transaction> trans) {
            return trans.stream()
                    .collect(
                            groupingBy(
                                    transaction -> transaction.getAccount().getNumber(), summingLong(Transaction::getSum)
                            )
                    );
        }

        static class Transaction {

            private final String uuid;
            private final Long sum;
            private final Account account;

            public Transaction(String uuid, Long sum, Account account) {
                this.uuid = uuid;
                this.sum = sum;
                this.account = account;
            }

            public String getUuid() {
                return uuid;
            }

            public Long getSum() {
                return sum;
            }

            public Account getAccount() {
                return account;
            }

            @Override
            public String toString() {
                return "Transaction{" +
                        "uuid='" + uuid + '\'' +
                        ", sum=" + sum +
                        '}';
            }
        }

        static class Account {
            private final String number;
            private final Long balance;

            public Account(String number, Long balance) {
                this.number = number;
                this.balance = balance;
            }

            public String getNumber() {
                return number;
            }

            public Long getBalance() {
                return balance;
            }

            @Override
            public String toString() {
                return "Account{" +
                        "number='" + number + '\'' +
                        ", balance=" + balance +
                        '}';
            }
        }
    }
    ~~~

5. El colector **groupingBy** puede aceptar los siguientes parametros.

    - Funcion de clasificacion
    - Colector downstream
    - Supplier

6. Escribe un colector que calcula cuantas veces cada **url** recibio un clic. El recopilador se aplicara a un stream de entradas de registro para crear un mapa: *url -> click count*.

    ~~~java
    class Monitor {

        public static Map<String, Long> getUrlToNumberOfVisited(List<LogEntry> logs) {
            return logs.stream()
                    .collect(
                            groupingBy(
                                    LogEntry::getUrl,
                                    counting()
                            )
                    );
        }

        static class LogEntry {

            private Date created;
            private String login;
            private String url;

            public LogEntry(Date created, String login, String url) {
                this.created = created;
                this.login = login;
                this.url = url;
            }

            public String getUrl() {
                return url;
            }

            public void setUrl(String url) {
                this.url = url;
            }

            @Override
            public boolean equals(Object o) {
                if (this == o) {
                    return true;
                }
                if (!(o instanceof LogEntry)) {
                    return false;
                }

                LogEntry logEntry = (LogEntry) o;

                if (!login.equals(logEntry.login)) {
                    return false;
                }
                return url.equals(logEntry.url);
            }

            @Override
            public int hashCode() {
                int result = login.hashCode();
                result = 31 * result + url.hashCode();
                return result;
            }

            @Override
            public String toString() {
                return "LogEntry{" +
                        ", created=" + created +
                        ", login='" + login + '\'' +
                        ", url='" + url + '\'' +
                        '}';
            }
        }
    }
    ~~~

7. Cual sera el **Map** de resultado para el codigo.

    ~~~java
    Map<Integer, Integer[]> numbers = Stream.of(1, 2, 1, 3, 2, 4).collect(Collectors.groupingBy(Function.identity()));

    // {1=[1, 1], 2=[2, 2], 3=[3], 4=[4]}
    ~~~

8. Cual es la declaracion correcta despues de ejecutar el codigo.

    ~~~java
    Map<Boolean, Long> map = Stream.of(5, 1, 9, -2, -5, 1)
            .collect(Collectors.partitioningBy(n -> n > 0, Collectors.summingLong(x -> x)));

    map.get(false); // -7
    map.get(true); 16
    ~~~

9. Cual sera el resultado del codigo.

    ~~~java
    enum Status {ACTIVE, BLOCKED, REMOVED}

    class Account {
        private long balance;
        private String number;
        private Status status;

        public Account(long balance, String number, Status status) {
            this.balance = balance;
            this.number = number;
            this.status = status;
        }

        // getters and setters
    }

    class Report {
        List<String> blocked;
        List<String> active;

        public Report(List<String> blocked, List<String> active) {
            this.blocked = blocked;
            this.active = active;
        }

        @Override
        public String toString() { return blocked + ", " + active; }
    }

    List<Account> accounts = List.of(
            new Account(3333L, "530012", Status.ACTIVE),
            new Account(15000L, "771843", Status.BLOCKED),
            new Account(15000L, "234465", Status.ACTIVE),
            new Account(8800L, "110011", Status.ACTIVE),
            new Account(45000L, "462181", Status.BLOCKED),
            new Account(0L, "681891", Status.REMOVED)
    );

    Report report = accounts
            .stream()
            .collect(Collectors.teeing(
                    Collectors.filtering(Account::isBlocked,
                        Collectors.mapping(Account::getNumber,
                            Collectors.toList())),
                    Collectors.filtering(Account::isActive,
                        Collectors.mapping(Account::getNumber,
                            Collectors.toList())),
                    Report:new)
            );
    
    System.out.println(report);

    // [771843, 462181], [530012, 234465, 110011]
    ~~~
