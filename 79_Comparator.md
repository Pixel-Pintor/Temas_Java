# Comparator

Cada vez que necesitamos ordenar una colección de datos, debemos comparar sus elementos entre sí para determinar cuál de ellos debe ir primero y cuál debe ir último. No es un gran problema si tenemos que comparar números o fechas, pero se vuelve un poco complicado para muchos otros ejemplos del mundo real, como estudiantes de escuela, publicaciones en redes sociales o cuentas bancarias. Aquí es donde **Comparator** viene al rescate, y vamos a discutir cómo usarlo en detalle en este tema.

## Tipos de datos personalizados

Vamos a crear una clase que modele un mensaje general. Para simplificar, asumimos que el contenido de tal mensaje puede ser representado por un **String**:

~~~java
class Message {

    private final String content;

    public Message(String content) {
        this.content = content;
    }

    public String getContent() {
        return content;
    }

    @Override
    public String toString() {
        return content;
    }
}
~~~

Ahora necesitamos poner algunos de esos mensajes en una coleccion para trabajar con ellos:

~~~java
List<Message> messages = new ArrayList<>();

messages.add(new Message("Hello"));
messages.add(new Message("humans!"));
messages.add(new Message("We"));
messages.add(new Message("came"));
messages.add(new Message("with"));
messages.add(new Message("peace!"));
~~~

Si tratamos de ordenar la lista de estos objetos **Message** obtendremos un error de compilacion, porque nuestra clase **Message** no admite ningun metodo que permita comparar sus instancias. Para resolver este problema, vamos a crear un **Comparator** que definira un orden de clasificacion para los objetos de la clase **Message**.

## Comparator

**Comparator<T>** es una interfaz generica que tiene un solo metodo abstracto **compare** y bastantes metodos no abstractos. Para crear un **Comparator**, necesitamos definir una clase que implemente la interfaz y anule su unico metodo abstracto.

~~~java
class MessageContentComparator implements Comparator {

    @Override
    public int compare(Message message1, Message message2) {
        // implementamos la comparacion
        return 0;
    }
}
~~~

Se espera que implementemos este metodo de acuerdo con las siguientes reglas:

- Deberia devolver 0 si ambos argumentos son iguales
- Deberia devolver un numero positivo si el primer argumento es mayor que el segundo
- Deberia devolver un numero negativo si el primer argumento es menor que el segundo.

De esta manera, incluso podemos anular el ordenamiento natural para cualquier tipo que implemente la interfaz **Comparable**.

En este ejemplo, queremos ordenar nuestros mensajes segun la longitud de su contenido.

~~~java
class MessageContentComparator implements Comparatos {

    @Override
    public int compare(Message message1, Message message2) {
        int firstLength = message1.getContent().length();
        int secondLength = message2.getContent().length();
        return Integer.compare(firstLength, secondLength);
    }
}
~~~

Aqui usamos el metodo estatico **compare** de la clase **Integer** para comparar con seguridad dos numeros **int**. Ordenemos la lista de mensajes usando **MessageContentComparator**.

~~~java
messages.sort(new MessageContentComparator());
messages.forEach(System.out.println);

// salida
We
Came
with
Hello
peace!
humans!
~~~

Los mensajes se han impreso en el orden de la longitud de sus contenido en lugar de en el orden en que se agregaron a la lista. Ademas, podemos tener multiples clases **Comparator** para nuestra clase y ordenar sus instancias usando diferentes ordenaciones dependiendo de nuestras necesidades.

## Multiples campos

Primero, extendamos la clase **Message** y luego ver como podemos implementar la ordenacion.

~~~java
class Message {

    private final String from;
    private final String content;
    private final LocalData created;
    private int likes;

    public Message(String from, String content, int likes, String created) {
        this.from = from;
        this.content = content;
        this.likes = likes;
        this.created = LocalDate.parse(created);
    }

    // getters and setters

    @Override
    public String toString() {
        return created.toString() + " " + from + " wrote: " +
                content + " (" + likes + ")";
    }
}
~~~

Despues de eso, necesitamos crear una nueva lista de mensajes para probar diferentes criterios de clasificacion.

~~~java
List<Message> messages = new ArrayList<>();

messages.add(new Message("Alien", "Hello humans!", 
        32, "2034-03-25"));
messages.add(new Message("Pirate", "All hands on deck!", 
        -2, "2034-01-05"));
messages.add(new Message("User765214", "Bump!", 
        1, "2033-02-17"));
messages.add(new Message("Unregistered", "This message was marked as spam", 
        -18, "2033-01-14"));
~~~

Ademas del comparador que ya tenemos, crearemos algunos mas.

~~~java
class MessageDateComparator implements Comparator<Message> {

    @Override
    public int compare(Message message1, Messsage message2) {
        return message1.getCreated().compareTo(message2.getCreated());
    }
}

