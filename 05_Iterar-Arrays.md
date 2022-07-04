# Iterar sobre Arrays
Una forma conveniente de procesar una matriz es iterar sobre ella mediante un bucle. El `length` propiedade de una matriz puede ayudarnos a evitar una `ArrayIndexOutOfBoundsException`.  
Puede llenar una matriz con los cuadrado de los indices de sus elementos.
~~~java
int n = 10;
int[] squares = new int[n];

System.out.println(Arrays.toString(squares)); // [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]

// iterar sobre el array
for (int i = 0; i < squares.length; i++) {
    squares[i] = i * i; // establece el valor del elemento
}

System.out.println(Arrays.toString(squares));
~~~
El siguiente programa verifica si la matriz dada esta ordenada en orden ascendente e imprime "OK" si la respuesta es positiva; de lo contrario, imprime "BROKEN".
~~~java
int[] numbers = { 1, 2, 3, 4, 5, 10, 6 };
boolean broke = false;

for (int i = 1; i < numbers.length; i++) {
    if (numbers[i] < numbers[i - 1]) {
        broken = true;
        break;
    }
}

if (broken) {
    System.out.println("BROKEN")
} else {
    System.out.println("OK");
}


// BROKEN
~~~
Tambien podemos usar un bucler para leer todos los elementos de una matriz desde la entrada estandar. Por ejemplo, la siguiente entrada consta de dos lineas. La primera linea contiene la longitud de la matriz y la segunda linea contiene todos sus elementos.
~~~java
5
101 102 504 302 881
~~~
Puede leer los numeros usando `Scanner` y luego generar todos los numeros que leyo.
~~~java
Scanner scanner = new Scanner(System.in);

int len = scanner.nextInt();
int[] array = new int[len];

for (int i = 0; i < len; i++) {
    array[i] = scanner.nextInt();
}

System.out.println(Arrays.toString(array));

// [101, 102, 504, 302, 881]
~~~
El ciclo `for-each` se usa para iterar a traves de cada elemento de una matriz, una cadena o una coleccion sin indices.
~~~java
char[] characters = { 'a', 'b', 'c', 'a', 'b', 'c', 'a' };

int counter = 0;
for (char ch : characters) {
    if (ch == 'a') {
        counter++;
    }
}

