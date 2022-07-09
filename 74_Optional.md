# Optional

Como muchos lenguajes de programación, Java utiliza **null** para representar la ausencia de un valor. A veces, este enfoque conduce a excepciones como **NPE**, mientras que las comprobaciones no nulas hacen que el código sea menos legible. El informático británico Tony Hoare, el inventor de concepto **null**, incluso describe la introducción de **null** como un *"error de miles de millones de dólares"*, ya que ha provocado innumerables errores, vulnerabilidades y bloqueos del sistema. Para evitar los problemas asociados con **null**, Java proporciona la clase **Optional** que es una alternativa más segura para el estándar de referencias **null**.

## Valores Opcionales

La clase **Optional<T>** representa la presencia o ausencia de un valor del tipo especificado **T**. Un objeto de esta clase puede estar **vacio** o **no vacio**.  
Veamos un ejemplo. En el siguiente codigo, creamos dos objetos opcionales llamados **absent** y **present**. El primer objeto representa un valor vacio (como **null**), y el segundo mantiene un valor de cadena real.

~~~java
Optional<String> absent = Optional.empty();
Optional<String> present = Optional.of("Hello");
~~~

El metodo **isPresent** comprueba si un objeto esta vacio o no:

~~~java
System.out.println(absent.isPresent()); // false
System.out.println(presente.isPresent()); // true
~~~

A partir de Java 11, tambien podemos invocar lo contrario al metodo **isEmpty**. Si pasas **null** al metodo **of**, causara un **NPE**.

## Opcionales y objetos anulables

En una situacion en la que no sabe si una variable es **null** o no, debe pasarla al metodo **ofNullable** en lugar el metodo **of**. Crea un Opcional vacio si el valor pasado es **null**. La palabra **nullable** significa que una variable es potencialmente **null**.  
En el siguiente ejemplo, el metodo **getRandomMessage** puede regresar **null** o algun mensaje de cadena. Dependiendo de lo que se devuelva, el resultado sera diferente.

~~~java
String message = getMessage(); // podria ser null

Optional<String> optMessage = Optional.ofNullable(message);

System.out.println(optMessage.isPresent()); // true
~~~

Si el **message** no es **null**, el codigo se imprimira **true**. De lo contrario, se imprimira **false** porque el objeto opcional esta vacio.  
En un sentido, **Optional** es como una caja que contiene algun valor o nada. Envuelve un valor o **null** manteniendo la posibilidad de comprobarlo utilizando metodos especiales.

## Obtener el valor de un Optional

Lo mas obvio que se puede hacer con un Optional es obtener su valor. Por ahora, vamos a discutir tres metodos con tal proposito:

- **get** retorna el valor si esta presente, de lo contrario lanza una excepcion
- **orElse** retorna el valor si esta presente, de lo contrario retornar **other**
- **orElseGet** retornar el valor si esta presente, de lo contrario invoca **other** y retorna su resultado.

Veamos como funciona. Primero, usamos el metodo **get** para obtener el valor presente.

~~~java
Optional<String> optName = Optional.of("John");
String name = optName.get(); // "John"
~~~

Este codigo funciona bien y retorna el nombre **"John"** del Optional. Pero si el objeto esta vacio, el programa lanza una **NoSuchElementException** excepcion.  

~~~java
Optional<String> optName = Optional.ofNullable(null);
String name = optName.get(); // throws NoSuchElementException
~~~

El metodo **orElse** se utiliza para extraer el valor envuelto dentro de un objeto Opcional o devolver algun valor predeterminado cuando el opcional esta vacio. El valor predeterminado se pasa al metodo como su argumento.

~~~java
String nullableName = null;
String name = Optional.ofNullable(nullableName).orElse("unknown");
        
System.out.println(name); // unknown
~~~

A diferencia del ejemplo anterior, este no genera una excepcion, sino que devuelve un valor predeterminado.  
El metodo **orElseGet** es bastante similar, pero se necesita una **funcion de proveedor** para producir un resultado en lugar de tomar algun valor para devolver.

~~~java
String name = Optional
        .ofNullable(nullableName)
        .orElse(SomeClass::getDefaultResult);
~~~

En este ejemplo, usamos el metodo **getDefaultResult** para producir un resultado predeterminado.

## Acciones condicionales

Tambien hay metodos convenientes que toman funciones como argumentos y realizan algunas acciones en valores envueltos dentro de Opcional.

- **ifPresent** realiza la accion dada con el valor, de lo contario no hace nada
- **ifPresentOrElse** realiza la accion dada con el valor, de lo contrario, realiza la accion dada basada en vacio.

El metodo **ifPresent** nos permite ejecutar alguno codigo en el valor si Opcional no esta vacio. El metodo toma una **funcion de consumidor** que puede procesar el valor.  
El siguiente ejemplo imprime la longitud del nombre de una empresa utilizando el **ifPresent**.  

~~~java
Optional<String> companyName = Optional.of("Google");
companyName.ifPresent((name) -> System.out.println(name.length())); // 6
~~~

Sin embargo, el siguiente codigo no imprime nada porque el objeto opcional esta vacio.

~~~java
Optional<String> noName = Optional.empty();
noName.ifPresent((name) -> System.out.println(name.length()));
~~~

El equivalente clasico de estos dos fragmentos de codigo tiene el siguiente aspecto.

~~~java
String companyName = ...;
if (companyName != null) {
    System.out.println(companyName.length());
}
~~~

Este codigo es mas propenso a errores porque es posible olvidarse de verificar el **null** y que eso genere un **NPE**.  
El metodo **ifPresenteOrElse** es una alternativa mas segura a toda declaracion **if-else**. Ejecuta una de dos funciones dependiendo de si el valor esta presente el **Optional**.