class MessageAuthorComparator implements Comparator<Message> {

    @Override
    public int compare(Message message1, Message message2) {
        return message1.getFrom().compareTo(message2.getFrom());
    }
}
~~~

Ahora podemos usar estas clases para ordenar la lista de instancias de **Message** por diferentes criterios, por ejemplo:

~~~java
// Por Fecha
messages.sort(new MessageDateComparator());
// Salida
2033-01-14 Unregistered wrote: This message was marked as spam (-18)
2033-02-17 User765214 wrote: Bump! (1)
2034-01-05 Pirate wrote: All hands on deck! (-2)
2034-03-25 Alien wrote: Hello humans! (32)

// Por nombre del autor
messages.sort(new MessageAuthorComparator());
// Salida
2034-03-25 Alien wrote: Hello humans! (32)
2034-01-05 Pirate wrote: All hands on deck! (-2)
2033-01-14 Unregistered wrote: This message was marked as spam (-18)
2033-02-17 User765214 wrote: Bump! (1)
~~~

Como puede ver, con un adecuado **Comparator**, podemos aplicar cualquier logica de clasificacion a cualquier clase.

## Carateristicas de Java 8

Ya que **Comparator** tiene solo un único método abstracto (SAM) y, por lo tanto, es una interfaz funcional, podemos usar funciones lambda para crear instancias. Por ejemplo, en lugar de la declaración de clase completa, podemos reescribir **MessageDateComparator** como sigue:

~~~java
Comparator<Message> dateComparator = (m1, m2) ->
        m1.getCreated().compareTo(m2.getCreated());
messages.sort(dateComparator);
~~~

Incluso podemos evitar usar la declaracion con nombre y pasar el lambda directamente al metodo de clasificacion como argumento.

~~~java
messages.sort((m1, m2) -> m1.getCreated().compareTo(m2.getCreated()));
~~~

Si no a reutilizar un objeto **Comparator**, declararlo como una clase independiente seria innecesario, por lo que puede definirlo como una funcion lambda y usarla de inmediato.

## Metodos de Utilidad

**Comparator** también tiene varios métodos no abstractos que se pueden usar para combinar comparadores para crear condiciones complejas para comparar objetos. Echemos un vistazo a algunos de ellos.  
**Comparator.naturalOrder** devuelve **Comparator** del tipo aplicable que compara objetos **Comparable** en el orden natural. Esto significa que si la clase que desea comparar usando este método no implementa la interfaz **Comparable**, obtendrá un error de compilación.  
**Comparator.reverseOrder** similar al anterior, pero compara objetos **Comparable** utilizando el orden inverso al natural.  
**reversed** es llamado en un **Comparator** y devuelve una nueva **Comparator** que impone el orden inverso de los afectados.  
**Comparator.comparing** acepta una función que extrae un clave **Comparable** de ordenación y devuelve un **Comparator** que compara objetos por esa clave.  
**thenComparing** devuelve un comparador de orden lexicográfico con una función que extrae una clave **Comparable** de clasificación.  
Tenga cuidado al usar el método **reversed()**: invertirá toda la cadena de comparadores precedentes. Use paréntesis para limitar su alcance si es necesario.  
Aquí hay algunos ejemplos de cómo podemos usar estos métodos para ordenar una colección de acuerdo con nuestras necesidades.  
Por fecha, nuevo primero, usando una referencia de método para pasar la función extractora de clave de ordenación:

~~~java
message.sort(Comparator.comparing(Message::getCreated).reversed());

// salida
2034-03-25 Alien wrote: Hello humans! (32)
2034-01-05 Pirate wrote: All hands on deck! (-2)
2033-02-17 User765214 wrote: Bump! (1)
2033-01-14 Unregistered wrote: This message was marked as spam (-18)
~~~

Por el número de likes, en orden descendente, y luego, para mensajes con igual número de likes, por el nombre del autor, en orden ascendente:

~~~java
messages.sort(Comparator.comparing(Message::getLikes)
        .reversed()
        .thenComparing(Message::getFrom));
~~~

Y así. Pruébelo en una colección más grande para ver cómo funciona. De esta manera, puede implementar cualquier clasificación de mensajes en un tablero de mensajes. 

## Comparator vs Comparable

