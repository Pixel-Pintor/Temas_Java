# La interfaz Map

En algunas situaciones, necesita almacenar pares de objetos asociados. Por ejemplo, al contar el numero de palabras en un texto, el primero es una palabra y el segundo es el numero de sus ocurrencias en el texto. Hay un tipo especial de colecciones llamado *mapa* para almacenar de manera efectiva tales pares de objetos.  
Un *mapa* es una coleccion de pares clave-valor. Las claves siempre son unicas, mientras que los valores pueden repetirse.  
Un buen ejemplo en el mundo es una guia telefonica donde las claves son los nombres de tus amigos y los valores son los telefonos asociados a ellos.  
El *framework Collections** provee la interface **Map<K, V>** para representar un *mapa* como un tipo de datos abstracto. Aqui, **K** es un tipo de llave, y **V** es un tipo de valor asociado. La interfaz **Map** no es un subtipo de **Collection**, pero los mapas son considerados colecciones.  
La interfaz declara muchos metodos para trabajar con mapas. Algunos de los metodos son similares a los metodos de **Collection**, mientras que otros son exclusivos de los mapas.

1. *Metodos de Coleccion*:

    - **int size()** retorna el numero de elementos en el mapa.
    - **boolean isEmpty()** retorna **true** si el mapa no contiene elementos si no retorna **false**.
    - **void clear()** elimina todos los elementos del mapa.

2. *Procesamiento de claves y valores:*

    - **V put(K key, V value)** asocia el valor especificado con la clave especificada y retorna el valor previamente asociado con la clave o **null**.
    - **V get(Object key)** retorna el valor asociado con la clave, o **null** de lo contrario.
    - **V remove(Object key)** elimina la asignacion de un **key** del mapa.
    - **boolean containsKey(Object key)** retorna **true** si el mapa contiene el **key** especificado.
    - **boolean containsValue(Object value)** retorna **true** si el mapa contiene el **value** especificado.

3. *Metodos avanzados:*

    - **V putIfAbsent(K key, V value)** pone un par si la clave especificada no esta ya asociados con un valor (o esta asignada a **null**) y retorna **null**, de lo contrario, retorna el valor actual.
    - **V getOrDefault(Object key, V defaultValue)** retorna el valor al que se asigna la clave espeficada, o **defaultValue** si este mapa no contiene ningun mapeo para la clave.

4. *Metodos que retornan otras colecciones:*

    - **Set<K> keySet()** retorna un **Set** de las claves contenidas en el mapa.
    - **Collection<V> values()** retornar una **Collection** de los valores contenidos en este mapa.
    - **Set<Map.Entry<K, V>> entrySet()** retorna un **Set** de las entradas (asociaciones) contenidas en este mapa.

Para comenzar a usar un mapa, debe crear una instancia de una de sus implementaciones: **HashMap**, **TreeMap** y **LinkedHashMap**. Usan diferentes reglas para ordenar elementos y tienen algunos metodos adicionales. Tambien existen *inmutables* cuyos nombres no son importantes para los programadores.

## Mapas inmutables

La forma mas sencilla de crear un *mapa* es invocar el metodo **of** de la interfaz **Map**. El metodo toma cero o cualquier numero par de argumentos en el formato **key1, value1, key2, value2, ...** y retorna una *mapa inmutable*.

~~~java
Map<String, String> emptyMap = Map.of();

Map<String, String> friendPhones = Map.of(
        "Bob", "+1-202-555-0118",
        "James", "+1-202-555-0220",
        "Katy", "+1-202-555-0175"
);
~~~

Ahora consideremos algunas operaciones que se puedan aplicar a mapa inmutable **friendPhones**.  
El tamaño de un mapa es igual al numero de pares contenidos en el.

~~~java
System.out.println(emptyMap.size()); // 0
System.out.println(friendPhones.size()); // 3
~~~

Es posible obtener un valor de un mapa por su clave.

