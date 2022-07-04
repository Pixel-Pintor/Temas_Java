# Array
Cuando necesite procesar varios objetos del mismo tipo, puede guardarlos en una **matriz** y luego procesarlos juntos como una sola unidad. Puede considerar una matriz como una coleccion de elementos del mismo tipo. Todos los elementos se almacenan en la memoria secuencialmente.  
En Java, una matriz tiene las siguientes caracteristicas importantes:  
- Una matriz es un tipo de referencia
- Todos los elementos de la matriz se almacenan en la memoria secuencialmente
- A cada elemento del arreglo se accede por su indice numerico, el primer elemento tiene el indice 0
- Se accede al ultimo elemento mediante el indice igual al tamaño de la matriz -1
- Es posible crear una matriz para almacenar elementos de cualquier tipo.  
Para declarar una matriz, debemos usar **[]** despues del tipo de elementos de la matriz:
~~~java
int[] array; // forma 1
int array[]; // forma 2
~~~
La primera forma es la que se usa con mas frecuencia.
---
## Crear una matriz con elementos especificos
La forma mas sencilla de instanciar e inicializar una matriz es enumerar todos sus elementos.
~~~java
int[] numbers = { 1, 2, 3, 4 };
~~~
Otra forma es inicializar una matriz usando variables:
~~~java
int a = 1, b = 2, c = 3, d = 4;
int[] numbers = { a, b, c, d }; // instanciacion e inicializacion
~~~
La forma mas general de crear una matriz es usar la palabra `new` y especificando el numero necesario de elementos:
~~~java
int n = ...; // n es la longitud del array
int[] numbers = new int[n];
~~~
Ahora, la matriz tiene `n` elementos. Cada elementos es igual a cero (el valor por defecto del tipo `int`). Es posibles separar la declaracion y la instanciacion en dos lineas:
~~~java
int[] numbers; // declaracion
numbers = new int[n]; // instanciacion e inicializacion
~~~
Ademas, podemos escribir la palabra clave `new` y enumerar todos los elementos de una matriz:
~~~java
float[] floatNumbers; // declaracion
floatNumbers = new float[] { 1.02f, 0.03f, 4f }; // instanciacion e inicializacion
~~~
Puede llenar una matriz con el metodo `fill()`.
~~~java
int size = 10;
char[] characters = new char[size];

// toma una array, indice inicial, indice final (exclusivo)
Arrays.fill(characters, 0, size / 2, 'A');
Arrays.fill(characters, size / 2, size, 'B');

// System.out.println(Arrays.toString(characters)); // imprime [A, A, A, A, A, B, B, B, B, B]
~~~
Para obtener la longitud de una matriz existente, accede a la propiedad especial `arrayName.length`.
~~~java
int[] array = { 1, 2, 3, 4 }; // array de numeros
int length = array.length; // numero de elemtnos del array
System.out.println(length); // 4
~~~
Puede usar el indice para establecer un valor de la matriz o para obtener un valor de ella.
~~~java
int[] numbers = new int[3]; // numbers: [0, 0, 0]
numbers[0] = 1; // numbers: [1, 0, 0]
numbers[1] = 2; // numbers: [1, 2, 0]
numbers[2] = numbers[0] + numbers[1]; // numbers: [1, 2, 3]
~~~
Si intentamos obtener el cuarto elemento (con indice 3) de la matriz `numbers`.
~~~java
int elem = numbers[3];

// lanza un ArrayIndexOutOfBoundsException
~~~
---
## Ejercicios
1. Cual es el resultado del codigo
~~~java
int[] numbers = { 1, 2, 3, 4, 5 };
Arrays.fill(numbers, 1, 5, 10);
System.out.println(Arrays.toString(numbers));

// salida: [1, 10, 10, 10, 10]
~~~
2. Inicializa un array de enteros
~~~java
public class Main {
    public static void main(String[] args) {
        int[] numbers = { 12, 17, 8, 101. 33 };
        System.out.println(Arrays.toString(numbers));
    }
}
~~~
3. Dado el codigo determinar el valor de `n`
~~~java
int arr[] = new int[] { 1, 2, 3, 4, 5, 6, 7, 8, 9 };
int n = 6;
n = arr[arr[n]];

// valor de n: 8
~~~
4. Inicializa un array de tipo `long`
~~~java
public class Main {
    public static void main(String[] args) {
        long[] longNumbers = {100000000001L, 100000000002L, 100000000003L};
        System.out.println(Arrays.toString(longNumbers));
    }
}
~~~
5. Igualdad de Arrays
~~~java
int[] numbers1 = { 1, 2, 3, 4 };
int[] numbers2 = { 1, 2, 3, 4 };
int[] numbers3 = { 4, 3, 2, 1 };
int[] numbers4 = { 1, 2, 3 };

Arrays.equals(numbers1, numbers1); // true
Arrays.equals(numbers1, numbers2); // true
Arrays.equals(numbers2, numbers3); // false
~~~
6. Determina el valor de salida y de `n`
~~~java
int arr[] = new int[] { 0, 1, 1, 2, 3, 5, 8, 13, 21 };
int n = 6;
n = arr[arr[n] / 2];

// valor de n: 3
~~~
