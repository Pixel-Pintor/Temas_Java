## REST - Manejo de excepciones
Devolver errores a un usuario es crucial durante el desarrollo de aplicaciones web. Cuando los usuarios envían una solicitud incorrecta que no se puede procesar o desean obtener información sobre un objeto que no existe, su aplicación web debe informarles qué es lo que está mal. Hay diferentes códigos de estado HTTP generales, por ejemplo, **400** por **Bad Request** o **404** por **Not Found**. El manejo de errores es muy importante, ya que permite a los usuarios comprender qué es lo que está mal de inmediato.  
Aquí encontrará dos formas de devolver un mensaje de error en las aplicaciones Spring Boot. Puedes usar la Clase Spring **ResponseStatusException** o crea tu propia excepción usando la anotación **@ResponseStatus**. Escribamos un código simple y mostremos cómo funciona. 

## Preparacion del controlador
Imagina una aplicación web que devuelve información sobre un vuelo por su número. Se vería así en JSON: 
~~~json
{
  "id" : 3,
  "from": "Berlin Tegel",
  "to": "Stuttgart",
  "gate": "D80"
}
~~~
En el siguiente ejemplo, creamos un simple clase **FlightInfo** con información sobre el aeropuerto, la ciudad y la puerta de embarque. No proporcionamos la fecha y la hora del vuelo en aras de la brevedad:
~~~java
public class FlightInfo {

    private int id;
    private String from;
    private String to;
    private String gate;
 
    // constructor
 
    // getters and setters
 
}
~~~
Ahora podemos implementar un simple **FlightController** controlador con una lista de vuelos. También usaremos un método que devuelve un objeto **FlightInfo** de la lista **flightInfoList** para obtener información sobre el vuelo específico:
~~~java
@RestController
public class FlightController {

    private final List<FlightInfo> flightInfoList = new ArrayList<>();

    public FlightController() {
        flightInfoList.add(
                new FlightInfo(1, "Delhi Indira Gandhi", "Stuttgart", "D80"));
        flightInfoList.add(
                new FlightInfo(2, "Tokyo Haneda", "Frankfurt", "110"));
        flightInfoList.add(
                new FlightInfo(3, "Berlin Schönefeld", "Tenerife", "15"));
        flightInfoList.add(
                new FlightInfo(4, "Kilimanjaro Arusha", "Boston", "15"));
    }

    @GetMapping("flights/{id}")
    public FlightInfo getFlightInfo(@PathVariable int id) {
        for (FlightInfo flightInfo : flightInfoList) {
            if (flightInfo.getId() == id) {
                return flightInfo;
            }
        }
        throw new RuntimeException();
    }
}
~~~

## ResponseStatusException
La primera forma de devolver un error es usar la clase **ResponseStatusException** introducida en Spring 5 para el manejo básico de errores como parte de **org.springframework.web.serverpaquete**. Es **RuntimeException** y es por eso que no necesitamos agregarlo a la firma de un método. 
Hay tres constructores en Spring para generar **ResponseStatusException**.
~~~java
ResponseStatusException(HttpStatus status)
ResponseStatusException(HttpStatus status, java.lang.String reason)
ResponseStatusException(
        HttpStatus status, 
        java.lang.String reason, 
        java.lang.Throwable cause
)
~~~
Hemos creado una instancia proporcionando **HttpStatus** y, facultativamente, el motivo y la causa. El motivo es un mensaje simple que explica la excepcion. La causa es una excepcion anidada.  
Y qué tipos **HttpStatus** hay? Los más comunes son **200 OK**, **404 NOT_FOUND**, **400 BAD_REQUEST**, **403 FORBIDDEN**, y **500 INTERNAL_SERVER_ERROR**.  
Cambiemos nuestro método **getFlightInfo** y escribir un código que genere una instancia de **ResponseStatusException**. Supongamos que los usuarios buscan información sobre un vuelo desde el aeropuerto de *Berlín Schönefeld*, pero el aeropuerto está cerrado por mantenimiento. En esta situación, debemos volver **ResponseStatusException** con mensaje de estado **BAD_REQUEST** y un motivo:
~~~java
@GetMapping("flights/{id}")
public static FlightInfo getFlightInfo(@PathVariable int id) {
    for (FlightInfo flightInfo : flightInfoList) {
        if (flightInfo.getId() == id) {
            if (Objects.equals(flightInfo.getFrom(), "Berlin Schönefeld")) {
                throw new ResponseStatusException(HttpStatus.BAD_REQUEST, 
                        "Berlin Schönefeld is closed for service today");
            } else {
                return flightInfo;
            }
        }
    }

    throw new RuntimeException();
}
~~~
Por defecto, Spring Boot no incluye el campo **message** en una respuesta. Para habilitarlo, agregue esta línea en **application.properties**: **server.error.include-message=always**.  
Hablemos de los pros y los contras de **ResponseStatusException**.  
Tiene muchos beneficios, permitiendonos.
- procesar excepciones del mismo tipo por separador
- establecer diferentes codigos de estado para la respuesta
- evite crear clases de excepciones adicionales
- lanzar una excepcion en cualquier lugar.  
La desventaja es la duplicacion de codigo ya que tenemos que escribir el mismo codigo en varios controladores.  
Si su aplicación arroja una excepción no detectada como **RuntimeException** o cualquier otro que no tenga detalles explícitos en el código HTTP, se convertirá en **500 Internal Server Error**. Este código de estado indica que algo anda mal con su servidor y debe corregirse porque las solicitudes de los usuarios no se pueden procesar correctamente.  