~~~java
String bobPhone = friendPhones.get("Bob"); // +1-202-555-0118
String alicePhone = friendPhones.get("Alice"); // null
String phone = friendPhones.getOrDefault("Alex", "Unknown phone"); // Unknown phone
~~~

Tambien es posible verificar si un mapa contiene una clave o valor particular usando los metodos **containsKey** y **containsValue**. Podemos acceder directamente al conjunto de claves y coleccion de valores desde un mapa.

~~~java
System.out.println(friendPhones.keySet()); // [James, Bob, Katy]
System.out.println(friendPhones.values()); // [+1-202-555-0220, +1-202-555-0118, +1-202-555-0175]
~~~

Dado que es *inmutable*, solo funcionaran los metodos que no cambien los elementos de este mapa. Otros lanzaran una excepcion. **UnsupportedOperationException**, si desea poner o quitar elementos.

## HashMap

Los **HashMap** representan una mapa respaldado por una *tabla hash*. Esta implementacion proporciona un rendimiento de tiempo constante para metodos **get** y **put** que suponen que la funcion hash dispersa los elementos correctamente entre los cubos.  
El siguiente ejemplos muestra un mapa de productos donde la clave es el codigo del productos y el valor es el nombre.

~~~java
Map<Integer, String> products = new HashMap<>();

products.put(1000, "Notebook");
products.put(2000, "Phone");
products.put(3000, "Keyboard");

System.out.println(products); // {2000=Phone, 1000=Notebook, 3000=keyboard}
System.out.println(products.get(1000)); // Notebook
products.remove(1000);
System.out.println(products.get(1000)); // null
products.putIfAbsent(3000, "Mouse"); // no cambia el elemento actual
System.out.println(products.get(3000)); // Keyboard
~~~

## LinkedHashMap

El **LinkedHashMap** almacena el orden en que se insertaron los elementos. Veamos de nuevo una parte del ejemplo anterior:

~~~java
Map<Integer, String> products = new LinkedHashMap<>();

products.put(1000, "Notebook");
products.put(2000, "Phone");
products.put(3000, "Keyboard");

System.out.println(products); // el orden siempre es el mismo {1000=Notebook, 2000=Phone, 3000=Keyboard}
~~~

En este codigo, el orden de los pares es siempre el mismo y coincide con el orden en que se insertan en el mapa.

## TreeMap

El **TreeMap** representa un mapa que nos da garantias sobre el orden de los elementos. Corresponde al orden de clasificacion de las claves determinado ya sea por su orden natural (si implementan la interfaz **Comparable**) o por una implementacion especifica de **Comparator**.  
Esta clase implementa la interfaz **SortedMap** que amplia interfaz base **Map**. Proporciona algunos metodos nuevos, relacionados con las comparaciones de claves.

- **Comparator<? super K> comparator()** retorna el comparador utilizado para ordenar elementos en el mapa o **null** si el mapa utiliza el ordenamiento natural de sus claves.
- **E firstKey()** retorna la primera clave (la mas baja) del mapa.
- **E lastKey()** retorna la ultima clave (mas alta) en el mapa.
- **SortedMap<K, V> headMap(K toKey)** retorna un submapa que contiene elementos cuyas claves son estrictamente menores o iguales que **toKey**.
- **SortedMap<K, V> tailMap(K fromKey)** retorna una submapa que contiene elementos cuyas claves son estrictamente mayores o iguales a **fromKey**.
- **SortedMap<K, V> subMap(K fromKey, E toKey)** retorna un submapa que contiene elementos cuyas claves estan dentro del rango **fromKey** a **toKey** (exclusivo).

El siguiente ejemplo muestra como crear y utilizar un objeto de **TreeMap**. Este mapa esta lleno de eventos, cada uno de ellos tiene una fecha (clave) y un titulo (valor).

~~~java
SortedMap<LocalDate, String> events = new TreeMap<>();

