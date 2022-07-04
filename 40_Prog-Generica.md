# Introduccion a la programacion generica
Un tipo generico es una clase generica (o interfaz) que se parametriza sobre tipos. Para declarar una clase generica, necesitamos declarar una clase con la seccion de parametros de tipo delimitada por corchetes angulares `<>` siguiendo el nombre de la clase.  
En el siguiente ejemplo, la clase `GenericType` tiene un unico parametro de tipo llamado `T`. Suponemos que el tipo `T` es "algun tipo" y escribe el cuerpo de la clase independiente del tipo concreto.
~~~java
class GenericType<T> {

    // un campo de alguno tipo
    private T t;

    // toma un valor de "algun tipo" y lo asigna al campo
    public GenericType(T t) {
        this.t = t;
    }

    // retorna un valor de "algun tipo"
    public T get() {
        return t;
    }

    // toma un valor de "algun tipo" lo asigna al campo y lo retorna
    public T set(T t) {
        this.t = t;
        return this.t;
    }
}
~~~
Despues de declararse, un parametro de tipo se puede usar dentro del cuerpo de la clase como un tipo ordinario. Por ejemplo, el ejemplo anterior usa el parametro de tipo `T` como:
- un tipo para un **campo**
- un **constructor** tipo de argumento
- un **metodo de instancia** y tipo de retorno

El comportamiento de ambos metodos de instancia no depende del tipo concreto de `T`; puede tomar/devolver una cadena o un numero de la misma manera. Una clase puede tener cualquier numero de parametros de tipo. Por ejemplo, la siguiente clase tiene tres.
~~~java
class Three<T, U, V> {
    T t;
    U u;
    V v;
}
~~~
Pero la mayoria de las clases genericas tienen uno solo o dos parametros de tipo.

## La convencion de nomenclatura para parametros de tipo
Existe una convencion de nomenclatura que restringe las opciones de nombre de parametro de tipo a letras mayusculas individuales. Sin esta convencion, seria dificil diferencias entre una variable de tipo y un nombre de clase ordinario.  
Los nombres de parametros de tipo mas utilizados son:
- `T` - Tipo
- `S`, `U`, `V` etc. - 2nd, 3rd, 4th tipos
- `E` - elemento (usado en diferentes colecciones)
- `K` - key
- `V` - value
- `N` - Numero

## Creando objetos de clases genericas
Para crear una objeto de una clase generica (estandar o personalizado), necesitamos especificar el argumento de tipo despues del nombre del tipo.
~~~java
GenericType<Integer> obj1 = new GenericType<Integer>(10);
GenericType<String> obj2 = GenericType<String>("abc");

// se pueden inferir los tipos
GenericType<Integer> obj1 = new GenericType<>(10);
GenericType<String> obj2 = new GenericType<>("abc");
~~~
El argumento de tipo tiene que ser un tipo de referencia no un tipo primitivo.  
A partir de Java 10, podemos escribir `var` en lugar de un tipo especifico para forzar la inferencia automatica de tipos en funcion del tipo de valor asignado:
~~~java
var obj3 = new GenericType<>("abc");
~~~
Despues de haber creado un objeto con un argumento de tipo especifico, podemos invocar metodos de clase que toman o devuelven el parametro de tipo:
~~~java
Integer number = obj1.get(); // 10
String string = obj2.get(); // "abc"

System.out.println(obj1.set(20)); // imprime 20
System.out.println(obj2.set("def")); // imprime "def"
~~~
Si una clase tiene varios parametros de tipo, debemos especificarlos todos al crear instancia:
~~~java
GenericType<Type1, Type2, ..., TypeN> obj = new GenericType<>(...);
~~~

## Matriz generica personalizada
Como un ejemplo mas complicado, consideremos la siguiente clase que representa una matriz inmutable generica. Tiene un campo para almacenar elementos de tipo `T`, un constructor para establecer elementos, un metodo para obtener un elemento por su indice y otro metodo para obtener la longitud de la matriz interna. La clase es inmutable porque no proporciona metodos para modificar la matriz de elementos.
~~~java
public class ImmutableArray<T> {

    private final T[] items;

    public ImmutableArray(T[] items) {
        this.items = items.clone();
    }

    public T get(int index) {
        return items[index];
    }

    public int length() {
        return items.length;
    }
}
~~~
Esta clase muestra que una clase generica puede tener metodos que no usan un tipo de parametro en absoluto.  
El siguiente codigo crea una instancia de `ImmutableArray` para almacenar tres cadenas y luego enviar los elementos a la salida estandar.
~~~java
var stringArray = new ImmutableArray<>(new String[] {"item1", "item2", "item3"});

for (int i = 0; i < stringArray.length(); i++) {
    System.out.println(stringArray.get(i) + " ");
}
~~~
Es posible parametrizar `ImmutableArray` con cualquier tipo de referencia, incluidas matrices, clases estandar o sus propias clases.
~~~java
var doubleArray = new ImmutableArray<>(new Double[] {1.03, 2.04});

MyClass obj1 = ...., obj2 = ...; // suponga, tiene dos objetos de la clase personalizada

var array = new ImmutableArray<>(new MyClass[] {obj1, obj2});
~~~ 

Nosotros usamos `var` en los ejemplos anteriores para ahorra espacio. En lugar de usar `var`, podriamos haber especificado explicitamente el tipo, por ejemplo `ImmutableArray<String> stringArray = ...;` y asi.
---

## Ejercicios
1. Un parametro de tipo generico se puede usar como...
- El tipo de retorno de un metodo de instancia
- El tipo de un campo
- El tipo de argumento de un metodo de instancia
- El tipo de una variable local dentro de un metodo de instancia.
2. Imagina una clase generica que se puede en X para crear una instancia.
~~~java
public class Example<T> { .... }

Example<X> example = ...

// Class type (e.g. String)
// An array of primitive types (e.g. int[])
~~~
3. Implemente una clase `Box` universal que acomode cualquier cosa con un metodo `put()` y lo devuelva con `get()`.
~~~java
class Box<T> {

    private T t;

    public void put(T t) { this.t = t;}

    public T get() {
        return this.t;
    }
}
~~~
4. Corregir el codigo para que compile. Debe imprimir `Passed value: value`.
~~~java
class Main {
    public static void main(String... args) {
        Printer<String> printer = new Printer<>();

        printer.set("value");
        printer.print();
    }
}

class Printer<T> {
    private T value;

    public void set(T value) {
        this.value = value;
    }

    void print() {
        System.out.println("Passed value: " + value);
    }
}
~~~
5. Escriba la manera correcta de escribir la clase generica `C`.
~~~java
class C<E> { .... }
~~~