Ambas interfaces proporcionan una funcionalidad similar: permiten comparar objetos de la misma clase. ¿Cuál deberías elegir? Depende de muchos factores.  
**Comparable** define el orden natural de los objetos de una clase que lo implementa. Por tanto, encaja perfectamente en los casos en los que comparamos objetos que inherentemente tienen un cierto orden natural, como números o fechas. Si su clase tiene un orden natural obvio, entonces usando **Comparable** es el camino a seguir.  
En otros casos, cuando su clase tiene múltiples propiedades, por ejemplo, **name** y **age** o **price** y **userRating**, es posible que no pueda definir el orden natural de dichos objetos. Además, puede haber una situación en la que una clase cuyas instancias se van a comparar no implementa la interfaz **Comparable** y no puede modificar el código fuente de esa clase. En todos estos casos, la interfaz **Comparator** es su elección.  
También, **Comparator** sirve como una extensión y permite personalizar el proceso de clasificación. Con su ayuda, puede redefinir fácilmente el orden natural o agregar nuevas reglas para ordenar objetos. Además, hace posible combinar órdenes de clasificación para crear una lógica de clasificación compleja basada en diferentes propiedades de los objetos.

---

## Ejercicios

1. Cual sera la secuencia de numeros que imprimira el codigo.

    ~~~java
    List<Integer> numbers = Arrays.asList(12, 2, 13, 4, 15, 6);
    numbers.sort((i1, i2) -> !i1.equals(i2) ? 0 : i2 - i1);
    System.out.println(numbers);
    // [12, 2, 13, 4, 15, 6]
    ~~~

2. Implemente un metodo que tome una lista y la organice en orden lexicografico en reversa.

    ~~~java
    class Utils {

        public static void sortStrings(List<String> strings) {
            strings.sort(Comparator.reverseOrder());
        }
    }
    ~~~

3. Tiene una clase **User**, debe implementar el comparador **UserComparator** que se usa para ordenar los usuarios.

    ~~~java
    class User {
        private String name;

        public User(String name) {
            this.name = name;
        }

        public String getName() {
            return name;
        }

        @Override
        public String toString() {
            return name;
        }
    }

    class UserComparator implements Comparator<User> {

        @Override
        public int compare(User user1, User user2) {
            return user1.getName().compareTo(user2.getName());
        }
    }

    class Main {

        public static void main(String[] args) {

            User user1 = new User("Andres");
            User user2 = new User("Christian");
            List<User> users = new ArrayList<>();
            users.add(user2);
            users.add(user1);
            System.out.println(users); // [Christian, Andres]
            users.sort(new UserComparator());
            System.out.println(users); // [Andres, Christian]
        }
    }
    ~~~

4. Imagine que esta desarrollando una solucion backend para una tienda de videjuegos en linea y ha diseñado algunas entidades. Al implementar la logica comercial, eventualmente necesitara comparar y clasificar clientes, videojuegos y calificaciones. Seleccione cual interfaz de Java se ajusta mejor para comparar instancias de esas entidades.

    ~~~java
    class Rating {
        private int userRating;
    }

    class VideoGame {
        private String title;
        private List<String> keywords;
        private int copiesSold;
        private double price;
        private Rating rating;
    }

    class Customer {
        private String userName;
        private List<VideoGame> games;
        private Map<Rating, VideoGame> reviews;
        private boolean premium;
    }
    // Rating -> Comparable
    // VideoGame -> Comparator
    // Customer -> Comparator
    ~~~

5. Seleccione las declaraciones correctas sobre el metodo **compare**.

    - Retorna una entero positivo si el primer argumento es mayour que el segundo argumento
    - Retorna un numero entero negativo cuando el primer argumento es menor que el segundo.

6. Escriba un metodo que toma una lista de numeros enteros y retorne una lista ordenada, los numeros impares deben estar al principio en orden ascendente y los numeros pares deben estar al final en orden descendente.

    ~~~java
    class Utils {

        public static List<Integer> sortOddEven(List<Integer> numbers) {

            List<Integer> finalOrder = new ArrayList<>();
            List<Integer> oddNumbers = new ArrayList<>();
            List<Integer> evenNumbers = new ArrayList<>();
            for (Integer num : numbers) {
                if (num % 2 == 0)
                    evenNumbers.add(num);
                else
                    oddNumbers.add(num);
            }
            oddNumbers.sort(Comparator.naturalOrder());
            evenNumbers.sort(Comparator.reverseOrder());

            finalOrder.addAll(oddNumbers);
            finalOrder.addAll(evenNumbers);

            return finalOrder;
        }
    }
    ~~~

7. Debe ordenar una lista de usuario por su nombre, pero si tienen el mismo nombre, debe ordenarlos por su edad.

    ~~~java
    class User {
        private String name;
        private int age;

        public User(String name, int age) {
            this.name = name;
            this.age = age;
        }

        @Override
        public String toString() {
            return name + "=" + age;
        }

        public String getName() {
            return name;
        }

        public int getAge() {
            return age;
        }
    }

    class Utils {

        public static void sortUsers(List<User> users) {
            users.sort(Comparator
                    .comparing(User::getName)
                        .reversed()
                    .thenComparing(User::getAge)
                        .reversed()
            );
        }
    }
    ~~~