events.put(LocalDate.of(2017, 6, 6), "The Java Conference");
events.put(LocalDate.of(2017, 6, 7), "Another Java Conference");
events.put(LocalDate.of(2017, 6, 8), "Discussion: career or education?");
events.put(LocalDate.of(2017, 6, 9), "The modern art");
events.put(LocalDate.of(2017, 6, 10), "Coffee master class");

LocalDate fromInclusive = LocalDate.of(2017, 6, 8);
LocalDate toExclusive = LocalDate.of(2017, 6, 10);

System.out.println(events.subMap(fromInclusive, toExclusive));

// El codigo genera el siguiente submapa
{2017-06-08=Discussion: career or education?, 2017-06-09=The modern art}
~~~

Use **TreeMap** solo cuando realmente necesita el orden de clasificacion de los elementos, ya que esta implementacion es menos eficiente que **HashMap**.

## Iterar sobre mapas

Es imposible iterar directamente sobre un mapa ya que no implementa la interfaz **Iterable**. Afortunadamente, algunos metodos de mapas retornan otras colecciones que son iterables. El orden de los elementos al iterar depende de la implementacion concreta de la interfaz **Map**.  
El siguiente codigo muestra como obtener claves y valores en un bucle *for-each*:

~~~java
Map<String, String> friendPhones = Map.of(
        "Bob", "+1-202-555-0118",
        "James", "+1-202-555-0220",
        "Katy", "+1-202-555-0175"
);

// imprimiendo nombrs
for (String name : friendPhones.keySet()) {
    System.out.println(name);
}

// imprimiendo telefonos
for (String phone : friendPhones.values()) {
    System.out.println(phone)
}
~~~

Si desea imprimir una clave y su valor asociado en la misma iteracion, puede obtener el **entrySet()** e iterar sobre el.

~~~java
for (var entry : friendPhones.entrySet()) {
    System.out.println(entry.getKey() + ": " + entry.getValue());
}

// imprime todos los pares
Bob: +1-202-555-0118
James: +1-202-555-0220
Katy: +1-202-555-0175
~~~

Usamos **var** desde Java 10 pero no es necesario, puede utilizar una declaracion explicita de datos como **Map.Entry<String, String>**.  
Se puede lograr el mismo comportamiento usando una expresion lambda con dos argumentos.

~~~java
friendPhones.forEach((name, phone) -> System.out.println(name + ": " + phone));
~~~

## Otras colecciones como valores

Es posible guardar otras colecciones como valores en mapas desde que las colecciones tambien sean objetos. Aqui hay un ejemplo con un mapa de sinonimos.

~~~java
Map<String, Set<String>> synonyms = new HashMap<>();

synonyms.put("Do", Set.of("Execute"));
synonyms.put("Make", Set.of("Set", "Attach", "Assign"));
synonyms.put("Keep", Set.of("Hold", "Retain"));

// {Keep=[Hold, Retain], Make=[Attach, Assign, Set], Do=[Execute]}
System.out.println(synonyms);
~~~

Almacenar colecciones como claves de un mapa, por otro lado, no es un caso comun y tiene algunas restricciones. Dichas claves deben estar representadas por *inmutables*.

## Igualdad de Mapas

Dos mapas se consideran iguales si contienen las mismas claves y valores. Los tipos de mapas no son importantes. Entonces, los siguientes mapas son completamente iguales.

~~~java
Map<String, Integer> namesToAges1 = Map.of("John", 30, "Alice", 28);
Map<String, Integer> namesToAges2 = new HashMap<>();

namesToAges2.put("Alice", 28);
namesToAges2.put("John", 30);

System.out.println(Objects.equals(namesToAges1, namesToAges2)); // true
~~~

Pero los siguientes mapas son diferenes ya que el segundo mapa no incluye a "Alice":

