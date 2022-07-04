# La interfaz Set
Cuando necesitamos mantener solo elementos unicos dentro de una coleccion, para deshacernos de los duplicados en una secuencia, o si tiene la intencion de realizar operaciones matematicas, puede usar **Set**.  
El marco **Collections** proporciona la interfaz `Set<E>` para representar un **conjunto** como un tipo de datos abstracto. Hereda todos los metodos de la interfaz `Collection<E>` pero no agrega ninguna nueva. Los metodos mas utilizados incluyen `contains`, `add`, `addAll`, `removeAll`, `size`, y otros que hemos considerado en el tema anterior sobre el marco de colecciones.  
Un metodo merece especial atencion, `retainAll(Collection<E> coll)` conserva solo los elementos del conjunto que estan contenidos en la coleccion especificada.  
Para comenzar a usar un conjunto, debe crear una instancia de una de las implementaciones: `HashSet`, `TreeSet` y `LinkedHashSet`. Estos son conjuntos mutables y usan diferentes reglas para ordenar elementos y tienen algunos metodos adicionales. Tambien hay **inmutables**, que tambien implementan la interfaz `Set<E>`.  

## Set inmutables
La forma mas sencilla de crear un **Set** es invocar el metodo `of` de la interfaz `Set`.
~~~java
Set<String> emptySet = Set.of();
Set<String> persons = Set.of("Larry", "Kenny", "Sabrina");
Set<Integer> numbers = Set.of(100, 200, 300, 400);
~~~
Devuelve una **inmutable** que contiene todos los elementos pasados o un un cojunto vacio. El orden de los elementos de **inmutables** no es fijo:
~~~java
System.out.println(emptySet); // []
System.out.println(persons);  // [Kenny, Larry, Sabrina] o diferente
System.out.println(numbers);  // [400, 200, 300, 100] o diferente
~~~
Una de las operaciones de conjuntos mas utilizadas es verificar si un conjunto contiene un elemento. Aqui hay un ejemplo:
~~~java
System.out.println(emptySet.contains("hello")); // false
System.out.println(persons.contains("Sabrina")); // true
System.out.println(persons.contains("John")); // false
System.out.println(numbers.contains(300)); // true
~~~
Para **inmutables** solo es posible invocar `contains`, `size` y `isEmpty`. Todos los demas tiraran `UnsupportedOperationException` ya que intentan cambiar el conjunto. Si desea agregar o quitar elementos, use uno de `HashSet`, `TreeSet` o `LinkedHashSet`.

## HashSet
La clase `HashSet` representa un conjunto respaldado por una **tabla hash**. Utiliza codigos hash de elementos para almacenarlos de manera efectiva. No garantiza el orden de iteracion del conjunto; en particular, no garantiza que el orden se mantenga constante en el tiempo.  
El siguiente ejemplo demuestra la creacion de `HashSet`y añadiendole paises (con un duplicado). El resultado de salida no contiene duplicados.
~~~java
Set<String> countries = new HashSet<>();

countries.add("India");
countries.add("Japan");
countries.add("Switzerland");
countries.add("Japan");
countries.add("Brazil");

System.out.println(countries); // [Japan, Brazil, Switzerland, India]
System.out.println(countries.contains("Switzerland")); // true
~~~
El `HashSet` debe ser tratado como un conjunto desordenado. No debe confiar en el orden de los elementos en este conjunto, incluso con el bucle **for-each**.  
Los `HashSet` ofrece un tiempo constante de `0(1)` de rendimiento para las operaciones basicas (`add`, `remove` y `contain`), suponiendo que la funcion hash dispersa los elementos correctamente entre los cubos.  

