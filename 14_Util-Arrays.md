# Java.Util.Arrays
La clase llamada `Arrays` pertenece al paquete `java.util` y nos proporciona un grupo de metodos estaticos que tienen varias sobrecargas para aceptar diferentes tipos. Estos son los metodos mas basicos:
- `Arrays.toString()`
- `Arrays.equals()`
- `Arrays.compare()`
~~~java
// uso de toString()
byte[] arr = { 0, 1, 2, 4, 8, 16, 32, 64 };
String arrayAsString = Arrays.toString(arr); // [0, 1, 2, 4, 8, 16, 32, 64]

System.out.println(arrayAsString);

// equals() verifica la igualdad de las matrices
byte[] arr1 = {1, 2, 3};
byte[] arr2 = {1, 2, 3};
byte[] arr3 = {2, 1, 3};

System.out.println(Arrays.equals(arr1, arr2)); // true
System.out.println(Arrays.equals(arr2, arr3)); // false

/*
compare() compara matrices lexicograficamente
Si la primera matriz es menor o mayor retorna -1 o 1
Si las matrices son iguales retorna 0
*/
byte[] arr1 = {0, 1, 2};
byte[] arr2 = {1, 2, 3};
byte[] arr3 = {1, 2, 3};
byte[] arr4 = {0, 1, 2};

System.out.println(Arrays.compare(arr1, arr2)); // -1
System.out.println(Arrays.compare(arr2, arr3)); // 0
System.out.println(Arrays.compare(arr3, arr4)); // 1
~~~
Estos son los metodos que se utilizan para copiar elementos de una matriz en una nueva matriz:
- `Arrays.copyOf()`
- `Arrays.copyOfRange()`  
`Arrays.copyOf()` toma dos argumentos: la matriz copiada a partir del primer elemento y la longitud de la nueva matriz. Si el valor del segundo argumento es mayor que la matriz original, los elementos faltantes se llenaran con ceros.
~~~java
byte[] arr1 = {1, 2, 3};
byte[] arr2 = Arrays.copyOf(arr1, 3);
byte[] arr3 = Arrays.copyOf(arr1, 4);

System.out.println(Arrays.toString(arr2)); // [1, 2, 3]
System.out.println(Arrays.toString(arr3)); // [1, 2, 3, 0]
~~~
`Arrays.copyOfRange()` funciona de la misma manera, pero aqui puede especificar el indica del elemento desde el que copiar la matriz. Este metodo tambien llena la matriz con ceros si la longitud de la nueva matriz se especifica como mayor que la que se esta copiando:
~~~java
byte[] arr1 = {1, 2, 3};
byte[] arr2 = Arrays.copyOfRange(arr1, 1, 3);
byte[] arr3 = Arrays.copyOfRange(arr1, 1, 4);

System.out.println(Arrays.toString(arr2)); // [2, 3]
System.out.println(Arrays.toString(arr3)); // [2, 3, 0]
~~~
En este metodo, especifica el indice del primero elemento inclusive y el indice del ultimo elemento exclusivamente.  
El siguiente par de metodos importantes estan diseñador para clasificar una matriz y buscar un elemento:
- `Arrays.sort()`
- `Arrays.binarySearch()`
Al usar `sort()` puede especificar el rango de elementos que se ordenaran.
~~~java
byte[] arr = {3, 1, 2};
Arrays.sort(arr);

System.out.println(Arrays.toString(arr)); [1, 2, 3]
~~~
Que pasa si no pasamos tipos base sino nuestras clases definidas por el usuario? De forma predeterminada, el metodo no sabe como ordenar los tipos personalizados. En este asunto, debemos usar la interfaz `Comparator`.
~~~java
public class ComparatorDemo {
    public static void main(String[] args) {
        Person[] persons = {new Person(40), new Person(30), new Person(20)};
        Arrays.sort(persons, new PersonComparator());

        System.out.println(Arrays.toString(persons));
    }
}

