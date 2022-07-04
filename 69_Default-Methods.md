# Metodos Predeterminados

Como probablemente recuerde, los métodos de interfaz son **abstractos** por defecto. Significa que no pueden tener un cuerpo, sino que simplemente declaran una firma. Sin embargo, algunos métodos pueden tener un cuerpo. Tales métodos se denominan **default** y están disponibles desde Java 8.  

## Metodos con un cuerpo
Los metodos predeterminados son opuestos a los abstractos. Tienen una implementacion:
~~~java
interface Feature {
    default void action() {
        System.out.println("Default action");
    }
}
~~~
Para indicar que un método es predeterminado, la palabra clave **default** está reservada. Recuerde que un método de interfaz se trata como **abstract** por defecto. Por lo tanto, debe indicar esto explícitamente poniendo la palabra clave **default** antes de los métodos con un cuerpo, de lo contrario, se produce un error de compilación.  
Aunque se implementan métodos predeterminados, no puede invocarlos directamente desde una interfaz como **Feature.action()**. Todavía necesita tener un objeto de una clase que implemente la interfaz:  

~~~java
class FeatureImpl implemens Feature {
    ...
}
...

Feature feature = new FeatureImple();
feature.action(); // Default action
~~~

Si desea personalizar un metodo predeterminado en una clase, anulelo como un metodo normal.

~~~java
class FeatureImpl implements Feature {
    public void action() {
        System.out.println("FeatureImpl especific action");
    }
}
...

Feature feature = new FeatureImpl();
feature.action(); // FeatureImpl-specific action
~~~

A veces, los metodos predeterminados son enormes. Para hacer posible la descomposicion de dichos metodos, Java permite declarar metodos privados dentro de una interfaz:

~~~java
interface Feature {
    default void action() {
        String answer = subAction();
        System.out.println(answer);
    }    

    private String subAction() {
        return "Default action";
    }
}
~~~

## ¿Por que son necesarios?

Como se menciono en el tema interfaz, la idea principal de una interfaz es declarar funcionalidad. Los metodos predeterminados amplian esa idea. No solo declaran la funcionalidad sino que tambien la implementan. La razon principal es admitir la compatibilidad con versiones anteriores. Consideremos un ejemplo.  
Suponga que programa un juego que tiene varios tipos de personajes. Estos personajes pueden moverse dentro de un mapa. Eso está representador por la interfaz **Movable**:

~~~java
interface Movable {
    void stepAhead();
    void turnLeft();
    void turnRight();
}
~~~

Entonces tenemos la interfaz y muchas clases que la implementan. Por ejemplo, un personaje **Batman**:

~~~java
class Batman implements Movable {
    public void stepAhead() {...}
    public void turnLeft() {...}
    public void turnRight() {...}
}
~~~

Una vez que decidas que los personajes deberían poder darse la vuelta. Significa que debes agregar un método **turnAround** a **Movable**. Puede implementar el método para todas las clases que implementen la interfaz. Otra forma es declarar un método **default** en la interfaz. Entonces no tienes que implementarlo en todas las clases.  
Otro ejemplo en el que la situación empeora aún más es cuando hablamos de interfaces que forman parte de la biblioteca estándar de Java. Supongamos que los mantenedores de Java decidieran mejorar una interfaz de uso común con un nuevo método en la próxima versión. Significa que si va a actualizar la versión de Java y hay clases que implementan la interfaz en su código, debe implementar el nuevo método. De lo contrario, su código no se compilará.  
A veces, los métodos predeterminados ayudan a evitar la duplicación de código. Efectivamente en nuestro caso los métodos **turnAround** pueden tener el mismo aspecto para todas las clases.  

~~~java
interface Movable {
    void stepAhead();
    void turnLeft();
    void turnRight();

    default void turnAround() {
        turnLeft();
        turnLeft();
    }
}
~~~

Si desea personalizar una implementacion predeterminada para **Batman**, simplemente anulelo:

~~~java
class Batman implements Movable {
    public void stepAhead() {...}
    public void turnLeft() {...}
    public void turnRight() {...}
    public void turnAround() {
        turnRight();
        turnRight();
    }
}
~~~

## El problema del diamante

