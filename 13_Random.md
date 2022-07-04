# Random
Un **numero aleatorio** es un numero que es casi imposibles de predecir, un generador de numerso aleatorios puede proporcionarle dicho numero cuando los necesite.  
Los numeros aleatorios que vamos a generar no son realmente aleatorios porque siempre pueden determinarse por un valor inicial llamado **semilla**.  
Cada vez que obtenemos un nuevo numero aleatorio, en realidad obtenemos el siguiente numero en una secuencia predefinida. Se garantiza que la misma **semilla** produce la misma secuencia si se usa la misma version de tiempo de ejecucion de Java porque el algoritmo es el mismo.  
Java proporciona la clase `Random` para generar valores pseudoaleatorios de diferentes tipos, como `int`, `long`, `double`, e incluso `boolean`. En primer lugar tenemos que importarlo.
~~~java
import java.util.Random;
~~~
Tenemos dos constructores para crear un objeto de esta clase:
- `Random()` crea un nuevo generador y establece la semilla aleatoria.
- `Random(long seed)` crear un nuevo generador con el valor inicial especificado de su estado interno.  
Si no especificamos una semilla, el generador nos dara una nueva secuencia cada vez. Pero si especificamos la semilla, la secuencia se calculara en base a ella.  
Despues de haber creado un generador, podemos invocar uno de  los siguientes metodos:
- `int nextInt()` devuelve un valor pseudoaletorio entero.
- `int nextInt(int n)` devuelve un valor pseudoaleatorio entre `0` y `n` (exclusive).
- `long nextLong()` devuelve un valor pseudoaleatorio tipo `long`.
- `double nextDouble()` devuelve un valor pseudoaletorio entre `0.0` y `1.0`.
- `void nextBytes(byte[] bytes)` genera bytes aleatorios y los coloca en una matriz de bytes proporcionada por el usuario.
~~~java
// este secuencia puede producir diferentes o el mismo resultado
Random random = new Random();
System.out.println(random.nextInt(5)); 

/*
Si necesitamos reproducir la misma secuencia de numeros aleatorios, podemos especificar una semilla para el constructor.
*/
Random random = new Random(100000);
System.out.println(random.nextInt(5)); // print 0, 1, 2, 3, 4
System.out.println(random.nextInt(5)); // print 0, 1, 2, 3, 4
~~~
Supongamos que necesitamos un programa que imprima un numeros especifico de enteros pseudoaleatorios dentro de un rango dado (limites incluidos). Aqui esta el programa completo que imprime 4 enteros pseudoaleatorios dentro de un rango dado:
~~~java
import java.util.*;

public class RandomNumbersDemo {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        int lower = scanner.nextInt();
        int upper = scanner.nextInt();
        Random random = new Random();

        int intervalLength = upper - lower + 1;

        System.out.println(random.nextInt(intervalLength) + lower);
        System.out.println(random.nextInt(intervalLength) + lower);
        System.out.println(random.nextInt(intervalLength) + lower);
        System.out.println(random.nextInt(intervalLength) + lower);
    }
}

/*
Por ejemplo, tenemos que generar numeros exactamente dentro de un rango de 20 a 30 (inclusive):
20 30
Un ejemplo de salida:
25
26
30
20
*/
~~~
---
## Ejercicios
1. Como seria la invocacion del metodo `nextInt()` para generar un numero desde 10 (lower) a 100 (upper) incluyendo los dos bordes.
~~~java
Random random = new Randon();
random.nextInt(upper - lower + 1) + lower;
~~~
2. Ingrese cualquier secuencia de numeros que pueda generar el siguiente codigo.
~~~java
Random generator = new Random();
int a = generator.nextInt(3);
int b = generator.nextInt(2) + 1;
int c = generator.nextInt(4);
System.out.println(a + " " + b + " " + c);

// secuencia generada: 0 1 3 y 1 2 1
~~~
3. Que numeros puede generar con el siguiente codigo.
~~~java
int n = random.nextInt(3);

// 0, 1, 2
~~~
4. Para los numeros dados K, N y M encuentre la primera semilla que sea mayor o igual que K donde cada uno de N numeros gaussianos sea menor o igual que M.
~~~java
public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        double k = scanner.nextDouble();
        double n = scanner.nextDouble();
        double m = scanner.nextDouble();
        long seed = 0;
        while (true) {
            Random random = new Random(seed);
            boolean allLessThanM = true;
            for (int i = 0; i < n; i++) {
                double gaussianValue = random.nextGaussian();
                allLessThanM &= gaussianValue <= m;
            }
            if (allLessThanM && seed >= k) {
                break;
            }
            seed++;
        }
        System.out.println(seed);
    }
}
~~~
5. Su trabajo es encontrar la semilla entre A y B (ambos inclusive) que produce N numeros psudoaleatorios desde 0 (inclusive) hasta K (exclusivo). Tambien debe tener el maximo de estos N para que sea el minimo entre todos los maximos de otras semilas en este rango.
~~~java
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int a = sc.nextInt();
        int b = sc.nextInt();
        int n = sc.nextInt();
        int k = sc.nextInt();

        int seed = a;
        int maximum = k;
        for (int i = a; i <= b; i++) {
            Random rd = new Random(i);
            int nextMaximum = 0;
            for (int j = 0; j < n; j++) {
                int nextInt = rd.nextInt(k);
                if (nextInt > nextMaximum) {
                    nextMaximum = nextInt;
                }
            }
            if (nextMaximum < maximum) {
                seed = i;
                maximum = nextMaximum;
            }
        }

        System.out.printf("%d%n%d%n", seed, maximum);
    }
}
~~~
