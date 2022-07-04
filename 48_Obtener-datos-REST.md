# Obtener datos de REST
Las aplicaciones basadas en web se comunican con un servidor a traves de una **API**: varios metodos que se pueden procesar a traves de **HTTP**. Un **controlador** es una parte de la aplicacion que maneja estos metodos API.  

## RestController
La anotacion `@RestController` por lo general se encuentra en la parte superior de la clase. Hace que una clase proporcione puntos finales exactos para acceder a los metodos REST. La clase junto con los metodos de clase pueden indicar que solicitudes se adaptan a su caso. Todas las solicitudes apropiadas se enviaran al metodo especifico de esta clase.  
Supongamos que queremos crear una API. Cuando un usuario accede a una URL especifica, recibe `1`. Para hacerlo posible con Spring, implementaremos dos anotaciones. La primera anotacion es `@RestController` que se utiliza para manejar cualquier solicitud REST enviada por un usuario a la aplicacion. Para crear un `@RestController`, debemos crear una clase Java y anotarla con `@RestController`.
~~~java
@RestController
public class TaskController {

}
~~~
La anotacion `@RestController` es un contenedor de dos anotaciones diferentes:
1. `@Controller` contiene metodos de controlador para varias solicitudes. Ya que optamos por `@RestController`, los metodos estan relacionados con las solicitudes REST.
2. `@ResponseBody` contiene un objeto de cada metodo de controlador. Se representan en formato JSON. Cuando enviamos una solicitud, la respuesta que recibimos esta en formato JSON. Esto quedara claro cuando comencemos a trabajar con objetos en nuestas peticiones `GET`.  
Podemos implementar metodos para manejar varias solicitudes RESt en `@RestController`. Para implementar una solicitud `GET`, podemos usar una anotacion `@GetMapping`. Indica que ruta URL debe asociarse con la solicitud `GET`. Despues de eso, podemos implementar un metodo que se ejecuta cuando la solicitud `GET` se recibe en esa ruta. Por ejemplo, podemos crear una solicitud `GET` que regresa `1` cuando se accede a *http://localhost:8080/test*.
~~~java
@RestController
public class TaskController {

    @GetMapping("/test")
    public int returnOne() {
        return 1;
    }
}
~~~
 
## GET con Collections
Una lista es una buena manera de almacenar datos. A veces, queremos devolver una lista completa o un indice de lista especifico cuando se recibe una solicitud `GET`. Podemos ajustar nuestro `@GetMapping` par hacerlo.  
Necesitamos crear un objeto simple para almacenar en nuestra lista. Llamalo `Task`. Implementara un **constructor basico**, asi como **getters** y **setters** para cada una de las propiedades del objeto.
~~~java
public class Task {

    private int id;
    private String name;
    private String description;
    private boolean completed;

    public Task() {}

    public Task(int id, String name, String description, boolean completed) {
        this.id = id;
        this.name = name;
        this.description = description;
        this.completed = completed;
    }

    // getters and setters
}
~~~
Despues de eso, podemos implementar una coleccion para almacenar nuestras tareas. Vamos a utilizar una lista. Cuando trabajamos con Spring, podemos terminar enfrentandonos a muchas solicitudes `GET` al mismo tiempo. En este caso, seria una buena idea usar una coleccion inmutable para eliminar cualquier problema relacionado con subprocesos. Tambien debemos asegurarnos de que nuestra coleccion pueda ser utilizada por nuestra aplicacion.
~~~java
@RestController
public class TaskController {
    private final List<Task> taskList = List.of(
        new Task(1, "task1", "A first test task", false),
        new Task(2, "task2", "A second test task", true)
    );
}
~~~
En el fragmento anterior, hemos creado la lista `Task` y la relleno con tareas de muestra. Puede comenzar a trabajar con el objeto desde una consulta de base de datos de inmediato. Despues de eso, necesitamos crear un `@GetMapping` que se puede utilizar para recuperar datos de la coleccion de tareas.
~~~java
@RestController
public class TaskController {
    private final List<Task> taskList = List.of(
            new Task(1, "task1", "A first test task", false),
            new Task(2, "task2", "A second test task", true)
    );

    @GetMapping("/tasks")
    public List<Task> getTasks() {
        return taskList;
    }
}
~~~
Ademas de un `List`, tambien es posibles devolver otros tipos de colecciones desde un `RestController`. Como en el caso de una lista, un `Set` se convierte en una matriz JSON. Sin embargo un `Map` se convierte en una estructura clave-valor JSON.

## PathVariable
Es posible que deseemos modificar el codigo anterior para que los usuarios puedan ingresar una ID para especificar que tarea les gustaria recuperar. Para ello, tendremos que añador una anotacion `@PathVariable` a `@GetMapping`. El siguiente codigo muestra como podemos agregar una identificacion a nuestra funcion `getTask`.
~~~java
@RestController
public class TaskController {
    private final List<Task> taskList = List.of(
        new Task(1, "task1", "A first test task", false),
        new Task(2, "task2", "A second test task", true)
    );

