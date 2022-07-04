# Sobrecarga
La sobrecarga le permite cambiar la firma del metodo: el numeros de parametros, su tipo o ambos. Si los metodos tienen el mismo nombre, pero un numero o tipo diferente de parametros, estan **sobrecargados**. Significa que puede invocar diferentes metodos con el mismo nombre pasando diferentes argumentos.
---
## Como sobrecargar metodos
Como ejemplo, consideremos alguno metodo sobrecargado de la clase estandar `Math`:
~~~java
public static int abs(int a) { return (a < 0) ? -a : 1; }
public static float abs(float a) { return (a <= 0.0F) ? 0.0F - a : a; }
~~~
Estos metodos tienen el mismo nombre pero diferente tipo de argumentos Estan **sobrecargados**. El tipo de valor de retorno de un metodo no se considera para la sobrecarga porque no forma parte de la firma. Aqui hay cuatro metodos `print` para imprimir diferentes valores.
~~~java
public static void print(String stringToPrint) {
    System.out.println(stringToPrint);
}

public static void print(String stringToPrint, int times) {
    for (int i = 0; i < times; i++) {
        System.out.println(stringToPrint);
    }
}

public static void print(int times, String stringToPrint) {
    for (int i = 0; i < times; i++) {
        System.out.println(stringToPrint);
    }
}

public static void print(int val) {
    System.out.println(val);
}

// invocamos los metodos
print("some string")
print("another string", 2);
print(2, "another string again");
print(5);

// salida
// some string
// another string
// another string
// another string again
// another string again
// 5
~~~
Tenga en cuenta que en el caso de que los parametros tengan diferentes tipos, cambiar el orden de estos parametros es un caos valido de sobrecarga, como en el segundo y tercer metodo del ejemplo anterior. Supondremos que la sobrecarga es una forma de polimorfismo estatico (tiempo de compilacion).
---
## Sobrecarga y lanzamiento
En el caso de que el tipo de un parametro de metodo no sea exactamente el mismo que el tipo del argumento pasado, el compilador elige el metodo que tiene el tipo de argumento mas cercano en el orden de la conversion implicita.
~~~java
public class OverloadingExample {

    public static void print(short a) {
        System.out.println("short arg: " + a);
    }

    public static void print(int a) {
        System.out.println("int arg: " + a);
    }

    public static void print(long a) {
        System.out.println("long arg: " + a);
    }

    public static void print(double a) {
        System.out.println("double arg: " + a);
    }

    public static void main(String[] args) {
        print(100);
    }
}

// llamamos al metodo y el compilador elige
print(100); // int arg: 100
~~~
Si comentamos el metodo tipo `int`, el compilador selecciona el metodo de tipo `long`, `long arg: 100`.  
Si comentador el metodo tipo `long`, como no tenemos uno con tipo `float` el compilador elige el de tipo `double`, `double arg: 100.0`.  
Si solo nos queda el metodo de tipo `short` se genera error por que la unica forma de convertir el 100 y pasarlo al metodo seria convertirlo explicitamente:
~~~java
public class OverloadingExample {

    public static void print(short a) {
        System.out.println("short arg: " + a);
    }

    public static void main(String[] args) {
        print((short) 100); // casting explicito
    }
}
~~~
---
## Ejercicios
1. Que imprimira el codigo.
~~~java
public class Main {

    public static void method(int i) {
        System.out.println("int");
    }

    public static void method(long l) {
        System.out.println("long");
    }

    public static void method(char c) {
        System.out.println("char");
    }

    public static void main(String[] args) {
        method('a');
    }
}

// char
~~~
2. Que imprimira el codigo.
~~~java
public class Main {

    public static void method(float f) {  
        System.out.println("float");
    }
    
    public static void method(int i) {  
        System.out.println("int");
    }
    
    public static void method(double d) {  
        System.out.println("double");
    }

    public static void main(String[] args) {
        method(10.2f);
    }
}

// float
~~~
3. Que imprimira el codigo.
~~~java
public class OverloadingDemo {

    public static void method(int i, float f) {
        System.out.println("int float method");
    }

    public static void method(float f, int i) {
        System.out.println("float int method");
    }

    public static void method(int i1, int i2) {
        System.out.println("int int method");
    }

    public static void method(double d1, double d2) {
        System.out.println("double double method");
    }

    public static void main(String[] args) {
        method(1f, 1f);
    }
}

// double double method
~~~
