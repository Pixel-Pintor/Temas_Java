# Genericos y Objetos
Consideremos una clase generica llamada `GenericType` que almacena un valor de "algun tipo":
~~~java
class GenericType<T> {

    private T t;

    public GenericType(T t) {
        this.t = t;
    }

    public T get() {
        return t;
    }
}
~~~
Es posible crear un objeto con un tipo concreto (por ejemplo `String`):
~~~java
GenericType<String> instance1 = new GenericType<>("abc");
String str = instance1.get();
~~~
Tambien podemos crear instancias con otros tipos (`Integer`, `Character`) y luego invoque el metodo `get()` para acceder al campo interno. De esta manera, los genericos nos permiten usar la misma clase y metodos para procesar diferentes tipos.

## Reutilizando codigo con Object
Pero hay otra forma de reutilizar el codigo. Si declaramos un campo de tipo `Object`, podemos asignarle un valor de cualquier tipo de referencia.
~~~java
class NonGenericClass {

    private Object val;

    public NonGenericClass(Object val) {
        this.val = val;
    }

    public Object get() {
        return val;
    }
}
~~~
Ahora podemos crear una instancia de esta clase con la misma cadena que en el ejemplo anterios.
~~~java
NonGenericClass instance2 = new NonGenericClass("abc");
~~~
Tambien es posible crear una instancia pasando un valor de tipo `Integer`, `Character`, o cualquier otro tipo de referencia. Utilizando `Object` de esta manera nos permite reutilizar la misma clase para almacenar diferentes tipos de datos.

## La ventaja de los genericos: del tiempo de ejecucion al tiempo de compilacion
Despues de una invocacion del metodo `get()` obtenemos un `Òbject`, no un `String` o un `Integer`. No podemos obtener una cadena directamente del metodo.
~~~java
NonGenericClass instance2 = new NonGenericClass("abc");
String str = instance2.get(); // Compile-time error: Incompatible types
~~~
Para recupear la cadena, debemos realizar un type-cast explicito a la clase `String`.
~~~java
String str = (String) instance2.get(); // "abc"
~~~
Esto funciona porque se paso de una cadena a `instance2`. Pero, ¿y si la instancia no almacena una cadena? Si este es el caso, el codigo lanza una excepcion. Aqui hay un ejemplo:
~~~java
NonGenericClass instance3 = new NonGenericClass(123);
String str = (String) instance3.get(); // throws java.lang.ClassCastException
~~~
Ahora podemos ver la principal ventaja de los genericos sobre la clase `Object`. Debido a que no es necesario realizar una conversion de tipo explicita, nunca obtenemos una exception de tiempo de ejecucion. Si hacemos algo mal, podemos verlo en tiempo de compilacion.
~~~java
GenericType<String> instance4 = new GenericType<>("abc");
        
String str = instance4.get(); // no hay conversion aqui
Integer num = instance4.get(); // no compilara
~~~
Un error en tiempo de compilacion sera detectado por el programador, no por un usuario del programa. Debido a que los genericos permiten que el compilador se encargue de la conversion de tipos, los genericos son mas seguros y flexibles en comparacion con `Object`.

## Genericos sin especificar un argumento de tipo
Cuando crea una instancia de una clase generica, tiene la opcion de no especificar ningun tipo de argumento.
~~~java
GenericType instance5 = new GenericType("my-string");
~~~
En este caso, el campo de la clase `Object`, y el metodo `get()` devuelve un `Object`. El codigo anterior es equivalente a la siguiente linea:
~~~java
GenericType<Object> instance5 = new GenericType<>("abc");
~~~
Por lo general, no utilizara genericos parametrizados por `Object` debido a los mismos problemas que se presentaron anteriormente. Solo recuerda que esta posibilidad existe.
---

## Ejercicios
1. Eche un vistazo a la siguiente clase no generica. Cuando lanzara una **runtime exception**.
~~~java
class NonGenericClass {

    private Object val;

    public NonGenericClass(Object val) {
        this.val = val;
    }

    public Object get() {
        return val;
    }
}

NonGenericClass instance = new NonGenericClass("Hello");

// Integer num = (Integer) instance.get();
~~~
2. Cual es el resultado del codigo si `Box` es una clase generica.
~~~java
class Box<T> {
    T value;

    void set(T value) {
        this.value = value;
    }
}

Box<String> box = new Box();
box.set(10L);

// Compile-time: incompatible types: long cannot be converted to java.lang.String
~~~
3. Considera la clase generica, que retornara el metodo `get()`.
~~~java
class MyClass<T> {

    private T t;

    public MyClass(T t) {
        this.t = t;
    }

    public T get() {
        return t;
    }
}

MyClass instance = new MyClass("Hello"!);

// retorna un objeto de referencia
~~~