    @GetMapping("/tasks/{id}")
    public Task getTask(@PathVariable int id) {
        return taskList.get(id - 1); // list indices start from 0
    }
}
~~~
Agregamos `{id}` al `@GetMapping` para decirlo a Spring que esperamos un parametro `id`. Podemos colocar la variable `id` como `@PathVariable` en los argumentos de nuestro metodo `getTask`. Despues de eso, la funcion devolvera solo un elementos en lugar de toda la coleccion.  
Si proporcionamos una identificacion no valida, el `get` arrojara una excepcion y recibiremos un **error 500**.  

## Personalizacion del codigo de estado
Por defecto, un metodo anotado con `@GetMapping` devuelve el codigo de estado `200 OK` en la respuesta si una solicitud ha sido procesada con exito y el codigo de estado `500` si hay una excepcion no detectada. Sin embargo, podemos cambiar este codigo de estado predeterminado devolviendo un objeot de la clase `ResponseEntity<T>` como resultado.
~~~java
@GetMapping("/tasks/{id}")
public ResponseEntity<Task> getTasks(@PathVariable int id) {
    return new ResponseEntity<>(taskList.get(id - 1), HttpStatus.ACCEPTED);
}
~~~
---

## Ejercicios
1. Complete el codigo para obtener un estudiate por su id.
~~~java
@RestController
public class StudentController {
    List<Student> studentList = List.of(
        new Student(0, "Jhon"),
        new Student(1, "Jane")
    );

    @GetMapping("/student/{id}")
    public Student getStudent(@PathVariable int id) {
        return studentList.get(id);
    }
}
~~~
2. Escribe la solicitud encima de cada manejador.
~~~java
@RestController
public class LibraryController {

    private List<Book> books = new ArrayList<>();

    // GET /books/145
    @GetMapping(path = "/books/{id}")
    public Book getBookById(@PathVariable int id) {
        return books.get(id - 1);
    }

    // GET /books
    @GetMapping(path = "/books")
    public Book getBook() {
        return books.get(0);
    }

    // GET /books/cinderella
    @GetMapping(path = "/books/{name}")
    public Book getBookByName(@PathVariable String name) {
        Book book;
        for (Book value : books) {
            book = value;
            if (book.getName().equals(name)) return book;
        }
        return null;
    }
}
~~~
3. Complete la anotacion que tenga como path variable el id.
~~~java
@GetMapping("/task/{id}")
public Task getTasks(@PathVariable int id) {
    return tasks.get(id);
}
~~~
4. Tiene una clase que representa una tarea, cual solicitud REST retorna la tarea con `id = 1` como valor de la lista.
~~~java
public class Task {
    private int id;
    private String name;
    private String description;
    private boolean completed;

    // getters and setters
}

@RestController
public class TaskController {
    List<Task> taskList = List.of(
        new Task(1, "task1", "A first test task", false),
        new Task(2, "task2", "A second test task", true)
    );

    @GetMapping("/tasks/{id}")
    public Task getTasks(@PathVariable int id) {
        return taskList.get(id);
    }
}


// GET tasks/0
~~~
5. Que solicitud debe enviarse para obtener la respuesta `"second line"`.
~~~java
@RestController("/lines")
public class LinesController {

    private List<String> strings;

    public LinesController() {
        strings = new ArrayList<>();
        strings.add("zero line");
        strings.add("first line");
        strings.add("second line");
    }
    
    @GetMapping("/{id}")
    public String getLine(@PathVariable int id){
        return strings.get(id);
    }

}

// GET /2
~~~
6. Complete la anotacion para mapear la ruta `/items`.
~~~java
@GetMapping("/items")
public List<Tasks> getTasks() {
    return tasks;
}
~~~
7. Crea un controlador que retorne una lista con todas las tareas en la ruta `GET /tasks/all`, debe responder con un codigo de estatus `200 (OK)`.
~~~java
class Task {
    private final String name;
    private final String description;

    Task(String name, String description) {
        this.name = name;
        this.description = description;
    }

    public String getName() {
        return name;
    }

    public String getDescription() {
        return description;
    }
}

@RestController
public class Controller {
    List<Task> tasks = List.of(
            new Task("Improve UI", "implement ..."),
            new Task("Color bug", "fix ...")
    );

    @GetMapping("/tasks/all")
    public ResponseEntity<List<Task>> getTasks() {
        return new ResponseEntity<>(tasks, HttpStatus.OK);
    }
}
~~~
8. Implemente un controlador con un endpoint `GET /welcome` que retorne `Welcome!`, codigo de estatus `200 (OK)`.
~~~java
@RestController
public class SimpleController {
    @GetMapping("/welcome")
    public ResponseEntity<String> getWelcome() {
        return new ResponseEntity<>("Welcome!", HttpStatus.OK);
    }
}
~~~
9. Implemente tres endpoints.
- `GET /value` retorna 1
- `GET /text` retorna el texto `two`
- `GET /json` retorn el siguiten JSON `{"number": 3}`
~~~java
@RestController
public class Controller {

