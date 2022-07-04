# Spring Beans
A menudo necesitamos crear diferentes objetos en una aplicacion para usar sus funcionalidades. Algunos de ellos necesitan otros objetos como sus dependencias, que a su vez requieren otros objetos y asi sucesivamente. Spring ofrece una excelente manera de simplificar esta enorme y complicada cadena de creacion de objetos. Puede crear todos los objetos necesarios durante el inicio de la aplicacion y ponerlos todos en un contenedor. Luego, cada clase puede recuperar cualquier objeto que necesite de este contenedor.  
Estos objetos administrados por contenedores se conocen como **beans** y organizan la columna vertebral de su aplicacion. Se ven exactamente como objetos estandar de Java, pero pueden crearse durante el inicio de la aplicacion, registrarse y luego inyectarse en diferentes partes de una aplicacion mediante el contenedor.  

## Declarando Beans
Los beans generalmente se declaran en las clases que tienen la anotacion `@Configuration`, pero tambien es posible declararlos en la clase que contiene `@SpringBootApplication`.  
Para declarar un bean, necesita tener un metodo que contenga la anotacion `@Bean`. El resultado de ejecutar este metodo sera un bean gestionado por el contenedor IoC. Aqui hay un ejemplo de un bean simple declarado en una clase de configuracion:
~~~java
@Configuration
public class Addresses {

    @Bean
    public String address() {
        return "Green Street, 102";
    }
}
~~~
Esto significa que cuando inicie la aplicacion, habra un **Bean String** manejable llamado `address` que contiene el valor de `"Green Street, 102"`. Spring invoca automaticamente el metodo con la anotacion `@Bean` durante el inicio para inicializar todos los beans declarados.  
Por defecto los beans son **singletons**. Significa que solo hay un objeto para toda la aplicacion. Pero este comportamiento predeterminado se puede cambiar. Aprendera mas sobre esto en los siguientes temas.  
Por defecto, el nombre de un bean es el mismo que el nombre del metodo. Sin embargo, puede especificar un nuevo nombre en la anotacion, por ejemplo `@Bean("greenStreet)`. En este caso, el nombre del bean es `greenStreet`.  

## Beans con Autowired
Ahora que ha declarado un bean, puede usarlo para crear otros beans que dependan de el.  
El contenedor Spring IoC proporciona la **inyeccion de dependencias** que nos permite hacer eso. Un bean que tiene un tipo adecuado se puede inyectar automaticamente en un metodo anotado con `@Bean`. Tambien esta la anotacion `@Autowired` que marca un constructor, un campo o un metodo para ser inyectado por el DI de Spring.
~~~java
class Customer {
    private final String name; 
    private final String address;

    Customer(String name, String address) {
        this.name = name;
        this.address = address;
    }

    // getters
    @Override
    public String toString() {
        return "Customer{" +
                "name='" + name + '\'' +
                ", address='" + address + '\'' +
                '}';
    }
}
~~~
Esta clase incluye el campo `address` que vamos a obtener de nuestro bean anterior para crear una nuevo objeto `Customer`.  
Aqui hay un metodo que devuelve un objeto de esta clase como un bean. La anotacion `@Autowired` marca el parametro del metodo que se inyectara automaticamente.
~~~java
@SpringBootApplication
public class DemoSpringApplication {

    public static void main(String[] args) {
        SpringApplication.run(DemoSpringApplication.class, args);
    }

    @Bean
    public Customer customer(@Autowired String address) {
        return new Customer("Clara Foster", address);
    }
}
~~~
La inyeccion de dependencias de Spring inyecta el bean `address` en este metodo y este bean se puede utilizar para construir un nuevo objeto de la clase `Customer`. La inyeccion funciona porque el tipo de bean que necesitamos es el mismo que el tipo de bean producido anteriormente, y Spring Container puede inyectar ese bean. Incluso si el argumento tuviera otro nombre (p.ej. `addr`), este codigo funcionara como se esperaba.  
Quiza se pregunte como podemos estar seguros de que ambos metodos se invocar y los beans se crean correctamente. No hay necesidad de introducir ningun concepto nuevo, solo podemos crear el tercer bean temporal dependiendo de `Customer` e imprima el bean autocableado. Agregalo en la misma clase donde pusiste el bean anterior.
~~~java
@Bean
public Customer temporary(@Autowired Customer customer) {
    System.out.println(customer);
    return customer;
}
~~~
Ahora, si ejecuta la aplicacion, vera la informacion sobre el cliente en el registro.
~~~java
Customer{name='Clara Foster', address='Green Street, 102'}
~~~
Tenga en cuenta que Spring imprime muchos mensajes de registros y esta informacion estara entre ellos porque los beans se inicializan durante el inicio de la aplicacion. Significa que Spring IoC creo correctamente todos nuestros beans y los beans se inyectaron con exito.

