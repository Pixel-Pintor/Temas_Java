# Post y Delete de datos a traves de REST
Cuando los usuarios reciben datos de aplicaciones web, es posible que deseen **agregar** nuevos o **eliminar** los datos existentes. Con `POST`, los usuarios pueden agregar nueva información enviando valores que desean cargar. `DELETE` permite a los usuarios eliminar los datos existentes de una aplicación.

## @PostMapping
Suponga que desea crear una aplicación en la que los usuarios puedan agregar nombres y direcciones de las personas que conocen. Para agregar una persona a la libreta de direcciones, un usuario debe enviar los datos al servidor, mientras que el servidor debe almacenarlos en algún lugar. Para hacerlo posible, implementa un `@PostMapping` en el `@RestController`.  
En nuestro ejemplo, queremos almacenar asignaciones de personas a direcciones, así que use un objeto `Map`. Nosotros podemos usar `ConcurrentHashMap` para implementar un hilo seguro `Map` en nuestra aplicación.
~~~java
@RestController
public class AddressController {
    private ConcurrentMap<String, String> addressBook = new ConcurrentHashMap<>();
}
~~~
Con `ConcurrentHashMap` podemos configurar un `@PostMapping` que toma el nombre y la dirección de una persona y los agrega a `Map`. Dado que el usuario desea enviar datos con una solicitud `POST`, necesitamos usar un `@RequestParam` para enviar los datos con una solicitud `POST`.  
`@RequestParam` es una **variable** proporcionada por un usuario en los **parámetros de consulta**. Se utiliza durante la manipulación de peticiones `POST`. `@RequestParam` puede proporcionarse de dos formas:
1. En la sección de parámetros de consulta de una solicitud REST. En Postman, se puede encontrar en la **Params**, etiquetada como **Query Params**;
2. En la ruta de la URL, en el siguiente formato: `localhost:<port>/<ApiPath>?<Param>=<value>&<Param>=<value>`.  
En los ejemplos a continuación, el puerto Spring está configurado en 8080, por lo que todos `POST` y `DELETE` las solicitudes se realizan en `localhost:8080`.  
Cuando proporcionamos un parámetro a través de los parámetros de consulta, debemos establecer un nombre y un valor. El nombre del parámetro debe coincidir con el nombre del `@RequestParam`, y el valor debe ser del mismo tipo que la variable `@RequestParam`. El siguiente código es un ejemplo de cómo `@RequestParam` se puede usar con `@PostMapping` para agregar los datos a la libreta de direcciones:
~~~java
@RestController
public class AddressController {
    private ConcurrentMap<String, String> addressBook = new ConcurrentHashMap<>();
    
    @PostMapping("/addresses")
    public void postAddress(@RequestParam String name, @RequestParam String address) {
        addressBook.put(name, address);
    }       
}
~~~
En este `@PostMapping`, esperamos dos `@RequestParam` con una solicitud El primero es el nombre del tipo `String`. La segunda es la dirección, también `String`. Cuando los usuarios envían un solicitud `POST` a la ruta `/addresses`, proporcionan dos parámetros en el cuerpo de la solicitud. Cuando se envía la solicitud, el nombre y la dirección se agregan a `ConcurrentHashMap`.  
Para probar si el `POST` fue un éxito, puede implementar una solicitud `GET` que devuelve una dirección solicitada basada en el nombre proporcionado: 
~~~java
@RestController
public class AddressController {
    private ConcurrentMap<String, String> addressBook = new ConcurrentHashMap<>();
    
    @PostMapping("/addresses")
    public void postAddress(@RequestParam String name, @RequestParam String address) {
        addressBook.put(name, address);
    }       
    
    @GetMapping("/addresses/{name}")
    public String getAddress(@PathVariable String name) {
        return addressBook.get(name);
    }
}
~~~
En la anterior solicitud `POST`, hemos añadido `Bob` que está asignado a `123 Younge Street`. Ahora bien, si enviamos una solicitud a `/addresses/Bob`, esperamos obtener `123 Younge Street`.  

## @DeleteMapping
Además de agregar nuevos datos, a veces los usuarios también necesitan eliminar algunos datos. En nuestra libreta de direcciones, es posible que queramos eliminar un nombre si ya no es necesario. En esta situación, podemos usar `@DeleteMapping` para enviar una solicitud para eliminar una parte de nuestros datos.  
Usando `@RequestParam` podemos pasar un parámetro al manejador `@DeleteMapping`. El parámetro que debe pasarse en nuestro ejemplo es el nombre de la persona que queremos eliminar. Una vez que se ha proporcionado el nombre, podemos eliminar el valor del `Map` y devolver un mensaje para indicar que se ha eliminado con éxito. 
~~~java
@RestController
public class AddressController {
    private ConcurrentMap<String, String> addressBook = new ConcurrentHashMap<>();