## TreeSet
La clase `TreeSet` representa un conjunto que nos da garantias sobre el orden de los elementos. Corresponde al orden de clasificacion de los elementos determinado ya sea por su orden natural (si implementan la interfaz `Comparable`) o por una implementacion especifica de `Comparator`.  
La clase `TreeSet` implementa la interfaz `SortedSet` que amplia la intefaz `Set`. La interfaz `SortedSet` proporciona algunos metodos nuevos relacionados con las comparaciones de elmentos:
- `Comparator<? super E> comparator()` devuelve el **comparador** utilizado para ordenar elementos en el conjunto o `null` si el conjunto utiliza la ordenacion natural de sus elementos.
- `SortedSet<E> headSet(E toElement)` retorna un subconjunto que contiene elementos que son estrictamente menores que `toElement`.
- `SortedSet<E> tailSet(E fromElement)` retorna un subconjunto que contiene elementos que son mayores o iguales a `fromElement`.
- `SortedSet<E> subSet(E fromElement, E toElement)` retorna un subconjunto que contiene elementos en el rango `fromElement` (inclusivo) a `toElement` (exclusivo).
- `E first()` retorna el primer elemento (el mas bajo) del conjunto.
- `E last()` retorna el ultimo elemento (mas bajo) del conjunto.  
El siguiente ejemplo muestra algunos de los metodos enumerados.
~~~java
SortedSet<Integer> sortedSet = new TreeSet<>();

sortedSet.add(10);
sortedSet.add(15);
sortedSet.add(13);
sortedSet.add(21);
sortedSet.add(17);

System.out.println(sortedSet); // [10, 13, 15, 17, 21]

System.out.println(sortedSet.headSet(15)); // [10, 13]
System.out.println(sortedSet.tailSet(15)); // [15, 17, 21]
 
System.out.println(sortedSet.subSet(13,17)); // [13, 15] 

System.out.println(sortedSet.first()); // minimum is 10
System.out.println(sortedSet.last());  // maximum is 21
~~~

## LinkedHashSet
La clase `LinkedHashSet` representa un conjunto con elementos vinculados. Se diferencia de `HashSet` garantizando que el orden de los elementos es el mismo que el orden en que fueron insertados en el conjunto. Reinsertar un elemento que ya esta en el `LinkedHashSet` no cambia este orden.  
En alguno sentido `LinkedHashSet` es algo intermedio entre `HashSet` y `TreeSet`. Implementado como una tabla hash con una lista enlazada que se ejecuta a traves de el, este conjunto proporciona **orden por insercion** y se ejecuta casi tan rapido como `HashSet`.
~~~java
Set<Character> characters = new LinkedHashSet<>();

for (char c = 'a'; c <= 'k'; c++) {
    characters.add(c);
}
        
System.out.println(characters); // [a, b, c, d, e, f, g, h, i, j, k]
~~~
En este codigo, el orden de los caracteres es siempre el mismo y coincide con el orden en que se insertan en el conjunto.  
Los `LinkedHashSet` ahorra el orden caotico proporcionado por `HashSet` sin incurrir en el aumento del costo de tiempo de las operaciones asociadas con `TreeSet`. Pero `LinkedHashSet` requiere mas memoria para almacenar elementos.

## Establecer operaciones
Ya has visto algunas operaciones sobre conjuntos. Ahora veamos las operaciones que generalmente se denominan **operaciones teoricas de conjuntos** que provienen de las matematicas. Es curioso que en Java sean comunes para todas las colecciones, no solo para los conjuntos.  
Aqui hay un ejemplo de tales operaciones. En primer lugar, creamos un conjunto mutable. Luego, le aplicamos operaciones cambiando los elementos:
~~~java
// obtener un conjunto mutable de uno inmutable
Set<String> countries = new HashSet<>(List.of("India", "Japan", "Switzerland"));

countries.addAll(List.of("India", "Germany", "Algeria"));
System.out.println(countries); // [Japan, Algeria, Switzerland, Germany, India]

countries.retainAll(List.of("Italy", "Japan", "India", "Germany"));
System.out.println(countries); // [Japan, Germany, India]

countries.removeAll(List.of("Japan", "Germany", "USA"));
System.out.println(countries); // [India]
~~~
Despues de realizar `addAll`, el conjunto `countries` no contiene paises duplicados. Los metodos `retainAll` y `removeAll` afectan solo a aquellos elementos que se especifican en los conjuntos pasados. Tambien es posibles utilizar cualquier clase que implemente la interfaz `Collection` para estos metodos (por ejemplo, `ArrayList`).  
En matematicas y otros lenguajes de programacion, las operacioens de conjunto demostradas se conocen como **union** (`addAll`), **interseccion** (`retainAll`) y **diferencia** (`removeAll`).  
Tambien existe un metodo que nos permite comprobar si un conjunto es un subconjunto de otros conjunto.
~~~java
Set<String> countries = new HashSet<>(List.of("India", "Japan", "Algeria"));

