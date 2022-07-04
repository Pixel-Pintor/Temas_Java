# Constructor
Los **constructores** son metodos especiales que inicializan un nuevo objeto de la clase. Se invoca un constructor de una clase cuando se crea una instancia usando la palabra clave `new`.  
Un constructor es diferente de otros metodos en que:  
- Tiene el mismo nombre que la clase que lo contiene
- No tiene un tipo de retorno (ni siquiera `void`)  
Los constructores inicializan **instancias** (objetos) de la clase. Establecen valores en los campos cuando se crea el objeto. Ademas los constructores pueden tomar parametros para inicializar campos por los valores dados.  
Aqui hay una clase llamada `Patient`. Un objeto de la clase tiene un nombre, una edad y una altura. La clase tiene un constructor de tres argumentos para inicializar objetos con valores especificos.
~~~java
class Patient {

    String name; 
    int age;
    float height;

    public Patient(String name, int age, float height) {
        this.name = name;
        this.age = age; 
        this.height = height;
    }
}
~~~
Avancemos mas y creemos algunas instancias de la clase usando el constructor que hemos escrito:
~~~java
Patient patient1 = new Patient("Heinrich", 40, 182.0f);
Patient patient2 = new Patient("Mary", 33, 171.5f);
~~~
---
## Constructor predeterminado y sin argumentos
El compilador proporciona automaticamente un constructor sin argumentos predeterminado para cualquier clase sin constructores.
~~~java
class Patient {
    String name;
    int age;
    float height;
}
~~~
Podemos crear una instancia de la clase. `Patient` utilizando el constructor predeterminado sin argumentos.
~~~java
Patient patient = new Patient();
~~~
Tambien podemos definir un constructor sin ningun argumento, pero usarlo para establecer valores predeterminados para los campos de una clase. Por ejemplo, podemos inicializar `name` con `"Unknown"`:
~~~java
class Patient {

    String name;
    int age;
    float height;

    public Patient() {
        this.name = "Unknown";
    }
}
~~~
Dichos constructores sin argumentos son utiles en los casos que cualquier valor predeterminado es mejor que `null`.
---
## Ejercicios
1. Clase `Book` con su constructor.
~~~java
class Book {

    String title;
    int yearOfPublishing;
    String[] authors;

    ppublic Book(String title, int yearOfPublishing, String[] authors) {
        this.title = title;
        this.yearOfPublishing = yearOfPublishing;
        this.authors = authors;
    }
}
~~~
2. Clase con constructor.
~~~java
class News {

    String title;
    String text;
    String[] tags;

    public News(String title, String text) {
        this.title = title;
        this.text = text;
        this.tags = new String[2];
    }
}

News news = new News("A title", "A text");
~~~
3. Clase Cuenta y su dueño.
~~~java
public class Main {


    public static void main(String[] args) {

        // create an instance of Account here
        Account account = new Account("123456", 1000, new User("demo-user", "Alexander", "Schmidt"));

        // pass it into process method
        process(account);
    }

    static class Account {

        final private String code;
        final private long balance;
        final private User owner;

        public Account(String code, long balance, User owner) {
            this.code = code;
            this.balance = balance;
            this.owner = owner;
        }

        public String getCode() {
            return code;
        }

        public long getBalance() {
            return balance;
        }

        public User getOwner() {
            return owner;
        }
    }

    static class User {

        final private String login;
        final private String firstName;
        final private String lastName;

        public User(String login, String firstName, String lastName) {
            this.login = login;
            this.firstName = firstName;
            this.lastName = lastName;
        }

        public String getLogin() {
            return login;
        }

        public String getFirstName() {
            return firstName;
        }

        public String getLastName() {
            return lastName;
        }
    }


    public static void process(Account account) {
        try {
            final Optional<User> owner = Optional.ofNullable(account.getOwner());

            System.out.println(account.getCode());
            System.out.println(account.getBalance());

            owner.ifPresent(o -> {
                System.out.println(o.getLogin());
                System.out.println(o.getFirstName());
                System.out.println(o.getLastName());
            });

        } catch (Exception e) {
            System.out.println("Something wrong...");
        }
    }
}
~~~