System.out.println(counter); // imprimr "3"
~~~
El bucle **for-each** tiene algunas limitaciones. En primer lugar, no puede usarlo si desea modificar una matriz, porque la variable que usamos para las iteraciones no almacena el elemento de la matriz en si, solo una copia. Tambien es imposible obtener un elemento por su indice ya que no tenemos pista de indice. Finalmente, como se desprende del nombre, no podemos movernos a traves de una matriz con mas de un paso por iteracion; iteramos sobre todos y cada uno de los elementos, por lo que trabajamos con ellos uno por uno. El ciclo **for-each** tambien le permite evitar el `ArrayIndexOutOfBoundsException`.
---
## Ejercicios
1. Comparar los tamaños de dos cajas.
~~~java
public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        final int sides = 3;
        int[] box1 = new int[sides];
        int[] box2 = new int[sides];

        for (int i = 0; i < sides; i++) {
            box1[i] = scanner.nextInt();
        }
        for (int i = 0; i < sides; i++) {
            box2[i] = scanner.nextInt();
        }
        Arrays.sort(box1);
        Arrays.sort(box2);
        String result;
        if (box1[0] < box2[0] && box1[1] < box2[1] && box1[2] < box2[2]) {
            result = "Box 1 < Box 2";
        } else if (box1[0] > box2[0] && box1[1] > box2[1] && box1[2] > box2[2]) {
            result = "Box 1 > Box 2";
        } else {
            result = "Incompatible";
        }
        System.out.println(result);
    }
}
~~~
2. Rotar el array por un numero dado.
~~~java
class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        var array = scanner.nextLine().split(" ");
        var rightRotationCount = scanner.nextInt();
        
        Integer[] ints = new Integer[array.length];
        for (int i = 0; i < array.length; i++) {
            ints[i] = Integer.parseInt(array[i]);
        }

        Collections.rotate(Arrays.asList(ints), rightRotationCount);
        
        for (int num : ints) {
            System.out.print(num + " ");
        }        
    }
}
~~~
3. Escribe un programa que lea un array en minuscula y verifica si el array esta en orden alfabetico.
~~~java
class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        boolean result = true;
        String[] strings = scanner.nextLine().split(" ");
        for (int i = 0; i < strings.length - 1; i++) {
            if (strings[i].compareTo(strings[i + 1]) > 0) {
                result = false;
                break;
            }
        }
        System.out.println(result);
    }
}
~~~
4. Escribe un programa que lea un array de enteros e imprima la longitud de la secuencia mas larga en orden estrictamente ascendente.
~~~java
class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int sizeArray = scanner.nextInt();
        int[] array = new int[sizeArray];
        int count = 1;
        List<Integer> counts = new ArrayList<>();
        for (int i = 0; i < sizeArray; i++) {
            array[i] = scanner.nextInt();
        }

        for (int i = 1; i < array.length; i++) {
            if (array[i - 1] <= array[i]) {
                count++;
            } else if (array[i - 1] > array[i]) {
                counts.add(count);
                count = 1;
            }
            counts.add(count);
        }

        int longestSequence = counts.stream()
                .max(Integer::compare)
                .orElse(1);
        System.out.println(longestSequence);
    }
}
~~~
5. Cuenta cuantas veces un nombre esta en un array.
~~~java
class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        int[] array = new int[n];
        for (int i = 0; i < n; i++) {
            array[i] = scanner.nextInt();
        }
        int num = scanner.nextInt();
        int count = 0;
        for (int i = 0; i < n; i++) {
            if (array[i] == num) {
                count++;
            }
        }
        System.out.println(count);
    }
}
~~~
6. Escribe un programa que lea un array de enteros y un numero, el programa debe verificar si el array contiene el numero.
~~~java
class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int size = scanner.nextInt();
        int[] array = new int[size];
        for (int i = 0; i < size; i++) {
            array[i] = scanner.nextInt();
        }
        int value = scanner.nextInt();
        boolean contains = false;
        for (int i = 0; i < size; i++) {
            if (array[i] == value) {
                contains = true;
            }
        }
        System.out.println(contains);
    }
}
~~~
7. Escribe un programa que lea un array de enteros y encuentre the valor minimo del array.
~~~java
class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        int[] array = new int[n];
        for (int i = 0; i < n; i++) {
            array[i] = scanner.nextInt();
        }
        int min = array[0];
        for (int i = 1; i < n; i++) {
            if (array[i] < min) {
                min = array[i];
            }
        }
        System.out.println(min);
    }
}
~~~
8. Escriba un programa que calcula la suma de los elementos de un array de enteros.
~~~java
class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        int[] array = new int[n];
        for (int i = 0; i < n; i++) {
            array[i] = scanner.nextInt();
        }
        int sum = 0;
        for (int i = 0; i < n; i++) {
            sum += array[i];
        }
        System.out.println(sum);
    }
}
~~~
9. Escriba un programa que lea un array de enteros e imprima el producto maximo de dos elementos adjacentes.
~~~java
class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        int[] array = new int[n];
        for (int i = 0; i < n; i++) {
            array[i] = scanner.nextInt();
        }
        int max = 0;
        for (int i = 0; i < n - 1; i++) {
            int product = array[i] * array[i + 1];
            if(product > max) {
                max = product;
            }
        }
        System.out.println(max);
    }
}
~~~
10. Escriba un programa que lea un array desorganizado de enteros y dos numeros, debe mostrar si los numeros estan seguidos en el array en cualquier orden.
~~~java
class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int arraySize = scanner.nextInt();
        int[] array = new int[arraySize];

        for (int i = 0; i < array.length; i++) {
            array[i] = scanner.nextInt();
        }
        int numOne = scanner.nextInt();
        int numTwo = scanner.nextInt();

        System.out.println(checkNumPosition(array, numOne, numTwo));
    }

    public static boolean checkNumPosition(int[] array, int numOne, int numTwo) {
        boolean numsNextToEachOther = false;
        for (int i = 0; i < array.length - 1; i++) {
            if ((array[i] == numOne && array[i + 1] == numTwo)
                    || (array[i] == numTwo && array[i + 1] == numOne)) {
                numsNextToEachOther = true;
                break;
            }
        }
        return numsNextToEachOther;
    }
}
~~~