## Excepciones personalizadas
Tambien es posibles establecer el codigo de respuesta y el estado de la excepcion personalizada. Podemos escribir una clase que extienda **RuntimeException** y agrega la anotacion **@ResponseStatus** a la excepcion como esta.
~~~java
@ResponseStatus(code = HttpStatus.BAD_REQUEST)
class FlightNotFoundException extends RuntimeException {
    
    public FlightNotFoundException(String cause) {
        super(cause);
    }
}
~~~
Ahora, podemos lanzar esta excepcion de la misma manera que **ResponseStatusException**. El estado se establecera automaticamente. Por ejemplo, en el controlador de vuelo.
~~~java
@GetMapping("flights/{id}")
public FlightInfo getFlightInfo(@PathVariable int id) {
    for (FlightInfo flightInfo : flightInfoList) {
        if (flightInfo.getId() == id) {
            return flightInfo;
        }
    }

    throw new FlightNotFoundException("Flight not found for id =" + id);
}
~~~
La principal ventaja es que podemos crear nuestras propias excepciones especificas y mantener nuestro codigo mas legible.  
Por otro lado, las excepciones personalizadas requieren la implementacion de clases adicionales.  

## Conclusion
Recuerde que un mal procesamiento de excepciones puede generar errores y baja legibilidad. Hemos considerado dos formas de manejar las excepciones en Spring. Ahora usted puede.
- lanzar **ResponseStatusException**
- crear excepciones personalizadas usando la anotacion **@ResponseStatus** y lanzarlos como **ResponseStatusException**  
Cada forma tiene sus ventajas y desventajas. Utilice la segunda opcion para excepciones especificas o la primera para evitar clases de excepciones adicionales.
---

## Ejercicios
1. Que mas se puede especificar junto al codigo de estatus con la anotacion **@ResponseStatus**
- **Reason**
2. Ordene las formas de maneja por el nivel de influencia. Por ejemplo, si la opcion es demasiado especifica, deberia ser la primera. Si la opcion influye en toda la aplicacion, sera la ultima.
- Lanzando **ResponseStatusException**
- Usando **@ExceptionHandler** en el controlador
- Usando **@RestControllerAdvice**
- No usando ninguna estrategia de manejo de excepciones.
3. Caracteristicas de los tipo de manejor de excepcion
- **ResponseStatus**: Requiere declarar clases adicionales y reduce la duplicacion de codigo.
- **ResponseStatusException**: Permite especificar un codigo de estatus por cada objeto de esta clase.
4. Como podemos re escribir el codigo sin usar una excepcion personalizada **FlightNotFoundException**.
~~~java
@ResponseStatus(code = HttpStatus.BAD_REQUEST)
class FlightNotFoundException extends RuntimeException {
    // ...
}

