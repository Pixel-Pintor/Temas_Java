# Interfaces Funcionales

En este tema, aprenderá un nuevo concepto llamado **interfaces funcionales**. Este es un conocimiento vital porque este tipo de interfaces permite a los desarrolladores usar la programación funcional sobre los conceptos de OOP y, en cierto sentido, conectan ambos mundos. Son las interfaces funcionales las que hacen posible el uso de expresiones lambda, referencias a métodos y otras cosas funcionales. Suponemos que ya está familiarizado con las interfaces, las expresiones lambda y las clases anónimas. Ahora vamos a mostrarte cómo están todos conectados entre sí.

## Funciones e Interfaces

Si una interfaz contiene solo un **unico metodo abstracto** (dicha interfaz a veces se denomina **tipo SAM**), se puede considerar como un funcion. Ademas de las formas estandar de implementar interfaces mediante herencia o clases anonimas, estas interfaces se pueden implementar mediante una expresion lambda o una referencia de metodo.  
Aqui hay un clon del estandar. **Function<T, R>** interfaz que demuestra la idea basica.

~~~java
@FunctionalInterface
interface Func<T, R> {
    R apply(T val); // Single abstract method
}
~~~

La anotacion **@FunctionalInterface** se usa para marcar interfaces funcionales y generar un error de tiempo de compilacion si una interfaz no cumple con los requisitos de una interfaz funcional. El uso de esta anotacion no es obligatorio pero si recomendable.  
La interfaz **Func<T, R>** cumple con los requisitos para ser una interfaz funcional porque tiene un solo metoto **apply**, que toma un valor del tipo **T** y retorna un resultado del tipo **R**.  
Este es un ejemplo de una interfaz lambda que implementa esta interfaz personalizada.

~~~java
Func<Integer, Integer> multiplier10 = n -> n * 10;
System.out.println(multiplier10.apply(5)); // 50
~~~

De manera similar, todas las funciones estandar se pueden representar como interfaces funcionales, incluidas **BiFunction<T, U, R>**. El concepto de interfaces funcionales es otra forma de modelar funciones en lugar de usar metodo regulares.  
Vale la pena notar, que los metodos **static** y **default**. Estan permitidos en las interfaces funcionales porque estos metodos no son abstractos.

~~~java
interface Func<T, R> {
    R apply(T val);

    static void doNothingStatic() { }

    default void doNothinByDefault() { }
}
~~~

## Implementacion de interfaces funcionales

Hay varias formas de implementar una interfaz funcional. Como sabe por la teoría de programación orientada a objetos anterior, es imposible crear directamente una instancia de una interfaz. Entonces, ¿qué debemos hacer?.  
En primer lugar, debemos implementar la interfaz para crear una clase concreta. Luego, crea una instancia de esta clase concreta. El requisito principal es implementar el método **apply** para obtener un comportamiento concreto.  
Consideremos tres maneras de hacerlo.  

1. Clases anonimas
    Por supuesto, como cualquier otra interfaz, una interfaz funcional puede implementarse usando una clase anónima o herencia regular.  
    Para implementar una interfaz funcional, vamos a crear una clase anónima y anular el método **apply**. El método anulado calcula el cuadrado de un valor dado:  

    ~~~java
    Func<Long, Long> square = new Func<Long, Long>() {
        @Override
        public Long apply(Long val) {
            return val * val;
        }
    };

    long val = square.apply(10L); // 100L
    ~~~

    En este ejemplo, modelamos una función matemática que eleva al cuadrado un valor dado. Este código funciona perfectamente, pero no está claro, ya que contiene muchos caracteres adicionales para realizar una sola línea de código útil.  
2. Expresion lambda
    Tambien se puede implementar e instanciar una interfaz funcional mediante una expresion lambda.  
    Aqui hay una expresion lambda que tiene el mismo comportamiento que la clase anonina anterior.

    ~~~java
    Func<Long, Long> square = val -> val * val;

    long val = square.apply(10L); // 100L
    ~~~

    El tipo de interfaz funcional (izquierda) y el tipo de lambda (derecha) son iguales desde una perspectiva semántica. Los parámetros y el resultado de una expresión lambda corresponden a los parámetros y el resultado de un **único método abstracto** de la interfaz funcional.
3. Referencias de metodos
    Otra forma de implementar una interfaz funcional es mediante el uso de referencias a métodos. En este caso, el número y tipo de argumentos y el tipo de retorno de un método deben coincidir con el número y tipos de argumentos y el tipo de retorno del único método abstracto de una interfaz funcional.  
    Supongamos que hay un metodo **square** que toma y retorna un valor **long**:

    ~~~java
    class Functions {

        public static long square(long val) {
            return val * val;
        }
    }
    ~~~

    El argumento y el tipo de retorno de este metodo se ajusta a la interfaz funcional **Func<Long, Long>**. Eso significa que podemos crear una referencia de metodo y asignarla a un objeto de **Func<Long, Long>**:

    ~~~java
    Func<Long, Long> square = Functions::square;
    ~~~

    Tenga en cuenta que el compilador crea una clase ocula intermedia que implementa la interfaz **Func<Long, Long>** de forma similar al caso de las expresiones lambda.  
