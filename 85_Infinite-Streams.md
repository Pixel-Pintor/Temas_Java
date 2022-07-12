# Infinite Streams

Las colecciones en Java siempre tienen un número finito de elementos porque la memoria de una computadora es limitada. Sin embargo, Stream API nos permite operar en flujos infinitos de elementos. Es posible debido a la naturaleza perezosa de los flujos: crear un flujo infinito es una operación intermedia, por lo que en realidad no crea elementos hasta que se ejecuta una operación de terminal. Se supone que un stream ya se ha vuelto finita en este momento.

## Generando un stream de objetos

El primero metodo que consideramos es **generate**. Se necessita una *funcion de proveedor* que produzca un objeto que formara parte del stream.

~~~java
Stream<T> generate(Supplier<T> supplier)
~~~

En el siguiente ejemplo, creamos un stream infinito de numeros aleatorios usando la referencia al metodo **random** de la clase **Math**.

~~~java
Stream<Double> randomNumbers = Stream.generate(Math::random);
~~~

La funcion proveedor tambien puede ser un constructor de una clase.

~~~java
Stream<User> userStream = Stream.generate(User::new);
~~~

Genera un stream de objetos **User** que emplean su constructor sin argumentos.  
Dependiendo de como funcione la *funcion de proveedor*, todos los elementos de dicho stream pueden ser diferentes o iguales.  
Pero, ¿por que no tratamos de imprimir los elementos de un stream infinito usando **forEach**?. Bueno, no es una buena idea. Su computadora no tiene suficiente memoria para hacer eso.  
No importa cómo haya creado un stream infinito, debe hacerlo finito usando **limit** antes de invocar una operación de terminal o para invocar solo un tipo especial de operaciones de terminal como **findFirst**/**findAny** que no trata de generar todos los elementos en el stream infinito.  
En el siguiente ejemplo, creamos un stream infinito y tomamos solo los primeros cinco elementos de el:

~~~java
Stream.generate(Math::random)
        .limit(5)
        .forEach(System.out::println);
~~~

Genera e imprime cinco numeros aleatorios. Su salida puede parecerse a los siguiente.

~~~java
0.9700910364529439
0.04347176223398197
0.2887419669771004
0.32376617731162183
0.22532773260604255
~~~

Por supuesto, tambien podemos usar una variable para producir un numero arbitrario de elementos, en lugar de una constante especifica.  
Veamos otro ejemplos que aplica el metodo **findAny**. Este metodo solo retorna un solo elemento de una secuencia.

~~~java
double randomNumber = Stream.generate(Math::random).findAny().get();
~~~

Este codigo funciona perfectamente porque el metodo **findAny** hace que solo se genere un elemento. No trata de crear todo el stream infinito.

## Iteraciones

El segundo metodo que consideramos es **iterate**. Es una alternativa completa a los bucles estandar.  
Se necesita un valor inicial (una *semilla*) y una *funcion de operador* para generar el siguiente valor basado en el anterior.

~~~java
Stream<T> iterate(T seed, UnaryOperator<T> next)
~~~

Como ejemplo, aqui hay una secuencia infinita de numeros impares.

~~~java
Stream<Integer> oddNumbersStream = Stream.iterate(1, x -> x + 2); // 1, 3, 5, ...
~~~

Aqui el valor inicial es **1**, y la funcion para producir los siguientes valores es **x -> x + 2**.  
De manera similar al metodo **generate**, este stream es infinita. Si queremos obtener los valores, debemos hacer que esta secuencia sea finita.  
En el siguiente ejemplo, imprimimos los primeros cinco numeros impares usando el metodo **iterate**.

~~~java
Stream.iterate(1, x -> x + 2)
        .limit(5)
        .forEach(System.out::println); // 1 3 5 7 9
~~~

Si usamos el metodo **findFirst** como la operacion terminal en lugar de **forEach**, podemos saltarnos la operacion **limit** porque es obvio que el primer elemento es 1 (la semilla).  
Curiosamente, el siguiente codigo conducira a un error.

~~~java
int min = Stream.iterate(1, x -> x + 1).min(Comparator.naturalOrder()).get();
~~~

Para nosotros, es obvio que el elemento más pequeño aquí es 1. Pero la computadora no conoce las propiedades de la función que se usa para producir los siguientes valores y no puede comprender que la función no creará otro valor mínimo. El mismo error ocurrirá si tratamos de realizar cualquier operación que requiera formalmente procesar todos los elementos del flujo infinito.  
Ademas, el metodo **iterate** tiene una version sobrecargada que genera un stream infinito. Se necesita un **predicado** para determinar si la secuencia tiene el siguiente elemento.

~~~java
Stream<T> iterate(T seed, Predicate<T> hasNext, UnaryOperation<T> next)
~~~

En el siguiente ejemplo, usamos el metodo para imprimir los numeros impares que son menores que 10.

~~~java
Stream.iterate(1, x -> x < 10, x -> x + 2)
        .forEach(System.out::println); // 1 3 5 7 9
~~~

Esta es una alternativa completa al clasifco ciclo **for**.

~~~java
for (int i = 1; i < 10; i += 2) {
    System.out.println(i);
}
~~~

Con esto, estamos completando nuestra consideracion de streams infinitos y metodos para producirlos.

---

## Ejemplos

1. Que secuencia de numeros imprimira el codigo.

	~~~java
	Stream.iterate(1, i -> i + 2)
                .skip(5)
                .limit(5)
                .forEach(n -> System.out.print(n + " "));
	// 11 13 15 17 19
	~~~

2. Que secuencia imprimira el codigo.

	~~~java
	IntStream.iterate(10, (n) -> n + 1).limit(3).forEach(System.out::println);
	// 10
	// 11
	// 12
	~~~

3. Que imprimira el codigo.

	~~~java
	boolean result = IntStream
			.iterate(0, n -> n + 1)
			.limit(100)
			.filter(n -> n % 2 != 0)
			.noneMatch(n -> n % 2 == 0);

	System.out.println(result); // true
	~~~

4. Cuales metodos generan streams infinitos de las colecciones?

	- **iterate**
	- **generate**

5. Necesita implementar el metodo **generateNCats** para generar **n** gatos identicos.

	~~~java
	class GenerateCats {

    	public static List<Cat> generateNCats(int n) {
        	return Stream.generate(Cat::new)
            	    .limit(n)
                	.collect(Collectors.toList());
    	}

    	public static void main(String[] args) {
        	Scanner scanner = new Scanner(System.in);

        	List<Cat> cats = generateNCats(scanner.nextInt());

        	System.out.println(cats.size());
    	}
	}

	class Cat {
    	String name;
    	int age;
	}
	~~~

6. Implemente un metodo que retorne un stream de las primeras **n** potencias de dos a partir de la potencia 0, su salida debe comenzar con 1.

	~~~java
	class StreamUtils {
    	public static Stream<Integer> generateStreamWithPowersOfTwo(int n) {
        	return Stream.iterate(1, x -> x * 2).limit(n);
    	}
	}
	~~~