~~~java
Map<String, Integer> namesToAges1 = Map.of("John", 30, "Alice", 28);
Map<String, Integer> namesToAges2 = Map.of("John", 30);

System.out.println(Objects.equals(namesToAges1, namesToAges2)); // false
~~~

---

## Ejercicios

1. Implemente estos dos metodos para una matriz dada de String:

    - **wordCount** que retorna un **SortedMap<String, Integer>** donde las claves son todas cadenas diferentes y los valores son el numero de veces que esa cadena aparece en la matriz. El metodo toma una matriz de cadenas como entrada.
    - **printMap** que imprime todas las entradas del mapa (*key : value*).

    ~~~java
    class MapUtils {

        public static SortedMap<String, Integer> wordCount(String[] strings) {
            SortedMap<String, Integer> letterCount = new TreeMap<>();
            for (String letter : strings) {
                if (letterCount.containsKey(letter)) {
                    letterCount.put(letter, letterCount.get(letter) + 1);
                } else {
                    letterCount.put(letter, 1);
                }
            }
            return letterCount;
        }

        public static void printMap(Map<String, Integer> map) {
            for (Map.Entry<String, Integer> entry : map.entrySet()) {
                System.out.printf("%s : %d%n", entry.getKey(), entry.getValue());
            }
        }
    }

    public class Main {
        public static void main(String[] args) {
            Scanner scanner = new Scanner(System.in);
            String[] words = scanner.nextLine().split(" ");
            MapUtils.printMap(MapUtils.wordCount(words));
        }
    }
    ~~~

2. Implemente el metodo **mapShare**, si la clave es "a" y tiene un valor, establezca ese mismo valor en la clave "b". En cualquier caso borre la clave "c", dejando el resto del map sin modificar.

    ~~~java
    class MapUtils {

        public static void mapShare(Map<String, String> map) {
            map.remove("c");
            map.put("b", map.getOrDefault("a", map.get("b")));
        }
    }

    public class Main {

        public static void main(String[] args) {
            Scanner scanner = new Scanner(System.in);
            Map<String, String> map = new HashMap<>();
            while (scanner.hasNextLine()) {
                String s = scanner.nextLine();
                String[] pair = s.split(":");
                map.put(pair[0], pair[1]);
            }
            MapUtils.mapShare(map);
            map.forEach((key, value) -> System.out.printl(key + ":" + value));
        }
    }
    ~~~

3. Escribe un program que lea pares clave valor y genere pares cuyas claves pertenecen al rango especificado (inclusive) en orden ascendente por la clave.

    ~~~java
    class Main {

        public static void main(String[] args) {

            Scanner scanner = new Scanner(System.in);
            Map<Integer, String> map = new HashMap<>();
            int leftBottom = scanner.nextInt();
            int rightBottom = scanner.nextInt();
            int count = scanner.nextInt();

            for (int i = 0; i < count; i++) {
                map.put(scanner.nextInt(), scanner.next());
            }

            for (int j = leftBottom; j <= rightBottom; j++) {
                if (map.containsKey(j)) {
                    System.out.println(j + " " + map.get(j));
                }
            }
        }
    }
    ~~~

4. Escriba un programa que averigue si dos palabras son anagramas o no. Si una palabra es un anagrama de otra, escriba "si", de lo contrario, escriba "no".

    ~~~java
    class Main {
        public static void main(String[] args) {
            Scanner scanner = new Scanner(System.in);
            String[] firstWord = scanner.nextLine().toLowerCase().split("");
            String[] secondWord = scanner.nextLine().toLowerCase().split("");
            TreeMap<String, Integer> firstMap = new TreeMap<>();
            TreeMap<String, Integer> secondMap = new TreeMap<>();

            for (String string : firstWord) {
                firstMap.put(string, firstMap.getOrDefault(string, 0) + 1);
            }

            for (String string : secondWord) {
                secondMap.put(string, secondMap.getOrDefault(string, 0) + 1);
            }

            System.out.println(firstMap.equals(secondMap) ? "yes" : "no");
        }
    }
    ~~~

