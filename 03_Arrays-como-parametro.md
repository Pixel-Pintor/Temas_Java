## Arrays como parametros
Un metodo puede tener parametros de cualquier tipo, incluidos arreglos, cadenas, tipos primitivos, etc. En el siguiente ejemplo, el metodo `processArray` tiene un unico parametro del tipo `int[]`:
~~~java
public static void processArray(int[] array) { ... }
~~~
El siguiente metodo intercambiar el primero y el ultimo elemento de su parametro (matriz).
~~~java
public static void swapFirstAndLastElements(int[] nums) {
    if (nums.length < 1) {
        return;
    }

    int temp = nums[nums.length - 1];
    nums[nums.length - 1] = nums[0]; 
    nums[0] = temp;                  
}

// llama el metodo desde el metodo principal
public static void main(String[] args) {
    int[] numbers = { 1, 2, 3, 4, 5 };
    System.out.println(Arrays.toString(numbers));
    swapFirstAndLastElements(numbers);
    System.out.println(Arrays.toString(numbers));
}

// la salida es:
// [1, 2, 3, 4, 5]
// [5, 2, 3, 4, 1]
~~~
---
## Varargs
El posible pasar un numero arbitrario de argumentos del mismo tipo a un metodo utilizando la sintaxis especial llamada **varargs** (argumentos de longitud variable). Estos argumentos se especifican mediante tres puntos despues del tipo. En el cuerpo del metodo, puede procesar este parametro como una matriz regular del tipo especificafo.  
El siguiente metodo toma un **vararg** y genera el numero de argumentos en la salida estandar utlizando la **longitud** de las matrices.
~~~java
public static void printNumberOfArguments(int... numbers) {
    System.out.println(numbers.length);
}
~~~
Ahora puede invocar el metodo pasando varios numeros enteros o una matriz de enteros.
~~~java
printNumberOfArguments(1);
printNumberOfArguments(1, 2);
printNumberOfArguments(1, 2, 3);
printNumberOfArguments(new int[] { }); // sin argumentos
printNumberOfArguments(new int[] { 1, 2 });

// salida
1
2
3
0
2
~~~
Este ejemplo tambien demuestra la diferencia entre los argumentos y los parametros de un metodo. El metodo tiene un solo parametro, pero se puede llamar con varios argumentos.  
Si un metodo tiene mas de un parametro, el parametro `vararg` debe ser el ultimo en la declaracion del metodo.  
~~~java
// incorrecto
public static void method(double... vararg, int a) { ... }
// correcto
public static void method(int a, double... vararg) { ... }
~~~
---
## Ejercicios
1. Escribe los metodos que utilicen de manera correcta el parametro **vararg**.
~~~java
public static void method(int[] a, long... vararg) { ... }
public static void method(long... vararg) { ... }
~~~
2. Escribe el metodo que debe tomar una matriz de valores booleanos y negar cada uno de ellos.
~~~java
public class Main {
    
    public static void inverseFlags(boolean.. flags) {
        for (int i = 0; i < flags.length; i++) {
            flags[i] = !flags[i];
        }
    }

    public static void main(String[] args) {
        final Scanner scanner = new Scanner(System.in);
        final List<Boolean> booleans = Arrays
            .stream(scanner.nextLine().split("\\s+"))
            .map(Boolean::parseBoolean)
            .collect(Collectors.toList());
        final boolean[] flags = new boolean[booleans.size()];
        for (int i = 0; i < flags.length; i++) {
            flags[i] = booleans.get(i);
        }    
        inverseFlags(flags);
        final String representation = Arrays.toString(flags)
            .replace("[", "")
            .replace("]", "")
            .replace(",", "")
        System.out.println(representation);    
    }
}
~~~
2. Llamando **vararg**
~~~java
public static void method(int... vararg)

// llamados diferentes
method(0); // 1
method(1, 2, 3); // 3
method(new int[] {1, 2}); // 2
method();
method(null); // arroja una excepcion
~~~
3. El metodo toma una array de enteros y retorn una array de enteros, el array retornado debe contener el primer y el ultimo elemento del array introducido.
~~~java
public class Main {

    public static int[] getFirstAndLast(int... input) {
        return new int[] {
            input[0],
            input[input.length - 1],
        }
    }

    public static void main(String[] args) {
        Scanner scanenr = new Scanner(System.in);
        int[] array = Arrays.stream(scanner.nextLine.split(" "))
            .mapToInt(Integer::parseInt)
            .toArray();
        int[] result = getFirstAndLast(array);
        Arrays.stream(result).forEach(e -> System.out.print(e + " "));    
    }
}
~~~
4. Escriba un metodo que ordene los numeros de un array en orden ascendente.
~~~java
public class Main {

    // solucion 1
    public static void sort(int[] numbers) {
        Arrays.sort(numbers);
    }

    // solucion 2
    public static void sort(int[] numbers) {
        boolean bol = true;
        while (bol) {
            bol = false;
            for (int j = 0; j < numbers.length - 1; j++) {
                if (numbers[j] > numbers[j + 1]) {
                    int temp = numbers[j];
                    numbers[j] = numbers[j + 1];
                    numbers[j + 1] = temp;
                    bol = true;
                }
            }

        }
    }

    public static void main(String[] args) {
        final Scanner scanner = new Scanner(System.in);
        String[] values = scanner.nextLine().split("\\s+");
        int[] numbers = Arrays.stream(values)
                .mapToInt(Integer::parseInt)
                .toArray();
        sort(numbers);
        Arrays.stream(numbers).forEach(e -> System.out.print(e + " "));
    }
}
~~~
5. El metodo deberia tomar una array y agregar un valor al elemento especificado por su indice.
~~~java
public class Main {

    public static void addValueByIndex(long[] array, int index, long value) {
        array[index] += value;
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        long[] array = Arrays.stream(scanner.nextLine().split(" "))
                .mapToLong(Long::parseLong)
                .toArray();
        int index = scanner.nextInt();
        long value = scanner.nextLong();
        addValueByIndex(array, index, value);
        Arrays.stream(array).forEach(e -> System.out.print(e + " "));
    }
}
~~~
6. El metodo toma un array de `String` y debe imprimir cada linea del array.
~~~java
class Application {
    
    void run(String[] args) {
        for (String string : args) {
            System.out.println(string);
        }
    }
}
~~~
