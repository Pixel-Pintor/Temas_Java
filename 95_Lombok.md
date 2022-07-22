# Reduccion de codigo repetitivo con Lombok

El *Proyecto Lombok* proporcional anotaciones de Java adicionales que hacen que su codigo sea menos detallado. Una de las principales criticas al lenguaje Java siempre ha sido que requiere mucho codigo repetitivo, es decir, muchas secciones de codigo repetidas con variaciones minimas. Lombok brinda una solucion conveniente al reducir algunos de los codigos repetitivos mas comunes.

## Instalando Lombok

Instalar Proyecto Lombok es tan simple como agregarlo como *Maven* o *Gradle*. Para Maven necesita abrir *pom.xml* y pegar la dependencia entre las etiquetas **<dependencies>**.

~~~xml
<dependencies>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.20</version>
        <scope>provided</scope>
    </dependency>
</dependencies>
~~~

Usando Groovy, esta es la sintaxis:

~~~groovy
dependencies {
    compileOnly 'org.projectlombok:lombok:1.18.20'
    annotationProcessor 'org.projectlombok:lombok:1.18.20'

    testCompileOnly 'org.projectlombok:lombok:1.18.20'
    testAnnotationProcessor 'org.projectlombok:lombok:1.18.20'
}
~~~

O simplemente use un complemento de Gradle que haga lo mismo:

~~~groovy
plugins {
    id "io.freefair.lombok" version "6.0.0-m2"
}
~~~

Usando Kotlin, es casi lo mismo

~~~kotlin
dependencies {
    compileOnly("org.projectlombok:lombok:1.18.20")
    annotationProcessor("org.projectlombok:lombok:1.18.20")

    testCompileOnly("org.projectlombok:lombok:1.18.20")
    testAnnotationProcessor("org.projectlombok:lombok:1.18.20")
}
~~~

## @Data: la unica anotacion para gobernarlos a todos

Antes de profundizar en la teoria detras de Lombok, echemos un vistazo a una comparacion directa de una clase escrita de la manera estandar de Java, seguida de la misma clase escrita con anotaciones Lombok. Tenemos un clase **Dog** que tiene cuatro campos: nombre, raza, edad y color. Crearemos constructores, getters y setters predeterminados, ademas sobreescribiremos los metodos por defecto.

~~~java
public class Dog {

    String name;
    String breed;
    int age;
    String color;

    public Dog(String name, String breed, int age, String color) {
        this.name = name;
        this.breed = breed;
        this.age = age;
        this.color = color;
    }

    public Dog() {}

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getBreed() {
        return breed;
    }

    public void setBreed(String breed) {
        this.breed = breed;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getColor() {
        return color;
    }

    public void setColor(String color) {
        this.color = color;
    }

    @Override
    public String toString() {
        return "Dog{" +
                "name='" + name + '\'' +
                ", breed='" + breed + '\'' +
                ", age=" + age +
                ", color='" + color + '\'' +
                '}';
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;

        Dog dog = (Dog) o;

        if (age != dog.age) return false;
        if (!name.equals(dog.name)) return false;
        if (!breed.equals(dog.breed)) return false;
        if (!color.equals(dog.color)) return false;

        return true;
    }

    @Override
    public int hashCode() {
        int result = name.hashCode();
        result = 31 * result + breed.hashCode();
        result = 31 * result + age;
        result = 31 * result + color.hashCode();
        return result;
    }
}
~~~

No importa como los cortes, ese muro de codigo es un gran desastre. Reescribamoslo usando solo 3 anotaciones de Lombok.  
La anotacion **@Data** hace la mayor parte del trabajo, pero podemos agregar **@NoArgsConstructor** y **@AllArgsConstructor** para darnos un par de constructores adicionales con los que trabajar en caso de que los necesitemos.

~~~java
@Data
@AllArgsConstructor
@NoArgsConstructor
class Dog {
    String name;
    String breed;
    int age;
    String color;
}
~~~

Hemos cubierto que el modelo se genera durante un proceso de compilacion y se presenta solo en archivos **.class**, no **.java**. Significa que mientras escribe el codigo, todos los constructores/metodos repetitivos no estan disponibles para usted.

~~~java
@Data
class Product {
    private String name;
    private int price;
}

class Main {

