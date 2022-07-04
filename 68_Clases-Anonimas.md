# Clases Anonimas

A veces, los desarrolladores necesitan usar una clase pequeña que reemplaza algunos métodos de otra clase o interfaz solo una vez. En este caso, **declarar una nueva clase puede ser superfluo**. Afortunadamente, Java proporciona un mecanismo para crear una clase en una sola declaración sin tener que declarar una nueva clase con nombre. Estas clases se denominan **anónimas** porque no tienen identificadores de nombre como **String** o **MyClass** (pero tienen un nombre interno).

## ¿Qué es una clase Anónima?

Las clases anonimas le permiten declarar e instanciar una clase al mismo tiempo. Una clase anonima siempre implementa una interfaz o extiende otra clase (concreta o abstracta). Esta es la sintaxis comun para crear una clase anonima.

~~~java
new SuperClassOrInterfaceName() {

    // fields
    // overriden methods
};
~~~

La sintaxis de una clase anónima es una expresión. Y es similar a una llamada de constructor excepto que hay una definición de clase contenida en un bloque de código. Una clase anónima debe anular todos los métodos abstractos de la superclase. Es decir, todos los métodos de la interfaz deben anularse excepto los métodos predeterminados. Si una clase anónima extiende una clase que no tiene métodos abstractos, no tiene que anular nada.  

## Escribir clases anónimas

Supongamos que tenemos la siguiente inferfaz con dos metodos:

~~~java
interface SpeakingEntity {

    void sayHello();

    void sayBye();
}
~~~

Aqui hay una clase anonima que representa a una persona de habla inglesa:

~~~java
SpeakingEntity englishSpeakingPerson = new SpeakingEntity() {

    @Override
    public void sayHello() {
        System.out.println("Hello!");
    }

    @Override
    public void sayBye() {
        System.out.println("Bye!");
    }
};
~~~

La clase anónima se declara y se instancia al mismo tiempo, como una expresión. Anula ambos métodos de la interfaz.  
Asignamos una instancia de la **clase anónima** a la variable del tipo de interfaz. Ahora, podemos invocar métodos anulados:  

~~~java
englishSpeakingPerson.sayHello(); // Hello!
englishSpeakingPerson.sayBye(); // Bye!
~~~

Declaremos e instanciamos otra clase anonima:

~~~java
SpeakingEntity cat = new SpeakingEntity() {

    @Override
    public void sayHello() {
        System.out.println("Meow!");
    }

    @Override
    public void sayBye() {
        System.out.println("Meow!");
    }
};

cat.sayHello(); // Meow!
cat.sayBye(); // Meow!
~~~

Asi que, **englishSpeakingPerson** y **cat** son instancias de diferentes clases anonimas que implementan la misma interfaz.

## Acceso a variables de contexto

En el cuerpo de una clase anonima, es posible capturar variables de una contexto donde se define:

- una clase anonima puede capturar miembros de su clase envolvente (la clase externa)
- una clase anonima puede capturar variables locales que se declaran como **final** o son **efectivamente definitivas** (es decir, la variable no cambia pero no tiene la palabra clave **final**).  

Aqui hay otra clase anonima que implementa la interfaz **SpeakingEntity**.

~~~java
public class AnonymousClassExample {

    private static String BYE_STRING = "Auf Wiedersehen!"; // static constant

    public static void main(String[] args) {

        final String hello = "Guten Tag!"; // final local variable

        SpeakingEntity germanSpeakingPerson = new SpeakingEntity() {

            @Override
            public void sayHello() {
                System.out.println(hello); // it captures the local variable
            }

            @Override
            public void sayBye() {
                System.out.println(BYE_STRING); // it captures the constant field
            }
        };

        germanSpeakingPerson.sayHello();

        germanSpeakingPerson.sayBye();
    }
}
~~~

La clase anonima captura el campo constante. **BYE_STRING** y la variable final local **hello**. Este codigo se compila con exito e imprime lo que esperabamos.