// Metodo que procesa la solicitud
@GetMapping("flights/{id}")
public FlightInfo getFlightInfo(@PathVariable int id) {
    for (FlightInfo flightInfo : flightInfoList) {
        if (flightInfo.getId() == id) {
            return flightInfo;
        }
    }

    // Forma original de manejar la excepcion
    throw new FlightNotFoundException("Flight not found for id =" + id);

    // Forma sin usar FlightNotFoundException
    throw new ResponseStatusException(HttpStatus.BAD_REQUEST, "Flight not found for id =" + id);
}
~~~
5. Cual codigo manejara la excepcion **IndexOutOfBoundsException**.
~~~java
@ExceptionHandler(IndexOutOfBoundsException.class)
public HashMap<String, String> handleIndexOutOfBoundsException(Exception e) {
    HashMap<String, String> response = new HashMap<>();
    response.put("message", "Handled by handleIndexOutOfBoundsException");
    response.put("error", e.getClass().getSimpleName());
    return response;
}
~~~
6. Complete el fragmento de codigo para aumentar la prioridad del manejador.
~~~java
@Order(Ordered.HIGHEST_PRECEDENCE)
@ExceptionHandler(ExecutionException.class)
public HashMap<String, String> handleExecutionException(Exception e){
   HashMap<String, String> response = new HashMap<>();
   response.put("message", "Handled by controller level handler");
   response.put("error", e.getClass().getSimpleName());
   Timestamp timestamp = new Timestamp(System.currentTimeMillis());
   response.put("timestamp", timestamp.getTime());
   return response;
}
~~~
7. Complete el codigo para hacer que maneje cada **runtime exception**.
~~~java
@ExceptionHandler(RuntimeException.class)
public HashMap<String, String> handleAllException(Exception e){
    HashMap<String, String> response = new HashMap<>();
    response.put("message", "Handled by handleAllException");
    response.put("error", e.getClass().getSimpleName());
    return response;
}
~~~
8. Necesita crear su propia excepcion. Quiere que todas su excepciones **YourExcepNameException** generen el mismo codigo **HttpStatus**, como puede hacerlo?
- Agregando **@ResponseStatus(code = HttpStatus.NOT_FOUND)**
9. Cual estatus es usado por defecto para excepciones **NPE** no manejadas?
- **500 Internal Server Error**
10. Complete el manejar para que pueda manejar excepciones de todos los controladores de la aplicacion.
~~~java
@RestControllerAdvice
public class ControllerExceptionHandler {
    
    @ExceptionHandler(Exception.class)
    public HashMap<String, String> handleNotFoundFlight(Exception e){
        HashMap<String, String> response = new HashMap<>();
        response.put("message", "The exception occured");
        response.put("error", e.getClass().getSimpleName());
        return response;
    }
}
~~~
11. Creamos una clase **Restaurant**, creamos **RestaurantController** con el metodo **getRestaurant** que retorna un objeto **Restaurant** por su id que sera buscado en **restaurantList**. Cambia el codigo para que pueda manejar las excepciones.
~~~java
public class Restauran {
    private String name;
    private String address;
    private String type;

    // constructor
    // getters and setters
}

@RestController
public class RestaurantController {

    private final List<Restaurant> restaurantList;

    public RestaurantController() {
        restaurantList = List.of(
                new Restaurant("Gandhi Cafe", "Germany, Stuttgart, Sonner str 8", "cafe"),
                new Restaurant("Aloha", "Spain, Valencia, Carrer de Vilafermosa 3", "bar")
        );
    }

    @GetMapping("restaurants/{id}")
    public Restaurant getRestaurant(@PathVariable int id) {
        return restaurantList.get(id); // change this
    }

}

// Primera forma de manejarlo
@GetMapping("restaurants/{id}")
public Restaurant getRestaurant(@PathVariable int id) {
    if (id < 0) {
        throw new ResponseStatusException(HttpStatus.BAD_REQUEST, "Id is incorrect");
    }
    return restaurantList.get(id);
}

// Segunda forma de manejarlo
@ResponseStatus(code = HttpStatus.BAD_REQUEST)
class RestaurantNotFoundException extends RuntimeException {
    // ...
}

// ...

@GetMapping("restaurants/{id}")
public Restaurant getRestaurant(@PathVariable int id) {
    if (id < 0) {
        throw new RestaurantNotFoundException("Id is incorrect");
    }
    return restaurantList.get(id);
}
~~~
12. Conecta las anotaciones con su caso de uso.
~~~java
@Order // Especifica la prioridad del manejador
@RestControllerAdvice // Especifica la clase o metodo que funciona como manejar globla
@ExceptionHandler // Especifica el metodo de manejo exacto
@RestHandler // Especifica el manejador para el controlador REST
@ResponseStatus // Establece el codigo de la respuesta.
~~~
13. Seleccione las ventajas de **ResponseStatusException**
- Evitar crear cualquier clase adicional de excepcion
- Procesa excepciones del mismo tipo separadamente