# Manejar solicitudes con cuerpos
Cuando queremos pasar información a un servidor de aplicaciones web, normalmente optamos por métodos **POST**. De hecho, podemos enviar información de cualquier tipo, incluso una cadena sin formato. Pero tendemos a trabajar con JSON, ya que es uno de los formatos más fáciles de trabajar. Los datos JSON se pueden convertir fácilmente en objetos con Spring, por lo que podemos trabajar con datos más complejos.   
Usaremos una anotación llamada **@RequestBody** para aceptar datos JSON a través de **@RestController**. Esta anotación puede enviar datos de un formato específico, incluido JSON, en el cuerpo de una solicitud. 

## Envio de un objeto al servidor
la anotación **@RequestBody** se utiliza en **@RestController** para enviar datos a una ruta a través del cuerpo de la solicitud. Un cuerpo de solicitud se puede utilizar para enviar datos en una variedad de formatos. De forma predeterminada, Spring espera datos en formato JSON , por lo que comenzaremos por ver cómo se pueden enviar los datos JSON usando un anotación **@RequestBody**.   
Primero, crearemos un objeto para representar los datos JSON que se enviarán a la aplicación. En este ejemplo, crearemos una aplicación que funcione con los datos del usuario, incluidos el nombre, la identificación, el número de teléfono y el estado de la cuenta del usuario. Nuestro objeto se puede configurar como se muestra a continuación: 
~~~java
public class UserInfo {

    private int id;
    private String name;
    private String phone;
    private boolean enabled;

    // getters and setters

    UserInfo() {}
}
~~~
Ahora, vamos a crear un simple solicitud **POST** que acepta una representación JSON del objeto **UserInfo**. los POSTLa solicitud devolverá el nombre de la cuenta del usuario y el estado de la cuenta: 
~~~java
@RestController
public class UserInfoController {

    @PostMapping("/user")
    public String userStatus(@RequestBody UserInfo user) {
        if (user.isEnabled()) {
            return String.format("Hello! %s. Your account is enabled.", user.getName());

        } else {
            return String.format(
                "Hello! Nice to see you, %s! Your account is disabled",
                user.getName()
            );
        }
    }
}
~~~
Cuando generamos nuestra solicitud **POST** en la ruta **/user**, necesitamos proporcionar un cuerpo de consulta que defina un objeto valido **UserInfo**. Estos datos se proporcionan para que cada propiedad del objeto se defina como una entrada al objeto JSON. Esto significa que la solicitud debe contener una identificación, nombre, teléfono y estado:
~~~json
{
    "id": 1,
    "name": "Scott",
    "phone": "555-555-5647",
    "enabled": true
}
~~~
Podemos enviar datos más complejos a nuestra aplicación con **@RequestBody**. Hay algunos otros formatos que podemos utilizar en la anotación **@RequestBody**. Los exploraremos en la siguiente sección.

## Envio de varios objetos
Es posible enviar múltiples objetos JSON en una sola solicitud usando una lista de objetos en nuestro **@RequestBody**.Para implementar una lista basada en **@RequestBody**, necesitamos cambiar el tipo de un solo objeto a una lista de objetos: 
~~~java
@RestController
public class UserInfoController {
    
    @PostMapping("/user")
    public String userStatus(@RequestBody List<UserInfo> userList) {
        return String.format("Added %d users", userList.size());
    }
}
~~~
En este ejemplo, nuestro **@RequestBody** ahora acepta una lista de **UserInfo**. Entonces, el JSON que enviamos al servidor ahora debería ser una lista de objetos JSON. Usa corchetes rectangulares **[]** para crear una lista de objetos JSON. Una lista contiene una secuencia de uno o más objetos JSON, como se muestra a continuación.
~~~json
[
    {
        "id": 1,
        "name": "Scott",
        "phone": "555-555-5647",
        "enabled": true
    },
    {
        "id": 2,
        "name": "Alice",
        "phone": "555-555-5643",
        "enabled": false
    }
]
~~~
Una vez que se ha enviado la solicitud, la matriz JSON se puede iterar y cada objeto se coloca en la lista **UserInfo**. En nuestro ejemplo anterior, mostramos la cantidad de objetos que se han pasado a través de la solicitud. 

## Formatos de datos adicionales
Además de las matrices JSON, también es posible utilizar un formato diferente. Por ejemplo, podemos usar XML para pasar objetos a través de nuestra anotación **@RequestBody**. Para hacer esto, necesitamos agregar **consumes = MediaType.APPLICATION_XML_VALUE** en la anotacion de **@PostMapping**: 
~~~java
@RestController
public class UserInfoController {
    