## Distinguir Beans del mismo tipo
Como mencionamos antes, la ubicacion de un punto de inyeccion esta determinada por el tipo de bean. Pero, ¿y si tenemos varios beans del mismo tipo y queremos usar uno en particular? Afortunadamente, existe la anotacion `@Qualifier` que nos permite especificar el nombre del bean que necesitamos usar.
~~~java
@Bean
public String address1() {
    return "Green Street, 102";
}

@Bean
public String address2() {
    return "Apple Street, 15";
}

@Bean
public Customer customer(@Qualifier("address2") String address) {
    return new Customer("Clara Foster", address);
}

@Bean
public Customer temporary(@Autowired Customer customer) {
    // Customer{name='Clara Foster', address='Apple Street, 15'}
    System.out.println(customer); 
    return customer;
}
~~~
En este ejemplo, especificamos el nombre del bean que necesitamos usar para construir el cliente. El ultimo bean nombrado `temporary` se crea unicamente para imprimir la informacion durante el inicio de la aplicacion.  
Si eliminamos `@Qualifier` desde el metodo del cliente, la aplicacion no se iniciara y obtendremos un error de que hay varios beans que se pueden inyectar.
~~~java
Parameter 0 of method customer in org.hyperskill.beans.DemoSpringApplication 
required a single bean, but 2 were found:
- address1: defined by method 'address1' in org.hyperskill.beans.DemoSpringApplication
- address2: defined by method 'address2' in org.hyperskill.beans.DemoSpringApplication
~~~
Entonces, si ocurre este error, solo necesita determinar que bean desea usar y aplicar la anotacion `@Qualifier`.

## Beans vs Objetos estandar
Todavia puede usar objetos estandar Java creandolos manualmente siguiendo el enfoque de programacion orientada a objetos.
~~~java
String address = "Green Street, 102";
Customer customer = new Customer("Clara Foster", address);
~~~
En aplicaciones reales, los beans generalmente se usan para formar la columna vertebral de su aplicacion y separarla por capas y archivos de configuracion, pero la mayoria de los objetos de dominio son objetos estandar de Java.
---

## Ejercicios
1. Arregle el codigo para inyectar el metodo `minNumber`.
~~~java
@Bean
public Long maxNumber() {
    return 100_000_000_000L;
}

@Bean
public Long minNumber() {
    return 0L;
}

@Bean
public Long anotherNumber(@Qualifier("minNumber") Long number) {
    return number + 100;
}
~~~
2. Si no existe la anotacion `@Qualifier`, el bean a inyectar se determina por **tipo**.
3. Un bean puede ser inicializado en una clase anotado con `@Configuration` y `@SpringBootApplication`.
4. Tiene una clase llamada `Company`, un objeto de esa clase es creado como bean, donde puede usar la anotacion `@Autowired`.
~~~java
class Company {
    private final String name;
    private final List<String> employees;

    Company(String name, List<String> employees) {
        this.name = name;
        this.employees = employees;
    }
}

@Bean
public List<String> employees() {
    return List.of(
        "Lillia Barber",
        "Todd Mcloughlin",
        "Jasmine Wu"
    );
}

@Bean Company company(@Autowired List<String> employees) {
    return new Company("WorkProject", employees);
}
~~~
5. Aqui hay un grupo de beans que representan diferentes compañias. Cual sera autoconectado en el bean `person`?
~~~java
@Bean
public Person person(@Qualifier("company1") Company company) {
    return new Person("John", company);
}

@Bean
public String address() {
    return "Park Avenue, 15";
}
    
@Bean(name = "company3")
public Company company1(String address) {
    return new Company("GoodInc", address);
}

@Bean
public Company company2(String address) {
    return new Company("Noodle", address);
}

@Bean(name = "company4")
public Company company3(String address) {
    return new Company("Hyperskill", address);
}

@Bean(name = "company1")
public Company company4(String address) {
    return new Company("AI Robots", address);
}

// Inyecta AI Robots
~~~
