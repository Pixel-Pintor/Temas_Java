# Bloque de inicializacion

Un bloque de *inicializacion estatico* es un bloque de codigo encerrado entre llave **{}** y precedido por la palabra **static**:

~~~java
static {
    // code
}
~~~

Se usa para inicializar campos *estaticos* y *constantes*, al igual que los constructores ayudan a inicializar campos de instancia. Podemos crear objetos e invocar metodos estaticos en un bloque estatico. Aqui hay un ejemplo:

~~~java
import java.util.Date;

public class StaticInitBlockExample {

    private static String stringField;
    private static Date dateField;
    private static final String A_STRING_CONSTANT;

    static {
        stringField = getEmptyString();
        dateField = new Date();
        A_STRING_CONSTANT = "unknown";
    }

    private static String getEmptyString() {
        return "empty";
    }
}
~~~

Una clase puede tener varios bloques estaticos que se ejecutaran en el orden en que aparecen en el codigo fuente. Los valores inicializados en el primer bloque se sobrescriben con los siguientes bloques.  
Pero la pregunta es, ¿que se realiza antes, la asignacion directa a campos estaticos o el bloque estatico?.  
Vea el siguiente ejemplo.

~~~java
public class StaticInitOrderExample {

    static int field = 30; // primera asignacion
    
    static {
        field = 50; // segunda asignacion
    }
}
~~~

En primer lugar, se realiza la asignacion directa al campo estatico. Despues de eso, se ejecuta el bloque estatico. Si imprime el valor **field**, sera igual a 50.  
Es imposible acceder a metodos y campos en un *bloque estatico*.  
Un bloque de inicializacion estatico se ejecuta una vez para todas la clase, no para instancia de la clase.

## Bloque de inicializacion de instancia

Tambien hay un *bloque de inicializacion de instancias*. Se utiliza para inicializar miembros de datos de instancia. Se ejecuta cada vez que se crea un objeto de la clase. Un bloque de inicializacion de instancia es un codigo encerrado entre llaves **{}**.

~~~java
class InstanceInitBlockExample {

    private int field;

    {
        field = 40;
    }
}
~~~

Por supuesto, tambie podemos asignar valores directamente a los campos:

~~~java
private int field = 40;
~~~

Pero si necesitamos realizar una logica mas compleja antes de invocar un constructor, es conveniente escribir un bloque de inicializacion de instancia. Por ejemplo, un bloque de inicializacion de instancias es util cuando necesitamos llenar una matriz:

~~~java
class ArrayInitExample {

    private int[] array;

    {
        System.out.println("Before the constructor");
        array = new int[10];
        for (int i = 0; i < array.length; i++) {
            array[i] = i * i;
        }
    }

    public void print() {
        for (int num : array) {
            System.out.println("%d", num);
        }
    }
}
~~~

El *bloque de inicializacion de instancias* se ejecuta antes de cualquier constructor de una clase (pero despues de los constructores de la superclase). El compilador de Java invoca el bloque como la primera declaracion en el constructor, antes de otras declaraciones.  
Todas las instancias de esta clase se inicializara durante la creacion. Hay un ejemplo:

~~~java
public class UsingArrayExample {

    public static void main(String[] args) {
        ArrayInitExample obj = new ArrayInitExample();
        obj.print();
    }
}

// salida del codigo:
Before de constructor
0 1 4 9 16 25 36 49 64 81
~~~

Puede escribir tantos bloques de inicializacion como necesite. Se realizaran en el orden en que aparecen en su codigo.

---

## Ejercicios

1. Cuales son las caracteristicas de los bloques de inicializacion de instancia (no estaticos)?

    - Estos bloques se ejecutan en el orden en que aparecen en el codigo fuente.
    - Puede acceder a los campos de instancia dentro de un bloque de inicializacion de instancia.

2. Aqui hay una clase que contiene multiples bloques de inicializacion estaticos. Que imprime el codigo.

    ~~~java
    class InitBlock {

        static int field = 20;

        static {
            field = 30;
        }

        static {
            field = 40;
        }
    }

    System.out.println(InitBlock.field); // 40
    ~~~

3. Hay una clase llamada **SomeClass**, cual es el valor del campo **field** al ejecutar el codigo.

    ~~~java
    class SomeClass {

        private static int field = 1;

        static {
            field = Integer.MAX_VALUE;
        }

        public SomeClass(int val) {
            field = val;
        }
    }

    SomeClass instance1 = new SomeClass(100);
    SomeClass instance2 = new SomeClass(200);

    // field = 200
    ~~~

4. Cuales son las caracteristicas de los *bloques de inicializacion estaticos*?

    - **static { }** define un nuevo bloque de inicializacion estatico.
    - Una clase puede tener varios bloques estaticos que se ejecutaran en el orden en que aparecen en el codigo fuente.

5. Hay un clase **A** que contiene varios bloques de inicializacion de instancia, cual es el orden de las salidas?

    ~~~java
    class A {

        {
            System.out.println("block A");
        }

        public A() {
            System.out.println("constructor");
        }

        {
            System.out.println("block B");
        }
    }

    // salida del codigo
    // 1) blockA; 2) block B; 3) constrctor
    ~~~

6. Cual es el valor del campo **field** al final del codigo.

    ~~~java
    class A {
        
        int field = 2;

        {
            field++;
        }

        public A() {
            field = 8;
        }

        {
            field++;
        }
    }

    A instance = new A();

    // field = 8
    ~~~

7. Tiene una clase con miembros estaticos, cual es el valor del campo estatico **str** durante el tiempo de ejecucion antes de crear una instancia de la clase y despues de crear una instancia.

    ~~~java
    public class AClass {

        private static String str = "a string";

        static {
            str = "another string";
        }

        public AClass() {
            str = "yet another string";
        }

        public static String getStr() {
            return str;
        }
    }

    // "another string", "yet another string"
    ~~~
