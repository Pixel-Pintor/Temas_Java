# La interfaz List
Las listas son el tipo mas cercano a los arreglos, excepto que su tamaño se puede cambiar dinamicamente mientras el tamaño de un arreglo esta restringido. Ademas, las listas proporcionan un comportamiento mas avanzado que las matrices.  
La interfaz `List<E>` representa una lista como un tipo de datos abstracto. Extiende la interfaz `Collection<E>` adquiriendo sus metodos y agregar algunos metodos nuevos:  
- `E set(int index, E element)` reemplaza el elemento en la posicion especificada en esta lista con el elemento especificado y devuelve el elemento que fue reemplazado.
- `E get(int index)` devuelve el elemento en la posicion especificada en la lista.
- `int indexOf(Object obj)` devuelve el indice de la primera aparicion del elemento en la lista o -1 si no existe tal elemento.
- `int lastIndexOf(Object obj)` devuelve el indice de la ultima aparicion del elemento en la lista -1 si no existe tal elemento.
- `List<E> subList(int fromIndex, int toIndex)` retorna una sublista de esta lista.  
No puede crear una instancia de la interfaz `List`, pero puede crear una instancia de uno de sus implementaciones: `ArrayList` o `LinkedList`, y luego usarla a traves de la interfaz `List`. Tendra acceso a todos los metodos declarador en ambas interfaces.  

## Listas inmutables
La forma mas sencilla de crear una lista es invocar el metodo `of` de la interfaz `List`.
~~~java
List<String> emptyList = List.of(); // 0 elementos
List<String> names = List.of("Larry", "Kenny", "Sabrina"); // 3 elementos
List<Integer> numbers = List.of(0, 1, 1, 0, 2); // 5 elementos
~~~
Devuelve una lista **inmutable** que contiene todos los elementos pasados o una lista vacia. Usar este metodo es conveniente cuando se crean constantes de lista o se prueba alguno codigo.
~~~java
List<String> daysOfWeek = List.of(
    "Monday",
    "Tuesday",
    "Wednesday",
    "Thursday",
    "Friday",
    "Saturday",
    "Sunday"
);

System.out.println(daysOfWeek.size()); // 7
System.out.println(daysOfWeek.get(1)); // Tuesday
System.out.println(daysOfWeek.indexOf("Sunday")); // 6

List<String> weekDays = daysOfWeek.subList(0, 5); 
System.out.println(weekDays); // [Monday, Tuesday, Wednesday, Thursday, Friday]
~~~
Dado que es **inmutable**, solo funcionaran los metodos que no cambien los elementos de la lista. Otros lanzan una excepcion.
~~~java
daysOfWeek.set(0, "Funday"); // arroja una excepcion
daysOfWeek.add("Holiday"); // arroja una excepcion
~~~
Antes de Java 9, otra forma de crear listas no modificables era la siguiente:
~~~java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
~~~

## Listas mutables
Cuando necesite usar una lista mutable, puede tomar una de las dos implementaciones mutables de uso comun de la interfaz `List`.  
La clase `ArrayList`. Representa una matriz redimensionable. Ademas de implementar la interfaz `List`, proporciona metodos para manipular el tamaño de la matriz que se utiliza internamente. Estos metodos no son necesarios en los programas a menudo, por lo que es mejor utilizar un objeto de esta clase a traves de la interfaz `List`.
~~~java
List<Integer> numbers = new ArrayList<>();

numbers.add(15);
numbers.add(10);
numbers.add(20);

System.out.println(numbers); // [15, 10, 20]
numbers.set(0, 30); // sin excepciones
System.out.println(numbers); // [30, 10, 20]
~~~
Si tiene una lista inmutable, puede tomar la version mutable usando el siguiente codigo.
~~~java
List<String> immutableList = Arrays.asList("one", "two", "three");
List<String> mutableList = new ArrayList<>(immutableList);
~~~
Otra implementacion mutable es la clase `LinkedList`. Representa una lista **doblemente enlazada** basada en nodos conectados. Todas las operaciones que indexan la lista recorreran la lista desde el principio o desde el final, lo que este mas cerca del indice especificado.
~~~java
List<Integer> numbers = new LinkedList<>();

numbers.add(10);
numbers.add(20);
numbers.add(30);

System.out.println(numbers); // [10, 20 ,30]
~~~
El acceso al primero y el ultimo elemento de la lista se realiza siempre en tiempo constante `0(1)` porque los enlaces se almacenan permanentemente en el primer y el ultimo elemento, por lo que agregar un elemento al final de la lista no significa que tenga que iterar toda la lista en busca del ultimo elemento. Pero acceder/establecer un elemento por su indice toma `0(n)` tiempo para una lista enlazada.

## Iterando sobre una lista
No hay problemas para iterar sobre los elementos de una lista.
~~~java
List<String> names = List.of("Larry", "Kenny", "Sabrina");

// usando un for-each
for (String name : names) {
    System.out.println(name);
}

// usando indices y el metodo size()
for (int i = 0; i < names.size(); i += 2) {
    System.out.println(names.get(i));
}
~~~

## Igualdad de lista
La pregunta final es como se comparan las listas. Dos listas son iguales cuando contienen los mismos elementos en el mismo orden. La igualdad no depende de los tipos de las listas en si.
~~~java
Objects.equals(List.of(1, 2, 3), List.of(1, 2, 3)); // true
Objects.equals(List.of(1, 2, 3), List.of(1, 3, 4)); // false
Objects.equals(List.of(1, 2, 3), List.of(1, 2, 3, 1));

List<Integer> numbers = new ArrayList<>();

numbers.add(1);
numbers.add(2);
numbers.add(3);