Supongamos que tenemos otra interfaz **Jumpable** que representa la capacidad de saltar. La interfaz contiene métodos abstractos para saltar en el lugar, saltando girando a la izquierda y a la derecha. También tiene un método **default** para un salto de vuelta con la misma firma que **Movable**.

~~~java
interface Jumpable {
    void jump();
    void turnLeftJump();
    void turnRightJump();
    default void turnAround() {
        turnLeftJump();
        turnLeftJump();
    }
}
~~~

**Spiderman** tiene ambas habilidades **Movable** y **Jumpable**, por lo que su clase implementa ambas interfaces. Tenga en cuenta que las interfaces tienen el método **default** **turnAround** con la misma firma, pero diferentes implementaciones. ¿Cuál debería ser elegido para la clase? Para evitar la ambigüedad, el compilador obliga a un programador a proporcionar la implementación explícitamente; de ​​lo contrario, genera una excepción de compilación.  

~~~java
class Spiderman implements Movable, Jumpable {
    // define an implementation for abstract methods
    public void stepAhead() {...}
    public void turnLeft() {...}
    public void turnRight() {...}
    public void jump() {…}
    public void turnLeftJump() {...}
    public void turnRightJump() {...}

    // define an implementation for conflicted default method
    public void turnAround() {
        // define turnaround for the spiderman
    }
}
~~~

Tambien puede elegir una de las implementaciones predeterminadas en lugar de escribir la suya propia.

~~~java
class Spiderman implements Movable, Jumplable {
    ...
    public void turnAround() {
        Movable.super.turnAround();
    }
}
~~~

Este problema se conoce como **el problema del diamante**.

## Conclusion

Algunos métodos de interfaz tienen un cuerpo. Estos métodos se denominan predeterminados y tienen la palabra clave **default** antes de una firma. La idea principal de admitir métodos predeterminados es proporcionar compatibilidad con versiones anteriores. Eso le permite agregar nuevos métodos a la interfaz existente sin cambiar todas las clases que implementan la interfaz. Recuerde que si una clase implementa varias interfaces y algunas de ellas tienen un método predeterminado con la misma firma, debe definir la implementación del método en la clase. Esto sucede porque el compilador no puede decidir qué implementación debe usarse.  

---

## Ejercicios

1. Hay una interfaz **Flying** que tiene un metodo **getHeight** que devuelve la altura de vuelo en metros. Agregar e implementar un metodo predeterminado **getHeightInKm** que devuelve la altura en kilometros.

    ~~~java
    interface Flying {
        int getHeight();

        default int getHeightInKm() {
            return getHeight() / 1000;
        }
    }
    ~~~

2. Cual es razon principal para introducir metodos predeterminados?

    - Para proporcionar compatibilidad con versiones anteriores.  

3. Resuelve el problema del diamante de la clase **ConsoleWriter**. Anular el metodo **greeting** segun la implementacion de la interfaz **Printer**.  

    ~~~java
    class ConsoleWriter implements Printer, Notifier {
        public void greeting() {
            Printer.super.greeting();
        }
    }

    interface Printer {
        default void greeting() {
            System.out.println("Printer is ready");
        }
    }

    interface Notifier {
        default void greeting() {
            System.out.println("Notifier is ready");
        }
    }
    ~~~

4. Cual es el resultado de invocar el metodo **main**.

    ~~~java
    class Main {
        private static void main(String... args) {
            Speaker speaker = new Speaker();
            speaker.notify();
        }
    }

    class Speaker implements Notification, Alarm {}

    interface Notification {
        default void notify() {
            System.out.println("Hello World!");
        }
    }

    interface Alarm {
        default void notify() {
            System.out.println("Attention");
        }
    }

    // Error de compilacion: El compilador detecta ambiguedad
    ~~~

5. Define e implementa el metodo **print** en la interface **Printer** para que imprima un texto.

    ~~~java
    class Main {
        public static void main(String... args) {
            Printer printer = new ConsolePrinter();
            printer.print(); // prints: This is a default message
        }
    }

    class ConsolePrinter implements Printer {
    }

    interface Printer {

        default void print() {
            System.out.println("This is a default message");
        }
    }
    ~~~
