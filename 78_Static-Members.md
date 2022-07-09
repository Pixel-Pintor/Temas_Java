# Static Members

Todos los objetos de una clase tienen los mismos campos y metodos, pero los valores de los campos de objeto suelen ser diferentes. Al mismo tiempo, una clase tambien puede tener campos y metodos que son comunes para todos os objetos. Dichos campos y metodos se conocen como **miembros estaticos**, que se declaran con la palabra clave **static**.

## Variables de Clase

Una **variable de clase (campo estático)** es un campo declarado con la palabra clave **static**. Puede tener cualquier tipo primitivo o de referencia, como un campo de instancia normal. Un campo estático tiene el mismo valor para todas las instancias de la clase. Pertenece a la clase, en lugar de a una instancia de la clase.  
Si queremos que todas las instancias de una clase compartan un valor comun, por ejemplo, una variable global, es mejor declararlo como estatico. Esto puede ahorrarnos algo de memoria porque todos los objetos creados comparten una sola copia de una variable estatica.  
Se puede acceder a las variables estaticas directamente por el nombre de la clase. Para acceder a un campo estatico, debe escribir.

~~~java
ClassName.fieldName;
~~~

Veamos un ejemplo. Aqui hay una clase con dos variables estaticas publicas.

~~~java
class SomeClass {

    public static String staticStringField;
    public static int staticIntField;
}
~~~

Podemos establecer sus valores y obtenerlos.

~~~java
SomeClass.staticIntField = 10;
SomeClass.staticStringField = "it's a static member";

System.out.println(SomeClass.staticIntField); // 10
System.out.println(SomeClass.staticStringField); // it's a static member
~~~

Generalmente, no es una buena idea declarar campos **non-final public static**, esto solo es un ejemplo.  
Tambien podemos acceder al valor de un campo estatico a traves de una instancia de la clase.

~~~java
SomeClass.staticIntField = 30;
SomeClass instance = new SomeClass();
System.out.println(instance.staticIntField); // 30
~~~

Veamos un ejemplo mas complejo. Aqui hay una clase con un campo estatico llamado **lastCreated**. El campo almacena la fecha de la ultima instancia creada.

~~~java
public class SomeClass {

    public static Date lastCreated;

    public SomeClass() {
        lastCreated = new Date();
    }
}
~~~

El valor del campo estatico se cambia en el constructor de clases cada vez que se crea una nuevo objeto.  
El siguiente codigo crea dos instancias y genera resultados intermedios.

~~~java
System.out.println(SomeClass.lastCreated); // null

SomeClass instance1 = new SomeClass();
System.out.println(SomeClass.lastCreated); // Sun Aug 20...

Someclass instance2 = new SomeClass();
System.out.println(SomeClass.lastCreated); // Sun Aug 20
~~~

## Constantes de Clase

Campos estaticos con la palabra **final** son considerados **constantes de clase**, lo que significa que no se pueden cambiar. De acuerdo con la convencion de nomeclatura, los campos constantes siempre deben escribirse en mayusculas con un guion bajo para separar partes del nombre.  
La clase estandar **Math**, por ejemplo, contiene dos constantes estaticas.

~~~java
public static final double E = 2.7182818284590452354;
public static final double PI = 3.14159265358979323846;
~~~

Las constantes suelen ser publicas, pero no es una regla.  
Para ver como funcionan en un ejemplo, declaremos una clase llamada **Physics** con dos constantes estaticas.

~~~java
class Physics {

    /**
     * The speed of light in a vacuum (m/s)
     */
    public static final long SPEED_OF_LIGHT = 299_792_458;

    /**
     * Electron mass (kg)
     */
    public static final double ELECTRON_MASS = 9.10938356e-31;
}
~~~

Para usar las constantes, escribamos el siguiente codigo.

~~~java
System.out.println(Physics.ELECTRON_MASS); // 9.10938356E-31
System.out.println(Physics.SPEED_OF_LIGHT); // 299792458
~~~

Dado que esos campos son constantes, no podemos cambiar sus valores. Si intentamos hacerlo, obtendremos un error.

~~~java
Physics.ELECTRON_MASS = 10; // compile-time error
~~~

## Metodos de Clase

Una clase puede tener **metodos estaticos** asi como campos estaticos. Estos metodos tambien se conocen como **metodos de clase**. Se puede acceder a un metodo estatico por el momento de la clase y no requiere un objeto de clase.  
Los metodos estaticos se puede llamar directamente con el nombre de la clase. Para acceder a un metodo, debe escribir.

~~~java
ClassName.staticMethodName(args);
~~~

Un metodo estatico puede tener argumentos como un metodo de instancia regular o bien pueden no tener argumentos. Pero, a diferencia de los metodos de instancia, los metodos estaticos tienen varias caracteristicas especiales.

- Un metodo estatico puede acceder solo a campos estaticos y no puede acceder a campos no estaticos.
- Un metodo estatico puede invocar otro metodo estatico, pero no puede invocar un metodo de instancia.
- Un metodo estatico no puede referirse a la palabra **this** porque no hay ninguna instancia en el contexto estatico.