class Person {
    private int age;

    public Person(int age) {
        this.age = age;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Person{" +
                "age=" + age +
                '}';
    }
}

class PersonComparator implements Comparator<Person> {
    @Override
    public int compare(Person p1, Person p2) {
        return Integer.compare(p1.getAge(), p2.getAge());
    }
}
~~~
Lo que tienes que hacer es simplemente implementar el metodo `compare()` de la interfaz `Comparator` y pasar una instancia de la clase requerida como segundo argumento para `Arrays.sort()`. Ahora que sabe como ordenar una matriz, practiquemos con el metodo `binarySearch()`.
~~~java
byte[] arr = {3, 1, 2};
byte key = 3;
Arrays.sort(arr);

System.out.println("The index is: " + Arrays.binarySearch(arr, key)); // 2
~~~
Necesita dos argumentos: el primero es la matriz en si y el segundo es el valor a buscar. A cambio, obtendra el indice del elemento especificado. Recuerda que este algoritmo no funciona correctamente con elementos duplicados. En tales casos, no hay garantia de que cual elemento se encontrara.  
Tambien tenermos metodos para convertir matrices.
- `Arrays.asList()`
- `Arrays.stream()`  
El metodo `Arrays.asList()` convierte un matriz en una lista.
~~~java
Byte[] arr = {2, 3, 4};
List<Byte> list = Arrays.asList(arr);
~~~
Este metodo no acepta matrices de tipo primitivo. Deberia usar sus clases de contenedor en su lugar.  
`Arrays.stream()` no brinda la posibilidad de convertir una matriz en una secuencia.
~~~java
int[] arr = {2, 3, 4};
int max = Arrays.stream(arr).max().getAsInt();
~~~
Este metodo abre las oportunidades para usar el poder de los flojos para realizar operaciones como encontrar valores **minimos** y **maximos**, la suma de todos los elementos, filtrar, etc.
---
## Ejercicios
1. Utilizar `Arrays.sort()` para ordenar la matriz dada e imprimirla.
~~~java
public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int[] arr = {scanner.nextInt(), scanner.nextInt(), scanner.nextInt()};
        Arrays.sort(arr);

        System.out.println(Arrays.toString(arr));
    }
}
~~~
2. Usa `Arrays.copyOfRange()` para copiar la matriz dada dentro del rango especificado.
~~~java
public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int[] arr = {scanner.nextInt(), scanner.nextInt(), scanner.nextInt(), scanner.nextInt()};
        int[] newArr = {scanner.nextInt(), scanner.nextInt()};

        System.out.println(Arrays.toString(Arrays.copyOfRange(arr, newArr[0], newArr[1])));
    }
}
~~~
3. Que imprime el codigo.
~~~java
byte[] arr = {5, 2, 4, 1};
byte key = 5;
Arrays.sort(arr);

System.out.println(Arrays.binarySearch(arr, key)); 

// imprime: 3
~~~
4. Cual es el ultimo elemento de `newArr`.
~~~java
byte[] arr = {2, 3, 4};
byte[] newArr = Arrays.copyOf(arr, 2);

/*
Recibe una matriz y crea una copia con el tamaño que se pasa como segundo argumento, en este caso toma los dos primeros valores [2, 3], la respuesta es el numero 3.
*/
~~~
5. Que imprime el siguiente codigo.
~~~java
byte[] arr1 = {1, 2, 3};
byte[] arr2 = {2, 3, 4};

System.out.println(Arrays.compare(arr1, arr2));

/*
Imprime -1 por que los arrays son diferentes.
*/
~~~
6. Convierte un array en una lista e imprime el primer elemento.
~~~java
public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Integer[] arr = {scanner.nextInt(), scanner.nextInt(), scanner.nextInt()};
        // write your code here
        List<Integer> list = Arrays.asList(arr);

        System.out.println(list.get(0));
    }
}
~~~