~~~java
Guten Tag!
Auf Wiedersehen!
~~~

Una declaracion de una variable o un metodo en una clase anonima oculta cualquier otra declaracion en el ambito adjunto que tenga el mismo nombre. No puede acceder a ninguna declaracion sombreada por sus nombres.  

## Cuando usar clases anonimas

En general, debe considerar usar una clase anonima cuando:

- Solo se necesita una instancia de la clase.
- La clase tiene un cuerpo muy corto.
- La clase se usa justo despues de definirla.

En este tema, hemos considerado clases anonimas bastante simples para comprender la sintaxis basica, pero en las aplicaciones de la vida real, proporcionan un mecanismo poderoso para crear clases que encapsulan comportamientos y los pasan a metodos adecuados. Esta es una forma conveniente de interactuar con partes de nuestra aplicacion o con algunas bibliotecas de terceros.  

## Ejercicios

1. En las clases anonimas podemos capturar variables del contexto donde se define. Sin embargo, hay algunas consideraciones. Elige las que sean correcta.

    - Una clase anonima puede capturar variables locales que se declaran como finales o efectivamente finales.
    - Una clase anonima puede capturar a todos los miembros de su clave envolvente (una clase externa).
2. Escriba una interfaz **StringReverser**, debe declarar una clase anonima que implemente la interfaz y asignar la instancia a la variable **reverser**. Debe anular el metodo **reverse** y devolver la cadena de entrada alreves.

    ~~~java
    class Main {

        public static void main(String[] args) {

            Scanner scanner = new Scanner(System.in);
            String line = scanner.nextLine();

            // Forma 1
            StringReverser reverser = str -> {
                    StringBuilder strBuild = new StringBuilder(str);
                return strBuild.reverse().toString();
            };

            // Forma 2
            StringReverser reverser = new StringReverser() {
            @Override
            public String reverse(String str) {
                StringBuilder strBuild = new StringBuilder(str);
                return strBuild.reverse().toString();
            }
        };

            System.out.println(reverser.reverse(line));
        }

        interface StringReverser {

            String reverse(String str);
        }

    }
    ~~~

3. Cuales son las reglas para crear clases anonimas.
    - Una clase anonima se declara y se instancia en una sola instruccion.
    - Una clase anonima siempre implementa una interfaz o extiende una clase.
4. Java tiene una interfaz llamada **java.lang.Runnable** con el metodo **run**. El metodo no tiene argumentos y no devuelve nada. Debe implementar el metodo **createRunnable**, el metodo anulado debe imprimir un **text** en la salida un numero especifico de veces **repeats**.

    ~~~java
    class Create {

        public static Runnable createRunnable(String text, int repeats) {
            return new Runnable() {
                @Override
                public void run() {
                    for (int i = 0; i < repeats; i++) {
                        System.out.println(text);
                    }
                }
            };

            // lambda
            return () -> {
                for (int i = 0; i < repeats; i++) {
                    System.out.println(text);
                }
            };
        }
    }
    ~~~

5. Tienes una interface **AnInterface**, seleccione la forma correcta para crear una clase anonima.
    
    ~~~java
    new AnInterface() {

        @Override
        public void method() {
            // do something
        }        
    };
    ~~~

6. Tiene la interface **Returner**, debe crear una clase anonima que implemente la interfaz y que anule ambos metodos.

    ~~~java
    public class Main {

        public static void main(String[] args) {

            final Scanner scanner = new Scanner(System.in);
            final String str = scanner.nextLine();
            final int number = Integer.parseInt(scanner.nextLine());

            Returner returner = new Returner() {

                @Override
                public String returnString() {
                    return str;
                }

                @Override
                public int returnInt() {
                    return number;
                }
            };

            System.out.println(returner.returnString());
            System.out.println(returner.returnInt()); 
        }
    }

    interface Returner {

        public String returnString();

        public int returnInt();
    }
    ~~~