Por lo general, las referencias a metodos son mas legibles que las expresiones lambda correspondientes. Trate de preferir esta forma de implementar e instanciar interfaces funcionales cuando sea posible.  

## Conclusion

Estos son los puntos clave de este tema:

- Una interfaz funcional es un tipo especial de interfaz que contiene un solo metodo abstracto, aunque tambien puede contener metodo **static** y **default**.
- Una interfaz funcional puede implementarse e instanciarse mediante una expresion lambda o una referencia de metodo.
- El compilador de Java crea automaticamente clases ocultas especiales para expresiones lambda y referencias a metodos.
- Es posible utilizar la anotacion **@FunctionalInterface** para forzar la verificacion de si una interfaz satisface el requisito de las interfaces funcionales.
- En Java, la programacion funcional se ha construido sobre la OOP, especialmente, interfaces y polimorfismo.

---

## Ejercicios

1. Escribe una expresion lambda que acepte tiene argumentos **String** y retorne una cadena con los argumentos concatenados en mayuscula.

    ~~~java
    class Seven {
       public static SeptenaryStringFunction fun = (s1, s2, s3, s4, s5, s6, s7) ->
            (s1 + s2 + s3 + s4 + s5 + s6 + s7).toUpperCase();
    }

    @FunctionalInterface
    interface SeptenaryStringFunction {
        String apply(String s1, String s2, String s3, String s4, String s5, String s6, String s7);
    }
    ~~~

2. En que casos podemos reemplazar una clase anonima con una expresion lambda? Todas las interfaces son de la biblioteca de clases estandar de Java.

    ~~~java
    Callable<String> callable = new Callable<String>() {
        @Override
        public String call() throws Exception {
            Thread.sleep(100);
            return "hello";
        }
    };

    // see NIO.2
    PathMatcher matcher = new PathMatcher() {
        @Override
        public boolean matches(Path path) {
            return false;
        }
    };
    ~~~

3. Selecciona las declaraciones correctas sobre los metodos de una interfaz funcional.

    - Puede contener metodos estaticos
    - Debe contener un solo metodo abstracto (no default)

4. Escribe tu propia interfaz funcional llamada **TernaryIntPredicate** y utilicelo con una expresion lambda. La interfaz debe tener un unico metodo no estatico (y no default) llamado **test** que acepta tres argumentos **int** y retorna una valor **boolean**. Escriba una expresion lambda que debe retornar si todos los valores pasados son diferentes

    ~~~java
    class Predicate {
        public static final TernaryIntPredicate ALL_DIFFERENT = (v1, v2, v3) ->
            v1 != v2 && v1 != v3 && v2 != v3;

        @FunctionalInterface
        public interface TernaryIntPredicate {
            boolean test(int val1, int val2, int val3);
        }
    }
    ~~~

5. Cual es el requisito para que una interfaz sea instanciada por una expresion lambda?

    - Contiene un unico metodo abstracto

6. Cual es una forma no valida de crear una instancai de una interfaz funcional?

    - Crear una instancia directa de esta interfaz

7. Suponga que esta escribiendo una nueva interfaz para procesar cadenas a traves de expresiones lambda. Tienes varias opciones para escribirla. Selecciona todos los que se pueden marcar con la anotacion **@FunctionalInterface**.

    ~~~java
    @FunctionalInterface
    interface StringOperator {
        String apply(String value);
    }

    @FunctionalInterface
    interface StringOperator extends Function<String, String> {

    }

    @FunctionalInterface
    interface StringOperator {
        String apply(String value);

        default boolean checkNotNull(String value) {
            return !Objects.isNull(value);
        }
    }
    ~~~

8. En que casos puede reemplazar una clase anonima con una expresion lambda? Todas las interfaces son de la biblioteca de clases estandar de Java.

    ~~~java
    // Metodos que son Single Abstract Methods
    Thread thread = new Thread(new Runnable() {
        @Override public void run() { 
            System.out.println("Hello!"); 
        } 
    });

    Comparator<String> comparator = new Comparator<String>() { 
        @Override public int compare(String s1, String s2) {
            return s1.compareTo(s2); 
        }
    };

    // see Swing 
    JButton button = new JButton("Click me"); 
    button.addActionListener(new ActionListener() { 
        @Override 
        public void actionPerformed(ActionEvent e) { 
            System.out.println("Button clicked"); 
        } 
    });
    ~~~