    @GetMapping("/value")
    public ResponseEntity<Integer> getValue() {
        return new ResponseEntity<>(1, HttpStatus.OK);
    }

    @GetMapping("/text")
    public ResponseEntity<String> getText() {
        return new ResponseEntity<>("two", HttpStatus.OK);
    }

    @GetMapping("/json")
    public ResponseEntity<Map<String, Integer>> getJson() {
        return new ResponseEntity<>(Map.of("number", 3), HttpStatus.OK);
    }
}
~~~
10. Implemente un endpoint que reciba un ID y lo retorne.
~~~java
@RestController
public class Controller {
    @GetMapping("/{id}")
    public ResponseEntity<String> getInputID(@PathVariable String id) {
        return new ResponseEntity<>(id, HttpStatus.OK);
    }
}
~~~
11. Agrega las anotaciones necesarias para convertir los metodos en controladores Spring.
~~~java
@RestController
public class Controller {
    private final Map<Long, Long> secret_numbers = Map.of(
            1L, 214071234072L,
            2L, 234923493247L,
            3L, 949493939949L,
            4L, 383832238472L,
            5L, 222993947872L
    );

    @GetMapping("/secret_numbers/{id}")
    public Map<String, Long> getNumberById(@PathVariable long id) {
        long result = -1;

        if (secret_numbers.containsKey(id)) {
            result = secret_numbers.get(id);
        }

        return Map.of("secret_number", result);
    }
}
~~~
12. Modifique el programa para que soporte el endpoint `GET /task/{id}`, si la tarea no existe debe retornar `defaultTask`, en cualquier caso debe responder con el codigo `200 (OK)`.
~~~java
class Task {
    private final int id;
    private final String name;
    private final String description;

    Task(int id, String name, String description) {
        this.id = id;
        this.name = name;
        this.description = description;
    }

    public int getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    public String getDescription() {
        return description;
    }
}

@RestController
public class Controller {
    List<Task> tasks = List.of(
            new Task(405, "Improve UI", "implement ..."),
            new Task(406, "Color bug", "fix ...")
    );

    Task defaultTask = new Task(0, null, null);

    @GetMapping("/task/{id}")
    public ResponseEntity<Task> getTask(@PathVariable int id) {
        Task taskReturn = defaultTask;
        for (Task task : tasks) {
            if (task.getId() == id) {
                taskReturn = task;
            }
        }
        return new ResponseEntity<>(taskReturn, HttpStatus.OK);
    }
}
~~~
13. Debe retornar un JSON con la informacion de dos colores con el endpoint `GET /colors`. 
~~~java
@RestController
public class Controller {
    private final List<Color> colors = List.of(
            new Color("black", "hue", "primary", new Code(List.of(0, 0, 0, 1), "#000")),
            new Color("white", "value", "primary", new Code(List.of(255, 255, 255, 1), "#FFF"))
    );

    @GetMapping("/colors")
    public ResponseEntity<Map<String, List<Color>>> getColors() {
        return new ResponseEntity<>(Map.of("colors", colors), HttpStatus.OK);
    }
}


class Color {
    private String color;
    private String category;
    private String type;
    private Code code;

    public Color(String color, String category, String type, Code code) {
        this.color = color;
        this.category = category;
        this.type = type;
        this.code = code;
    }

    // getters and setters
}

class Code {
    private List<Integer> rgba;
    private String hex;

    public Code(List<Integer> rgba, String hex) {
        this.rgba = rgba;
        this.hex = hex;
    }

    // getters and setters
}
~~~
~~~json
{
   "colors":[
      {
         "color":"black",
         "category":"hue",
         "type":"primary",
         "code":{
            "rgba":[
               0,
               0,
               0,
               1
            ],
            "hex":"#000"
         }
      },
      {
         "color":"white",
         "category":"value",
         "type":"primary",
         "code":{
            "rgba":[
               255,
               255,
               255,
               1
            ],
            "hex":"#FFF"
         }
      }
   ]
}
~~~
14. Implementa un servicio que retorne informacion sobre los estudiantes.
~~~java
@RestController
public class Controller {
    Student s1 = new Student(1, "Jim");
    Student s2 = new Student(2, "Mike");
    Student s3 = new Student(3, "Dwight");
    List<Student> studentList = List.of(s1, s2, s3);

    @GetMapping("/students")
    public List<Student> getStudentList() {
        return studentList;
    }

    @GetMapping("/students/{id}")
    public Student getStudent(@PathVariable int id) {
        return studentList.get(id - 1);
    }
}

class Student {
    int id;
    String name;

    Student (int i, String n) {
        this.id = i;
        this.name = n;
    }

    // getters and setters
}
~~~