    public static void main(String[] args) {
        Product p = new Product();
        p.setName("bread"); // no encuentra el metodo setName()
    }
}
~~~

No hay metodo **setName()**, se generara mas adelante en la compilacion. Todavia puede compilar y ejecutar el codigo usando Maven o Gradle directamente. Sin embargo, el problema trae inconvenientes y ralentiza el desarrollo. Esta es una razon por la que añadimos un complemente IDE especial, que genera el modelo de Lombok para IDE de forma interactiva sin esperar una compilacion.  
Ya hemos usado 3 anotaciones de Lombok, pero profundicemos y exploremoslas explicitamente. La anotacion **@Data** tiene un gran impacto porque es una forma abreviada de otras cinco anotaciones. Incluyen **@ToString**, **@EqualsAndHashCode**, **@Getter** en todos los campos, **@Setter** en todos los campos no finales, y **@RequiredArgsConstructor**. Exploremos brevemente cada uno.  
La anotacion **@ToString** nos da un **toString()** que retorna un string bien formateado con todos los campos contenidos en el. Di que quieres hacer un **Dog**, por lo que creo un Border Collie negro de cuatro años llamado Ralph, luego el metodo **toString()** devolveria una cadena como esta:

~~~java
Dog { name = 'Ralph', breed = 'Border Collie', age = 4, color = 'black' }
~~~

La anotacion **@EqualsHashCode** se explica por si misma, siempre que comprenda **hashCode()** y **equals()** de la clase **Object**. La version cora es que le permiten comparar objetos de su clase en funcion de los valores de sus campos, en lugar de solo verificar si tienen la misma referencia de objeto. La anotacion **@Getter** proporciona getters para cada campo, mientras que **@Setter** le proporciona setters para todos los campos que no se declaran **final**.

## Constructores: ¿Por que tantos?

Hay tres anotaciones de constructor diferentes en Lombok. La anotacion **@RequiredArgsConstructor** proporciona un constructor que solo se ocupa de los parametros para inicializar los campos que se *requieren*. Un campo se considera obligatorio si se declara final pero no se inicializa. O si esta anotado con **@NonNull**, otra anotacion que inyecta una verificacion null en el constructor. En pocas palabras, cualquier campo que pueda comenzar como null queda fuera de este constructor.  
A menudo, necesitara un constructor sin parametros: esto es epecialmente cierto si esta trabajando con un framework de base de datos como Hibernate, por ejemplo. El **@NoArgsConstructor** hara el trabajo por usted. Sin embargo, vale la pena señalar que **@NoArgsConstructor** potencialmente puede causar problemas si algunos campos son obligatorios (como se definio anteriormente). Puedes evitar eso usando **@NoArgsConstructor(force = true)**. Esto inicializara automaticamente los campos finales con 0, falso o null, segun sus tipos, y omite el registro nulo **@NonNull**.  
Finalmente, tenemos el **@AllArgsConstructor**, que como su nombre lo indica, proporcional un parametro para cada campo de la clase. Esto es util si desea crear objetos de su clase todos a la vez. Esto da como resultado menos codigo que usar un constructor basico y llenar los campos vacios usando setters.

---

## Ejercicios

1. Seleccione todas las anotaciones que generen metodos **hashCode** y **equals**.

    - **@Data**
    - **@EqualsAndHashCode**

2. Complete el codigo.

    ~~~java
    @Data
    @NoArgsConstructor(force = true)
    public class Dragon {

        private final String name;
        private final DragonType dragonType;
        private int currentLevel;
    }
    ~~~

3. Complete el codigo para que compile.

    ~~~java
    @AllArgsConstructor
    class Cat {

        String name;
        int age;
        String color;
    }

    Cat tiger = new Cat("Tiger", 3, "ginger");
    ~~~

4. Reescribe el codigo para que no sean necesarias tantas anotaciones lombok.

    ~~~java
    @Data
    public class Dog {

        public static void main(String[] args) {
            // ...
        }
    }
    ~~~

5. Como funciona lombok?

    - El procesador de anotaciones busca las anotaciones y luego empaquega el codigo repetitivo en archivos *.class* en el momento de la compilacion.

6. Ordene los constructores del que tiene menos argumentos al que tiene mas.

    - **@NoArgsConstructor**
    - **@RequiredArgsConstructor**
    - **@AllArgsConstructor**