Objects.equals(numbers, List.of(1, 2, 3)); // true
~~~
---

## Ejercicios
1. Implemente el metodo `changeList` que encuentre la palabra mas larga, reemplace todas las palabras con esa palabra.
~~~java
public class Main {

    static void changeList(List<String> list) {
        String word = "";
        for (String str : list) {
            if (str.length() > word.length()) {
                word = str;
            }
        }

        for (int i = 0; i < list.size(); i++) {
            list.set(i, word);
        }
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String s = scanner.nextLine();
        List<String> lst = Arrays.asList(s.split(" "));
        changeList(lst);
        lst.forEach(e -> System.out.print(e + " "));
    }
}

// metodo alternativo
static void changeList(List<String> list) {
    String longestString = Collections.max(list, Comparator.comparingInt(String::length));
    list.replaceAll(str -> longestString);
}

// metodo alternativo
static void changeList(List<String> list) {
    String maxWord = list.stream()
            .max(Comparator.comparing(String::length))
            .get();
    list.replaceAll(word -> maxWord);
}
~~~
2. Escriba un programa que lea la lista de numeros enteros separados por espacios de la entrada estandar y luego elimine todos los numeros con **indices** (0, 2, 4, etc). Despues de eso, el programa debe generar la secuencia resultante en el **orden inverso**.
~~~java
class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String str = scanner.nextLine();
        List<String> list = Arrays.asList(str.split(" "));
        list = filterList(list);
        list.forEach(num -> System.out.print(num + " "));
    }

    static List<String> filterList(List<String> list) {
        List<String> newList = new ArrayList<>();
        for (int i = 0; i < list.size(); i++) {
            if (i % 2 != 0) {
                newList.add(list.get(i));
            }
        }
        Collections.reverse(newList);
        return newList;
    }
}
~~~
3. Implemente un metodo para dividir una lista generica en sublistas. El metodo toma dos argumentos: una lista generica y un tamaño de sublistas. El tamaño especificado de las sublistas puede ser mayor que el tamaño de la lista dada. Cada sublista, excepto la ultima, debe tener el tamaño especificado. La ultima sublista puede tener un numero menor de elementos.
~~~java
class ListUtils {

    /**
     * It splits the passed list into a sequence of sublists with a predefined size 
     */
    public static <T> List<List<T>> splitListIntoSubLists(List<T> list, int subListSize) {
        List<List<T>> sublists = new ArrayList<>();

        for (int i = 0; i < list.size(); i += subListSize) {
            sublists.add(list.subList(i, Math.min(i + subListSize, list.size())));
        }

        return sublists;
    }
}

public class Main {
    public static void main(String[] args) {
        final Scanner scanner = new Scanner(System.in);

        final String[] values = scanner.nextLine().split("\\s+");

        final List<Integer> list = Arrays.asList(values).stream()
                .map(Integer::parseInt)
                .collect(Collectors.toList());

        final int subListSize = Integer.parseInt(scanner.nextLine());

        final List<List<Integer>> subLists = ListUtils.splitListIntoSubLists(list, subListSize);

        subLists.forEach(subList -> {
            final String representation = subList.stream().map(Object::toString).collect(Collectors.joining(" "));
            System.out.println(representation);
        });
    }
}
~~~
4. Encuentre las declaraciones correctas.
~~~java
List<Integer> list = new ArrayList<>();
LinkedList<Integer> list3 = new LinkedList<>(new ArrayList<>());
Collection<Integer> list4 = new ArrayList<>();
ArrayList<Integer> list5 = new ArrayList<>();
~~~
5. Que pasa si cambia `ArrayList` a `LinkedList`.
~~~java
List<Integer> list = new ArrayList<>();

list.add(1);
list.add(2);
list.add(3);
list.add(2);

for (Integer number : list) {
    System.out.println(number);
}

// no pasa nada el codigo funciona perfectamente
~~~
6. Tiene cuatro listas, verifique cuales comparaciones retornan `true`.
~~~java
List<Integer> list1 = new ArrayList<>();
list1.add(1);
list1.add(2);
list1.add(3);

List<Integer> list2 = new LinkedList<>();
list2.add(3);
list2.add(2);
list2.add(1);

List<Integer> list3 = List.of(1, 2, 3);
List<Integer> list4 = List.of(1, 1, 2, 2, 3, 3);

list1.equals(list1); // true
list1.equals(list2); // false
list1.equals(list3); // true
list2.equals(list3); // false
list3.equals(list4); // false
~~~
7. Encuentre las listas mutables y las inmutables.
~~~java
List<String> list = new LinkedList<>(); // mutable
List<String> list = List.of(); // inmutable
List<String> list = new ArrayList<>(); // mutable
List<String> list = Arrays.asList(); // inmutable
~~~
8. Tienes una lista de objetos `GreekLetter`, imprima cada elemento de esta lista en una nueva linea.
~~~java
public class Main {

    public static void main(String[] args) {
        List<GreekLetter> letterList = new ArrayList<>();

        final int gamma = 3;
        final int omega = 24;
        final int alpha = 1;

        letterList.add(new GreekLetter("Gamma",  gamma));
        letterList.add(new GreekLetter("Omega", omega));
        letterList.add(new GreekLetter("Alpha",  alpha));

        for (GreekLetter greekLetter : letterList) {
            System.out.println(greekLetter);
        }
    }

    static class GreekLetter {

        private final String letter;
        private final Integer position;

        public GreekLetter(String letter, Integer position) {
            this.letter = letter;
            this.position = position;
        }

        @Override
        public String toString() {
            return "{" +
                    "letter='" + letter + '\'' +
                    ", position=" + position +
                    '}';
        }
    }
}
~~~
