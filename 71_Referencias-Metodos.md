# Referencias de metodos

Como sabe, las expresiones lambda le permiten usar código como datos y pasarlo como argumentos de un método. Otra forma de hacerlo es usar referencias a métodos. A menudo son incluso más legibles que las expresiones lambda correspondientes. Además, las referencias a métodos obligan a los desarrolladores a descomponer un programa en un conjunto de métodos cortos con áreas claras de responsabilidad.  

## Haga el codigo mas clase con referencias de metodos

Por referencia de método, nos referimos a una función que se refiere a un método en particular a través de su nombre y puede invocarse en cualquier momento que lo necesitemos. La sintaxis base de una referencia de método se ve así:

~~~java
objectOrClass :: methodName
~~~

Donde **objectOrClass** puede ser un **nombre de clase** o una **instancia particular** de una clase.  
Aqui hay un ejemplo, creamos una referencia al metodo estatico estandar **max** de la clase **Integer**.

~~~java
BiFunction<Integer, Integer, Integer> max = Integer::max;
~~~

Aqui, **Integer::max** es una referencia de metodo a un metodo estatico.  
Este codigo funciona porque la definicion del metodo **int max(int a, int b)** se ajusta el tipo **BiFunction<Integer, Integer, Integer>**: ambos significan tomar dos argumentos enteros y retornar un entero.  
Ahora tenemos el objeto **max** que se puede utilizar como una funcion invocando el metodo **apply**.

~~~java
System.out.println(max.apply(50, 70));
~~~

Entonces, una vez asignada a un objeto, una referencia de metodo funcion de la misma manera que una expresion lambda.  
Aquia hay una forma alternativa de crear el mismo objeto usando una expresion lambda.

~~~java
BiFunction<Integer, Integer, Integer> max (x, y) -> Integer.max(x, y);
~~~

Se recomienda usar referencias de metodos en lugar de expresiones lambda si solo necesita invocar un metodo estandar sin otras operaciones. Su codigo sera mas corto, mas legible y mas facil de probar.  

## Tipos de referencias de metodos

Es posible escribir referencias de metodos a metodos estaticos y de instancia (no estaticos).  
En general, hay cuatro tipos de referencias de metodos:

- referencia a un metodo estatico
- referencia a un metodo de instancia de un objeto existente
- referencia a un metodo de instancia de un objeto de un tipo particular
- referencia a un constructor

1. Referencia a un metodo estatico  
    La forma general es la siguiente:

    ~~~java
    ClassName :: staticMethodName
    ~~~

    Echemos un vistazo a la referencia al metodo estatico **sqrt** de la clase **Math**:

    ~~~java
    Function<Double, Double> sqrt = Math::sqrt;
    ~~~

    Ahora podemos invocar el metodo **sqrt** para los valores dobles:

    ~~~java
    sqrt.apply(100.0d); // the result is 10.0d
    ~~~

    El metodo **sqrt** tambien se puede escribir usando la siguiente expresion lambda:

    ~~~java
    Function<Double, Double> sqrt = x -> Math.sqrt(x);
    ~~~

2. Referencias a un metodo de instancia de un objeto
    La forma general se ve asi:

    ~~~java
    objectName :: instanceMethodName
    ~~~

    Veamos el ejemplo de una referencia al metodo **indexOf** de una cadena en particular.

    ~~~java
    String whatsGoingOnText = "What's going on there?";
    Function<String, Integer> indexWithinWhatsGoingOnText = whatsGoingOnText::indexOf;
    ~~~

    Aqui esta el resultado de aplicarlo a diferentes argumentos:

    ~~~java
    System.out.println(indexWithinWhatsGoingOnText.apply("going")); // 7
    System.out.println(indexWithinWhatsGoingOnText.apply("Hi"));    // -1
    ~~~

    Como puede ver, en realidad siempre trabajamos con el objeto **whatsGoingOnText** capturado del contexto.  
    El siguiente ejemplo de una expresion lambda es una equivalente completo de la referencia anterior y puede mejorar su compresion de la situacion:

    ~~~java
    Function<String, Integer> indexWithinWhatsGoingOnText = string -> whatsGoingOnText.indexOf(string);
    ~~~

3. Referencia a un metodo de instancia de un objeto de un tipo particular
    Aqui hay una forma general de una referencia.

    ~~~java
    ClassName :: instanceMethodName
    ~~~

    En ese caso, debe pasar una instancia de la clase como argumento de la funcion.  
    Centremonos en la siguiente referencia a una instancia del metodo **doubleValue** de la clase **Long**:  

    ~~~java
    Function<Long, Double> converter = Long::doubleValue;
    ~~~

    Ahora podemos invocar el **converter** para valores long:

    ~~~java
    converter.apply(100L); // resultado es 100.0d
    converter.apply(200L); // resultado es 200.0d
    ~~~

    Ademas, podemos escribir el mismo convertidos usando la siguiente expresion lambda.

    ~~~java
    Function<Long, Double> converter = val -> val.doubleValue();
    ~~~

