# Operador Ternario  
El operador ternario es un operador que evalua una condicion y elige uno de dos casos para ejecutar. Se puede considerar como una forma de declaracion `if-else`.
~~~java
int a = ...;
int b = ...;
int max = ...;

if (a > b) {
    max = a;
} else {
    max = b;
}

// El operador ternario equivalentes es el siguiente
int max = a > b ? a : b;
~~~
Consideremos otro ejemplo que imprime si un numero es par o impar.
~~~java
int num = ...; // Inicializa con un valor
SOUT(num % 2 == 0 ? "Even" : "Odd");
~~~
Imagina que necesitas comparar 2 numeros enteros e imprimr `equal` en caso que sean iguales, `more` si el primero tiene un valor mayor que el segundo y `less` de lo contrario, la tarea se puede resolver usando una combinacion de 2 operadores.
~~~java
int a = ...;
int b = ...;
String result = a == b ? "equal" : a > b ? "more" : "less";
~~~

---
## Ejercicios
1. Escribe formas de escribir una expresion para encontrar el maximo de dos numeros con el operador ternario.
~~~java
long max = first < second ? second : first;
long max = first > second ? first : second;
~~~
2. Convierte la expresion en un operador ternario
~~~java
// Expresion
if (a + b == c) {
    d = c;
} else {
    d = 10;
}

// Ternario
int d = a + b != c ? 10 : c;
~~~
3. Cual de las siguientes declaraciones retorna 10?
~~~java
x <= 10 ? x : y;
x < y ? x : y;
~~~