Sin embargo, los metodos de instancia pueden acceder a campos y metodos estaticos.  
Los metodos estaticos se utilizan a menudo como **metodos de utilidad** que son los mismo para todos el proyecto. Como ejemplo, puede crear una clase con solo metodos estaticos para realizar operacion matematicas tipicas.  
La biblioteca de clases de Java proporciona una gran cantidad de metodos estaticos para diferentes clases. Estos son solo algunos de ellos.

- La clase **Math** tiene muchos metodos estaticos, como **Math.min(a, b)**, **Math.abs(val)**, **Math.pow(x, y)** y asi.
- La clase **Arrays** tiene muchos metodos estaticos para procesar matrices como **toString(...)**.
- **Long.valueOf(...)**, **Integer.parseInt(...)**, **String.valueOf(...)** son metodos estaticos tambien.

Aqui hay una clase con un constructor, un metodo estatico y un metodo de instancia.

~~~java
public class SomeClass {

    public SomeClass() {
        invokeAnInstanceMethod(); // esto es posible
        invokeAStaticMethod(); // tambien es posible
    }

    public static void invokeAStaticMethod() {
        // es imposible invocar invokeAnInstanceMethod()
    }

    public void invokeAnInstanceMethod() {
        invokeAStaticMethod(); // esto es posible
    }
}
~~~

Este ejemplo muestra que puede invocar un metodo estatico desde el contexto de instancia (constructores y metodos de instancia), pero no puede invocar un metodo de instancia desde un contexto estatico.  
La unica forma de llamar a un metodo de instancia desde uno estatico es proporcionar una referencia a esta instancia como argumento. Tambien puede crear objetos de otras clases y llamar a sus metodos de forma similar. Aqui hay un ejemplo:

~~~java
public static void invokeAStaticMethod(SomeClass someClassInstance) {

    // llamando a un metodo de instancia desde un contexto estatico pasando una instancia como argumento
    someClassInstance.invokeAnInstanceMethod();

    // llamando metodos de instancia y estaticos de la instancia AnotherClass
    AnotherClass anotherClassInstance = new AnotherClass();
    anotherClassInstance.invokeAnotherClassIntanceMethod();
    anotherClassInstance.invokeAnotherClassStaticMethod();
}
~~~

Un ejemplo de un metodo estatico es el metodo **main**. Siempre debe ser estatico.

---

## Ejercicios

1. Selecciona las declaraciones correctas sobre los **miembros estaticos**.

    - Un campo estatico tiene el mismo valor para todas las intancias de una clase.
    - Un metodo estatico puede ser accesado por el nombre de la clase y no necesita un objeto.

2. Tiene un clase **MyClass** que contiene miembros estaticos y de instancia. Seleccione las declaraciones correctas.

    ~~~java
    class MyClass {
        
        static final String TEXT = "Hello";
        int magic = 10;
        static void doSomething() { }
        void doDo() {}
    }
    ~~~

    - En el cuerpo de **doDo** puede acceder al campo de **TEXT**
    - En el cuerpo de **doDo** puede invocar el metodo **doSomething**
    - En el cuerpo de **doSomething** puede acceder al campo **FIELD**

3. Encuentre las lineas del codigo que son incorrectas.

    ~~~java
    public class Test {
    
        public static final String DEFAULT_APPLICATION_NAME = "MyDemoApp";
        public static final int MAX_IMAGE_SIZE_KB = 4096;
    
        public static String hello = "Hello"; // (1)
    
        public static void main(String args[]) {
        
            System.out.println(DEFAULT_APPLICATION_NAME); // (2)
        
            MAX_IMAGE_SIZE_KB = 2048; // (3) - INCORRECTO
        
            printHello(); // (4) - INCORRECTO
        }
    
        private void printHello() {
            System.out.println(hello); // (5)
        }
    }
    ~~~

4. Jhon decidio crear una empresa de fabricacion. El le pide que escriba un controlador de fabricacion simple.

    1. **getNumberOfProducts** debera devolver el numero total de productos solicitados.
    2. **requestProduct** debe realizar un seguimiento de los productos solicitados y formatear el argumento del producto en el formato: **No. Requested Detail**.

    ~~~java
    class ManufacturingController {

        public static int totalProducts;

        public static String requestProduct(String product) {
            totalProducts++;
            return String.format("%d. Requested %s", totalProducts, product);
        }

        public static int getNumberOfProducts() {
            return totalProducts;
        }
    }

    class Main {

        public static void main(String[] args) {
            Scanner scanner = new Scanner(System.in);

            while (scanner.hasNextLine()) {
                String product = scanner.nextLine();
                System.out.println(ManufacturingController.requestProduct(product));
                System.out.println(ManufacturingController.getNumberOfProducts());
            }
        }
    }
    ~~~

5. Implementar una clase **Cat** y metodo estatico **getNumberOfCats**.

    ~~~java
    class Cat {

        static int counter = 0;
        String name;
        int age;
        final int thresholdAmountOfCats = 5;

        public Cat(String name, int age) {
            this.name = name;
            this.age = age;
            counter++;
            if (counter > thresholdAmountOfCats) {
                System.out.println("You have too many cats");
            }
        }

        public static int getNumberOfCats() {
            return counter;
        }
    }
    ~~~