4. Referencia a un constructor
    Esta referencia tiene la siguiente declaracion.

    ~~~java
    ClassName :: new
    ~~~

    Por ejemplo, consideremos nuestra clase personalizada **Person** con un solo campo **name**.  

    ~~~java
    class Person {
        String name;

        public Person(String name) {
            this.name = name;
        }
    }
    ~~~

    Aqui hay una referencia al constructor de esta clase:

    ~~~java
    Function<String, Person> personGenerator = Person::new;
    ~~~

    Esta funcion produces nuevos objetos **Person** en funciones de sus nombres.

    ~~~java
    Person johnFoster = personGenerator.apply("John Foster");
    ~~~

    Aqui hay una expresion lambda correspondiente que hace lo mismo.

    ~~~java
    Function<String, Person> personGenerator = name -> new Person(name);
    ~~~

    Ademas, usaremos expresiones lambda y referencias de metodos juntas.  

---

## Ejercicios

1. Conecte cada referencia de metodo con su ejemplo.

    ~~~java
    // Referencia a un constructor
    String::new

    // Referencia a una metodo estatico
    String::valueOf

    // Referencia a un metodo de instancia (no estatico) de un objeto ya existente
    str::contains

    // Referencia a un metodo de instancia de un objeto de un tipo particular
    String::contains
    ~~~

2. Tiene una clase con un constructor y una instancia de esa clase, seleccione todas las referencias de metodos correctas.

    ~~~java
    class Clazz {
        int magic;

        public Clazz(int magic) {
            this.magic = magic;
        }

        public static int staticMethod(int a) {
            return a + a;
        }

        public int instanceMethod(int b) {
            return b * magic;
        }
    }

    Clazz::instanceMethod
    instance::instanceMethod
    Clazz::staticMethod
    Clazz::new
    ~~~

3. Escribe la referencia al metodo estatico **parseInt** de **Integer**.

    - **Integer::parseInt**

4. Seleccione las referencias a metodos correctas.

    - **ClassName::staticMethodName**
    - **this::nonStaticMethodName**

5. Tiene la clase **Article**, escribe el metodo de referencia de **getAuthors** sin una instancia.

    ~~~java
    class Article {
    
        String name;
        String text;
        String[] authors;
    
        public String[] getAuthors() {
            return authors;
        }
    
        // other getters and setters
    }

    // Article::getAuthors
    ~~~

6. Seleccione todas las referencias de metodos sintacticamente correctas.

    - **object::instanceMethodName**
    - **ClassName::staticMethodName**

7. Debe crear un objeto que represente un funcion de comparacion y asignarlo a la variable **comparator**. La funcion de comparacion dede tomar 2 parametros y devolver el minimo de ellos si la matriz debe ordernarse en orden ascendente, y el maximo de ellos si la matriz debe ordenarse en orden ascendente.
    
    ~~~java
    public class Main {

        public static void main(String[] args) {
            Scanner scanner = new Scanner(System.in);
            boolean isAscending = "ascending".equals(scanner.nextLine());
            int[] array = Arrays.stream(scanner.nextLine().split(" "))
                .mapToInt(Integer::parseInt)
                .toArray();
            BiFunction<Integer, Integer, Integer> comparator = null;

            comparator = isAscending ? Integer::min : Integer:: max;

            sort(array, comparator);
            Arrays.stream(array).forEach(e -> System.out.print(e + " "));
        }

        public static void sort(int[] array, BiFunction<Integer, Integer, Integer> comparator) {
            for (int i = 0; i < array.length - 1; i++) {
                for (int j = 0; j < array.length - i - 1; j++) {
                    if (comparator.apply(array[j], array[j + 1]) == array[j + 1]) {
                        int temp = array[j];
                        array[j] = array[j + 1];
                        array[j + 1] = temp;
                    }
                }
            }
        }
    }
    ~~~

8. Escribe la referencia al metodo **toRadians** de la clase **Math**.

    - **Math::toRadians**

9. Tiene una clase **Journal**, escrina la referencia a sus constructor.

    ~~~java
    class Journal {

        String title;
        String publisher;

        public Journal() { }

        // getters and setters
    }

    // Journal::new
    ~~~

10. Reemplace la expresion lambda con una referencia del metodo.

    ~~~java
    Function<String, String> toUpperCase = s -> s.toUpperCase();
    String::toUpperCase
    ~~~