System.out.println(countries.containsAll(Set.of())); // true
System.out.println(countries.containsAll(Set.of("India", "Japan")));   // true
System.out.println(countries.containsAll(Set.of("India", "Germany"))); // false
System.out.println(countries.containsAll(Set.of("Algeria", "India", "Japan"))); // true
~~~
Como puede ver, este metodo retorna `true` incluso para un conjunto vacio y un conjunto que es completamente igual al conjunto inicial.

## Establecer igualdad
Por ultimo, pero no menos importante, es como se comparan los conjuntos. Dos conjuntos son iguales cunaod contienen los mismos elementos. La igualdad no depende de los tipos de conjunto en si.
~~~java
Objects.equals(Set.of(1, 2, 3), Set.of(1, 3));    // false
Objects.equals(Set.of(1, 2, 3), Set.of(1, 2, 3)); // true
Objects.equals(Set.of(1, 2, 3), Set.of(1, 3, 2)); // true

Set<Integer> numbers = new HashSet<>();

numbers.add(1);
numbers.add(2);
numbers.add(3);

Objects.equals(numbers, Set.of(1, 2, 3)); // true
~~~
---

## Ejercicios
1. Implemente un metodo que cree un conjunto de numeros separados por espacio y otros metodo que elimine todos los numeros mayores que 10 del conjunto dado.
~~~java
class SetUtils {

    final static int TEN = 10;

    public static Set<Integer> getSetFromString(String str) {
        return Stream.of(str.split(" ")).map(Integer::parseInt).collect(Collectors.toSet());
    }

    public static void removeAllNumbersGreaterThan10(Set<Integer> set) {
        set.removeIf(number -> number > TEN);
    }

}

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String numbers = scanner.nextLine();
        Set<Integer> set = SetUtils.getSetFromString(numbers);
        SetUtils.removeAllNumbersGreaterThan10(set);
        set.forEach(e -> System.out.print(e + " "));
    }
}
~~~
2. Implementar una operacion matematicas que verifique si el segundo conjunto es un **superconjunto estricto** del primero conjuntos. El segundo set debe contener todos los elementos del primero conjunto, pero los conjuntos no deben ser iguales.
~~~java
public class Main {

    private static <T> boolean isStrictSuperset(Set<T> set1, Set<T> set2) {
        boolean equalSet = Objects.equals(set1, set2);
        boolean containsSet = set2.containsAll(set1);
        return !equalSet && containsSet;
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        Set<String> set1 = readStringSet(scanner);
        Set<String> set2 = readStringSet(scanner);

        System.out.println(isStrictSuperset(set1, set2));
    }

    private static Set<String> readStringSet(Scanner scanner) {
        return Arrays
                .stream(scanner.nextLine().split("\\s+"))
                .collect(Collectors.toSet());
    }
}
~~~
3. Cuales elementos tendra el conjunto al final de las operaciones.
~~~java
Set<Integer> set = new HashSet<>(Set.of(1, 2, 3, 4));
set.retainAll(Set.of(2, 3, 4, 5));
set.removeAll(Set.of(1, 2));
System.out.println(set); // [3, 4]
~~~
4. Cuantos items tendra el conjunto al final
~~~java
Set<Integer> set = new TreeSet<>();
set.add(100);
set.add(200);
set.add(100);
set.add(100);

// al final tendra solo dos elementos [100, 200]
~~~
5. Necesita verifica si algunos elementos pertenecen a un conjunto con bastante frecuencia y no necesita ordenar los elementos. Que conjunto es el mas adecuado?. R: `HashSet`.
6. Tiene una conjunto de cadenas, imprima cada elemento en una nueva linea.
~~~java
public class Main {

    public static void main(String[] args) {
        Set<String> nameSet = new TreeSet<>(Arrays.asList("Mr.Green", "Mr.Yellow", "Mr.Red"));

        // solucion 1
        for (String name : nameSet) {
            System.out.println(name);
        }

        // solucion 2
        new TreeSet<>(List.of("Mr.Green", "Mr.Red", "Mr.Yellow")).forEach(System.out::println);
    }
}
~~~