~~~java
Optional<String> optName = Optional.ofNullable(/* some value goes here */);

optName.ifPresentOrElse(
        (name) -> System.out.println(name.length()), 
        () -> System.out.println(0)
);
~~~

Si **optName** contiene algun valor (como **"Google"**), se llama a la expresion lambda e imprime la longitud del nombre. Si **optName** esta vacio, la segunda funcion imprime **0** como valor predeterminado. A veces, los desarrolladores llaman a la segunda expresion **lambda alternativa** que es un plan alternativo si algo salio mal (sin valor).

---

## Ejercicios

1. Escriba un codigo que sume todos los valores e imprima el resultado. Si **Optional** esta vacio, solo saltelo.

    ~~~java
    class Main {
        // Solucion 1
        public static void main(String[] args) {
            ValueProvider provider = new ValueProvider();
            List<Optional<Integer>> optionalList = provider.getValues();
            Integer sum = 0; 
            for (Optional<Integer> num : optionalList) {
                sum += num.orElse(0);
            }
            System.out.println(sum);
        }

        // Solucion 2
        public static void main(String[] args) {
            ValueProvider provider = new ValueProvider();
            List<Optional<Integer>> listValues = provider.getValues();
            AtomicInteger sum = new AtomicInteger();
            for (Optional<Integer> value : listValues) {
                value.ifPresent(sum::addAndGet);
            }
            System.out.println(sum);
        }
    }

    class ValueProvider {
        private List<Optional<Integer>> opts; // cache to provide reproducing method invocation

        public List<Optional<Integer>> getValues() {
            if (opts != null) {
                return opts;
            }

            java.util.Scanner scanner = new java.util.Scanner(System.in);
            int number = scanner.nextInt();
            opts = java.util.stream.IntStream
                    .rangeClosed(1, number)
                    .mapToObj(n -> {
                        String val = scanner.next();
                        return "null".equals(val) ?
                            Optional.<Integer>empty() :
                            Optional.of(Integer.valueOf(val));
                    })
                    .collect(java.util.stream.Collectors.toList());

            return opts;
        }
    }
    ~~~

2. Implementa el metodo **getValue**. Deberia leer un **String** de la consol y construir un objeto **Optional<String>** con el valor. Si la entrada es igual a **emtpy** crea un **Optional** vacio.

    ~~~java
    class Main {
        public static void main(String[] args) {
            InputStringReader reader = new InputStringReader();        
            Optional<String> value = reader.getValue();
            if (value.isPresent()) {
                System.out.println("Value is present: " + value.get());
            } else {
                System.out.println("Value is empty");
            }
        }
    }

    class InputStringReader {
        public Optional<String> getValue() {
            Scanner scanner = new Scanner(System.in);
            String value = scanner.nextLine();
            Optional<String> optional = Optional.empty();
            if (!Objects.equals(value, "empty")) {
                optional = Optional.of(value);
            }
            return optional;
        }
    }
    ~~~

3. Escribe un codigo en el **Main** que imprime el valor de **provider.getValue** si no es **null**.

    ~~~java
    class Main {
        public static void main(String[] args) {
            ValueProvider provider = new ValueProvider();
            Optional<String> value = provider.getValue();

            // Solucion 1
            if (value.isPresent()) {
                System.out.println(value.get());
            }

            // Solucion 2
            value.ifPresent(System.out::println);
        }
    }

    class ValueProvider {
        private Optional<String> inputOpt;
        public Optional<String> getValue() {
            if (inputOpt.isEmpty()) {
                java.util.Scanner scanner = new java.util.Scanner(System.in);
                String input = scanner.next();
                inputOpt = "null".equals(input) ? Optional.empty() : Optional.of(input);
            }

            return inputOpt;
        }
    }
    ~~~

4. Debe completar el codigo para imprimir el nombre de la persona y su direccion (separados por la frase **lives at**) si la direccion esta presente y si no lo esta imprimir **"Unknown"**.

    ~~~java
    public class Main {

        public static void main(String[] args) {
            Scanner scanner = new Scanner(System.in);

            String name = scanner.nextLine();
            Optional<String> optAddress = AddressBook.getAddressByName(name);

            // solucion 1
            optAddress.ifPresentOrElse(
                    address -> System.out.println(name + " lives at " + address),
                    () -> System.out.println("Unknown")
            );

            // solucion 2
            System.out.println(optAddress.isPresent() ? "%s lives at %s".formatted(name, optAddress.get()) : "Unknown");
        }
    }

    class AddressBook {
        private static final Map<String, String> namesToAddresses = new HashMap<>();

        static {
            namesToAddresses.put("Pansy Barrows", "63 Shub Farm Drive, Cumberland, RI 02864");
            namesToAddresses.put("Kevin Bolyard", "9526 Front Court, Hartsville, SC 29550");
            namesToAddresses.put("Earl Riley", "9197 Helen Street, West Bloomfield, MI 48322");
            namesToAddresses.put("Christina Doss", "7 Lincoln St., Matawan, NJ 07747");
        }

        static Optional<String> getAddressByName(String name) {
            return Optional.ofNullable(namesToAddresses.get(name));
        }
    }
    ~~~

5. Que es **Optional**?

    - Es una contenedor que puede estar vacio o lleno con un valor

6. **Optional** fue creado para evitar que excepciones?

    - **NullPointerException**

7. Cual metodo se debe utilizar para crear un **Optional** basado en un objeto que podria ser **null**?

    - **orNullable(object)**