    @DeleteMapping("/addresses")
    public String removeAddress(@RequestParam String name) {
        addressBook.remove(name);
        return name + " removed from address book!";
    }
}
~~~
Para verificar que se eliminó la asignación, podemos enviar un `GET` para devolver el contenido de la variable `addressBook`. Eche un vistazo al fragmento a continuación. Muestra todo el controlador.
~~~java
@RestController
public class AddressController {
    private ConcurrentMap<String, String> addressBook = new ConcurrentHashMap<>();
    
    @PostMapping("/addresses")
    public void postAddress(@RequestParam String name, @RequestParam String address) {
        addressBook.put(name, address);
    }       
    
    @GetMapping("/addresses")
    public ConcurrentMap<String, String> getAddressBook() {
        return addressBook;
    }
    
    @DeleteMapping("/addresses")
    public String removeAddress(@RequestParam String name) {
        addressBook.remove(name);
        return name + " removed from address book!";
    }
}
~~~
Una vez `@DeleteMapping` se ha establecido, sólo tenemos que enviar una solicitud `DELETE` a la `/addressesURL` con la dirección que queremos eliminar en los parámetros de consulta. Para probar esto, primero completemos nuestro `Map` con datos Para hacer esto, podemos enviar algunas solicitudes `POST` a la aplicación web. Considere los siguientes dos `POST` peticiones: 
- `localhost:8080/addresses?name=Bob&address=123 Younge Street`
- `localhost:8080/addresses?name=Alice&address=200 Rideau Street`  
Esto agregará dos entradas al `Map`, el primero es `Bob` viviendo en `123 Younge Street`. el segundo es `Alice` viviendo en `200 Rideau Street`. Podemos verificar si las entradas se agregaron con una solicitud `GET` de `/addresses`.  
Una vez eliminados los datos, podemos verificar que la solicitud se ha completado con éxito enviando otra solicitud `GET` de todo `Map`. Como resultado, el valor de `Bob` se elimina de la `Map`.  
---

## Ejercicios
1. Complete el manejador con las etiquetas faltantes.
~~~java
@PostMapping("/{id}")
public void addDescription(@PathVariable int id, @RequestBody String description) {
    strings.add(id, description);
}
~~~
2. Complete el manejador con las etiquetas faltantes
~~~java
@PostMapping("/products/{id}")
public void addProduct(@PathVariable int id, @RequestParam String description) {
    productMap.add(id, description);
}
~~~
3. Cual anotacion se utiliza para habilitar los parametros de consulta en una solicitud `REST`?
~~~java
@RequestParam
~~~
4. Conecte la ruta de la solicitud con la descripcion del metodo.
~~~java
@GetMapping(path = "/movies/{movieId}/comments")
public List<String> getComments(@PathVariable String movieId)
// 1. GET /movies/7851305/comments

@GetMapping("/movies/{id}")
public Movie getMovie(@PathVariable String id)
// 2. GET /movies/1562481

@PostMapping("/movies/{movieId}/comments")
public void comment(@PathVariable String movieId, @RequestBody String comment)
// 3. POST /movies/5196724/comments

@PostMapping("/movies")
public void addMovie(@RequestBody Movie movie)
// 4. POST /movies
~~~
5. Complete el `POST` para que permita una solicitud a una ruta como la siguiente: `POST http://localhost:8080/addresses?name=Alice`.
~~~java
@RestController
public class AddressController {
    private ConcurrentMap<String, Integer> addressBook = new ConcurrenHashMap<>();

    @PostMapping("/addresses")
    public void postAddress(@RequestParam String name) {
        addressBook.put(name, 1);
    }
}
~~~
6. Complete el manejador `DELETE` para que permita una solicitud commo la siguiente: `DELETE http://localhost:8080/customers?id=1`.
~~~java
@RestController
public class CustomerController {
    private ConcurrentMap<String, String> customerMap = new ConcurrentHashMap<>();

    @DeleteMapping("/customers")
    public void deleteCustomer(@RequestParam String id){
        customerMap.delete(id);
    }
}
~~~
7. Complete el codigo
~~~java
@RestController
public class MovieController {
    private ConcurrentMap<String, String> movieMap = new ConcurrentHashMap<>();
    
    @PostMapping("/movies")
    public void postMovie(@RequestParam String name, @RequestParam int rating) {
        movieMap.put(name, rating);
    }       
}
~~~
8. Conecte cada solicitud con su tipo.
~~~java
@GetMapping("/movies/{movieId}")
public List<String> getComments(@PathVariable String movieId)
// Tipo GET

@PostMapping("/movies")
public List<String> getComments(@RequestParam String movieId)
// Tipo POST

@DeleteMapping(path = "/movies")
public List<String> getComments(@RequestParam String movieId)
// Tipo DELETE
~~~
9. Seleccione la solicitud `DELETE` que es valida para el metodo implementado en el codigo.
~~~java
@RestController
public class CustomerController {
    private ConcurrentMap<String, String> customers = new ConcurrentHashMap<>();
    
    @DeleteMapping("/customers")
    public String removeCustomer(@RequestParam String name) {
        customers.remove(name);
        return name + " customer removed!";
    }
}

// DELETE localhost:8080/customers?name=Bob
~~~
