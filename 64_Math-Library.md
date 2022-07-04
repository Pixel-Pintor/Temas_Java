# Math Library
Existen algunos metodos para redondear numeros o para otras operaciones. Miremos algunos de ellos.
- **Math.min(..., ...)** retorna el valor mas pequeño de dos argumento.
- **Math.max(..., ...)** retorna el valor mayor de dos argumentos.
~~~java
int min = Math.min(11, 81); // min is 11
int max = Math.max(20, 30); // mas is 30
~~~
- **Math.abs(...)** retorna el valor absoluto de su argumento.
~~~java
int abs = Math.abs(-10); // abs is 10
double dabs = Math.abs(-10.33); // dabs is 10.33
~~~
- **Math.floor(...)** retorna el numero redondeado por lo bajo.
- **Math.ceil(...)** retorna el numero redondeado por lo alto.
~~~java
double floor = Math.floor(3.78); // floor is 3.0
double ceil = Math.ceil(4.15); // ceil is 5.0
~~~

## Funciones exponenciales
Cuando necesitamos calcular un cuadrado o una raiz cuadrada de un numero dado, podemos aplicar los siguientes metodos.
- **Math.sqrt(...)** retorna la raiz cuadrada de su argumento.
- **Math.cbrt(...)** retorn la raiz cubica de su argumento.
~~~java
double sqrt = Math.sqrt(2); // sqrt is 1.4142...
double cbrt = Math.cbrt(27.0); // cbrt is 3.0
~~~
- **Math.pow(..., ...)** retorna el primero valor a la potencia del segundo valor.
~~~java
double square = Math.pow(5, 2); // the square of 5 is 25
double cube = Math.por(2, 3); // the cube of 2 is 8.0
~~~

## Funciones trigonometricas
Y aqui estan algunas de las funciones trigonometricas proporcionadas por `Math`.
- **Math.sin(...)** devuelve el seno trigonometrico del angulo dado en radianes.
- **Math.cos(...)** devuelve el coseno trigonometrico del angulo dado en radianes.
~~~java
double cos = Math.sin(pi / 2); // sin90° is 1.0
double cos = Math.cos(pi); // cos180° is -1.0
~~~
- **Math.toRadians(...)** convierte un angulo medido en grados a un anglo medido en radianes (aproximadamente).
~~~java
double grad = Math.toRadians(30); // grad is 0.5235...
~~~

## Y hay mas
Tambien hay metodos para funciones hiperbolicas, logaritmicas, angulares y otras.
- **Math.random()** devuelve un valor doble con signo positivo, mayor o igual a 0,0 y menor a 1,0.
~~~java
double random = Math.random(); // a random value >= 0.0 and < 1.0
~~~
Aparte de las funciones, Java `Math` contiene dos **constantes  comunes**:
- **Math.PI** es la relaciones entre la circumferencia de un circulo y su diametro.
- **Math.E** es la base del logaritmo natural.  

## La longitud de la hipotenusa
Ahora echemos un vistazo a un ejemplo. Supongamos que tenemos un triangulo rectangulo (un angulo mide 90 grados). Conocemos las longitudes de ambos lados: a = 3 y b = 4. Nuestra tarea es calcular la longitud de la hipotenusa. Ahora es el momento de repasar la lista de las funciones de la clase `Math`. Despues de encontrar el que necesitamos, lo unico que queda es escribir el siguiente codigo.
~~~java
double a = 3, b = 4;
double c = Math.hypot(a, b); // c is 5.0
~~~
---

## Ejercicios
1. Implementa la formula de Hero.
~~~java
class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        double a = scanner.nextDouble();
        double b = scanner.nextDouble();
        double c = scanner.nextDouble();
        double p = (a + b + c) / 2.0;
        double valS = Math.sqrt(p * (p - a) * (p - b) * (p - c));
        System.out.println(valS);
    }
}
~~~
2. Conecta los metodos con sus resultados.
~~~java
Math.ceil(3.04); // 4.0
Math.ceil(4.76); // 5.0
Math.floor(3.08); // 3.0
Math.floor(2.83); // 2.0
~~~
3. Selecciona las invocaciones que retornen 10 o 10.0.
~~~java
Math.round(9.7);
Math.ceil(9.7);
~~~