5. Modifique y retorne el mapa dado de la siguiente manera:

    - Si la primera clave es par, retorne el submapa desde la **firstKey** inclusive hasta **firstKey + 4** inclusive en orden ascendente.
    - De lo contrario, retorne el submapa desde **lastKey - 4** inclusive hasta **lastKey** inclusive en orden ascendente.

    ~~~java
    class MapUtils {
        
        private static final int FOUR = 4;
        private static final int TWO = 2;

        public static Map<Integer, String> getSubMap(TreeMap<Integer, String> map) {
            Map<Integer, String> reversedMap = new TreeMap<>(Collections.reverseOrder());

            if (map.firstKey() % TWO != 0) {
                reversedMap.putAll(map.subMap(map.firstKey(), true, map.firstKey() + FOUR, true));
            } else {
                reversedMap.putAll(map.subMap(map.lastKey() - FOUR, true, map.lastKey(), true));
            }

            return reversedMap;
        }
    }

    public class Main {

        public static void main(String[] args) {
            Scanner scanner = new Scanner(System.in);

            TreeMap<Integer, String> map = new TreeMap<>();
            Arrays.stream(scanner.nextLine().split("\\s")).forEach(s -> {
                String[] pair = s.split(":");
                map.put(Integer.parseInt(pair[0]), pair[1]);
            });

            Map<Integer, String> res = MapUtils.getSubMap(map);
            res.forEach((k, v) -> System.out.println(k + " : " + v));
        }
    }
    ~~~

6. Escriba un metodo que obtenga dos lineas de palabras, averigue si puede escribir una nota de rescate con las palabras disponibles en las noticias.

    ~~~java
    class Main {

        public static void main(String[] args) {

            Scanner scanner = new Scanner(System.in);
            String[] newsArray = scanner.nextLine().split(" ");
            String[] noteArray = scanner.nextLine().split(" ");

            boolean getMoney = NoteUtils.composeNote(
                NoteUtils.wordMap(newsArray), NoteUtils.wordMap(noteArray)
            );

            NoteUtils.printResult(getMoney);
        }
    }

    class NoteUtils {

        protected static Map<String, Integer> wordMap(String[] strArray) {
            for (String str : strArray) {
                Integer i = map.get(str);
                map.put(str, i == null ? 1 : i + 1);
            }
            return map;
        }

        protected static boolean composeNote(Map<String, Integer> map1, Map<String, Integer> map2) {
            boolean isComposable = true;
            for (Map.Entry<String, Integer> entry : map2.entrySet()) {
                isComposable = map1.containsKey(entry.getKey()) && map1.get(entry.getKey()).intValue() >= entry.getValue().intValue();
                if (!isComposable) {
                    break;
                }
            }
            return isComposable;
        }

        protected static void printResult(boolean getMoney) {
            System.out.println(getMoney ? "You get money" : "You are busted");
        }
    }
    ~~~

7. Crear un **TreeMap** y llenelo con pares clave valor.

    ~~~java
    public class Main {

        public static void main(String[] args) {    
            SortedMap<String, Integer> map = new TreeMap<>();
            map.put("Omega", 24);
            map.put("Alpha", 1);
            map.put("Gamma", 3);
            System.out.println(map);
        }
    }
    ~~~

8. Cuales son los valores despues de ejecutar el codigo.

    ~~~java
    Map<String, Integer> map = new HashMap<>();

    map.put("one", 1);
    map.put("two", 2);
    map.put("three", 3);
    map.put("four", 4);

    Integer val4 = map.get("four"); // 4
    Integer val5 = map.getOrDefault("five", 5); // 5
    Integer val6 = map.get("six"); // null
    ~~~

9. Muestra cada par clave-valor del mapa dado en el bucle, cada par en una nueva linea.

    ~~~java
    