    @PostMapping(value = "/user", consumes = MediaType.APPLICATION_XML_VALUE)
    public String userStatus(@RequestBody UserInfo user) {
        return String.format("Added User %s", user);
    }
}
~~~
Utilizando el argumento **consumes**, es posible personalizar los datos que se envían a la solicitud **POST**. Cuando el **consumes** se agrega un argumento a la asignación, también necesitamos etiquetar el argumento de la ruta con **value**. Esto le permite a Spring distinguir entre los argumentos. Si no se proporciona el argumento **consumes**, será JSON de forma predeterminada. Hay muchos otros valores **MediaType**. Por ejemplo, **TEXT_PLAIN** se puede utilizar para texto sin formato , y **TEXT_HTML** se puede utilizar para formatos HTML. 
---

## Ejercicios
1. Que significa la anotacion **@RequestBody**.
- Signinifica que el argumento del metodo anotado se pasara como un cuerpo de solicitud.
2. La clase que representa un tipo de objeto debe contener
- setters para sus campos.
3. Complete el codigo para que acepte un cuerpo de solicitud.
~~~java
@RestController
public class UserInfoController {

    @PostMapping("/book")
    public String addBook(@RequestBody Book book) {
        return "Book added!";
    }
}
~~~
4. Tienes una solicitud JSON, complete el manejador para hacer que se permita el la solicitud.
~~~json
POST http://localhost:8080/movies
Content-Type: application/json
Cache-Control: no-cache

[
  {
    "id" : "974ks2h2e",
    "title" : "The Revenant",
    "budget" : 135000000
  },
  {
    "id" : "fnw73031s1",
    "title" : "The Shape of Water",
    "budget" : 20000000
  }
]
~~~
~~~java
@PostMapping(value = "/movies", consumes = "application/json")
String addMovies(@RequestBody List<Movie> movies) {...}
~~~
5. Complete el codigo para que acepte la solicitud JSON.
~~~json
POST http://localhost:8080/flights
Content-Type: application/json

{
    "id" : "1234",
    "destination" : "YYZ",
    "source": "MCO"
}
~~~
~~~java
@PostMapping(value = "/flights")
String addFlight(@RequestBody Flight flight) {...}
~~~
6. Mira el manejador y determina la solicitud JSON que funcionaria.
~~~java
@PostMapping(value = "/products")
public String addProduct(@RequestBody Product productToAdd)
~~~
~~~json
POST http://localhost:8080/products
Content-Type: application/json

{
    "id" : "290",
    "productType":"Paper"
}
~~~
7. Elije la clase correcta que representa el objeto JSON.
~~~json
{
  "id" : "1112hjkh3481soa7",
  "destination" : "London",
  "seat" : 143,
  "withLunch" : true
}
~~~
~~~java
public class TicketInfo {

    public String id;
    public String destination;
    public int seat;
    public boolean withLunch;

    TicketInfo() {}

    //setters and getters
}
~~~
8. Mira el manejador y elige la solicitud JSON que puede manejar.
~~~java
@PostMapping(value = "/employees")
public String greet(@RequestBody List<Employee> employees)
~~~
~~~json
POST http://localhost:8080/employees
Content-Type: application/json

[
  {
    "id" : "290",
    "firstName" : "Andrew",
    "lastName" : "Scott",
    "phone" : "570827"
  },
  {
    "id" : "832",
    "firstName" : "Sally",
    "lastName" : "Johnson",
    "phone" : "293818"
  }
]
~~~
9. Complete el manejador para que pueda recibir el objeto JSON.
~~~json
POST http://localhost:8080/movies
Content-Type: application/json

[
  {
    "id" : "974ks2h2e",
    "title" : "The Revenant",
    "budget" : 135000000
  },
  {
    "id" : "fnw73031s1",
    "title" : "The Shape of Water",
    "budget" : 20000000
  }
]
~~~
~~~java
@PostMapping(value = "/movies", consumes = "application/json")
String addMovies(@RequestBody List<Movie> movies) {...}
~~~
10. Observa el manejador y selecciona el objeto JSON que puede recibir.
~~~java
@PostMapping(value = "/employees", consumes = "application/json")
public String greet(@RequestBody List<Employee> employees)
~~~
~~~json
POST http://localhost:8080/employees
Content-Type: application/json
Cache-Control: no-cache

[
  {
    "id" : "290",
    "firstName" : "Andrew",
    "lastName" : "Scott",
    "phone" : "570827"
  },
  {
    "id" : "832",
    "firstName" : "Sally",
    "lastName" : "Johnson",
    "phone" : "293818"
  }
]
~~~
11. Podemos especifica que el formato JSON es aceptado con el siguiente codigo.
- **@PostMapping(consumes = "application/json")**
12. Elija las diferencias entre los metodos manejadores **GET** y **POST**.
- **GET** no puede tener una opcion **@RequestBody**
