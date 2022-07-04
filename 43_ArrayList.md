# ArrayList
La clase `ArrayList` representa una matriz redimensionable de objetos de un tipo especificado. A diferencia de la matriz estandar, puede crecer dinamicamente despues de la adicion y contraerse despues de la eliminacion de sus elementos. Este comportamiento es muy util si no conoce el tamaño de la matriz de antemano o si necesita una que pueda cambiar de tamaño durante la vida util de un programa. No puede almacenar tipos primitivos, sin embargo puede almacenar cualquier tipo de referencia.  

## Crear un ArrayList
La forma mas sencilla es usar un constructor sin argumentos.
~~~java
ArrayList<String> list = new ArrayList<>();
~~~
La lista creada esta vacia, pero su capacidad inicial es 10 (por defecto). Tambien podemos especificar la capacidad inicial del mismo:
~~~java
ArrayList<String> list = new ArrayList<>(50);
~~~
Esta lista esta vacia, pero su capacidad inicial esta establecida en 50. O puedes construir un `ArrayList` que consta de elementos de otra lista:
~~~java
ArrayList<String> list = new ArrayList<>(anotherList);
~~~

## Metodos basicos
Tiene un conjunto de metodos convenientes que emulan y amplian la funcionalidad de las matrices estandar. Hay un metodo para determinar el tamaño de la coleccion. `size` que devuelve el numero de elementos de la lista. Intentemos aprender el tamaño de la nuestra:
~~~java
ArrayList<String> names = new ArrayList<>();
System.out.println(names.size()); // 0
~~~
Como era de esperar porque esta vacia el resultado es cero. Tambien podemos querer aprender el valor de la posicion especificada del objeto. Para eso, las colecciones tienen un `get(int index)` que devuelve el objeto de la lista que esta presente en el indice especificado.
- `add(Object o)` agrega un elemento pasado a la ultima posicion de la coleccion.
- `add(int index, Object o)` agrega un elemento pasado a la posicion especificada de la coleccion.
- `set(int index, Object o)` reemplaza el elemento presente en el indice especificado con el objeto.  
Agreguemos algunos nombres a nuestra coleccion.
~~~java
names.add("Justin");
names.add("Helen");
names.add(1, "Joshua");
names.add(0, "Laura"); // [Laura, Justin, Joshua, Helen]

// Reemplaza un nombre por otro
names.set(3, "Marie"); // [Laura, Justin, Joshua, Marire]
~~~
Finalmente, existen metodos para eliminar elementos de la coleccion.
- `remove(Object o)` elimina la **primera aparicion** del elemento especificado de la lista.
- `remove(int index)` elimina el elemento en la posicion especificada en la lista.
- `clear()` elimina todos los elementos de la coleccion.  
Intentemos eliminar elementos por valor e indice.
~~~java
names.remove("Justin");
names.remove(1);
names.clear();
~~~

## Mas metodos ArrayList
Hemos ilustrado las posibilidades de los metodos basicos para colecciones en Java aplicados a un objeto `ArrayList`. Pero esta clase tiene algunos metodos mas propios. Primero, vamos a crear otro `ArrayList`:
~~~java
ArrayList<Integer> numbers = new ArrayList<>();

numbers.add(1);
numbers.add(2);
numbers.add(3);
numbers.add(1);

// El metodo addAll(Collection c) agrega toda una coleccion
ArrayList<Integer> numbers2 = new ArrayList<>();
numbers2.add(100);
numbers2.addAll(numbers); // [100, 1, 2, 3, 1]

// contains() comprueba si una lista contiene un valor o no
// indexOf y lastIndexOf encuentra el indice de la primera y la ultima aparicion
System.out.println(numbers.contains(2)); // true
System.out.println(numbers.contains(4)); // false
System.out.println(numbers.indexOf(1)); // 0
System.out.println(numbers.lastIndexOf(1)); // 3
System.out.println(numbers.lastIndexOf(4)); // -1
~~~
Como puedes ver, esta clase proporciona un rico conjunto de metodos para trabajar con elementos. No tiene que escribirlos usted mismo, como lo hace con las matrices estandar.  

## Iterando sobre ArrayList
Es posible iterar sobre elementos de una instancia de la clase. Se hace de la misma manera que iterar sobre una matriz. En el siguiente ejemplo, usamos **for** y **for-each** para sumar las cinco primeras potencias de diez en una lista y luego imprimimos los numeros en la salida estandar.
~~~java
ArrayList<Long> powersOfTen = new ArrayList<>();

int count = 5;
for (int i = 0; i < count; i++) {
    long power = (long) Math.pow(10, i);
    powersOfTen.add(power);
}

for (Long value : powersOfTen) {
    System.out.println(value + " ");
}

// imprime
// 1 10 100 1000 10000
~~~
--- 

## Ejercicios
1. Implemente un metodo que concatena todos los numeros positivos de dos listas.
~~~java
class ConcatPositiveNumbersProblem {

    public static ArrayList<Integer> concatPositiveNumbers(ArrayList<Integer> l1, ArrayList<Integer> l2) {
        ArrayList<Integer> numbers = new ArrayList<>();
        for (int num : l1) {
            if (num >= 0) {
                numbers.add(num);
            }
        }
        for (int num : l2) {
            if (num >= 0) {
                numbers.add(num);
            }
        }
        return numbers;
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        ArrayList<Integer> list1 = readArrayList(scanner);
        ArrayList<Integer> list2 = readArrayList(scanner);

        ArrayList<Integer> result = concatPositiveNumbers(list1, list2);

        result.forEach(n -> System.out.print(n + " "));
    }

    private static ArrayList<Integer> readArrayList(Scanner scanner) {
        return Arrays
                .stream(scanner.nextLine().split("\\s+"))
                .map(Integer::parseInt)
                .collect(Collectors.toCollection(ArrayList::new));
    }
}
~~~
2. Crear una lista e introduce numeros enteros.
~~~java
public class Main {

    public static void main(String[] args) {
        ArrayList<Integer> list = new ArrayList(Arrays.asList(2, 0, 1, 7));
        System.out.println(list);
    }
}
~~~
