# Enums en Java

Cuando una variable solo puede tomar uno de un pequeño conjunto de valores posibles, es una buena idea usar **enum** en un programa. Enum es una palabra clave especial abreviatura de *enumeracion* que nos permite crear una lista de constantes agrupadas por sus contenido: temporadas, colores, estados, etc. Cuando almacenamos un monton de constantes en un solo lugar y las manejamos juntas, nos ayuda a evitar erroress, y hace que el codigo se vea mas legible y claro.

## Definicion de una Enumeracion

Podemos crear nuestra propia enumeracion de forma similar a la declaracion de clases. De acuerdo con la convencion del codigo Java, las constantes en una enumeracion se escriben en letras mayusculas. Todas las constantes deben estar separadas por comas. Eche un vistazo a la enumeracion de ejemplo **Season**:

~~~java
public enum Season {
    SPRING, SUMMER, AUTUMN, WINTER // cuatro instancias
}
~~~

En general, una enumeracion se puede considerar como una clase con instancias prefefinida. Aqui, tenemos cuatro instancias de estaciones. **SPRING**, **SUMMER**, **AUTUMN** y **WINTER** dentro de **Season**. Si queremos extender la lista de constantes, simplemente podemos agregar otra instancia en nuestra enumeracion: mediados de invierno, invierno australiano, etc. No olvide que en la vida real tienen que tener sentido.  

## Metodos para procesar Enumeraciones

Supongamos que tenemos que escribir un programa con una enumeracion que muestre tres posibles estados de usuario. Vamos a crear una enumeracion **UserStatus** con estos estados:

~~~java
public enum UserStatus {
    PENDING, ACTIVE, BLOCKED
}
~~~

Y ahora inicializamos una variable del tipo **UserStatus** del ejemplo anterior.

~~~java
UserStatus active = UserStatus.ACTIVE;
~~~

Cada valor de enumeracion tiene un nombre al que se puede acceder mediante al metodo **name()**:

~~~java
System.out.println(active.name()); // ACTIVE
~~~

A veces, es posible que necesitemos acceder a una instancia de enumeracion por su nombre. Esto se puede hacer con el metodo **valueOf()** que nos proporciona otra forma de inicalizar una variable:

~~~java
UserStatus blocked = UserStatus.valueOf("BLOCKED"); // BLOCKED
~~~

Una cosa importante para recordar acerca de este metodo es que distingue entre mayusculas y minusculas. Eso significa que si la cadena dada no coincide exactamente con ninguna constante, obtendremos una **IllegalArgumentException**.

~~~java
UserStatus blocked = UserStatus.valueOf("blocked"); // IllegalArgumentException, valueOf es case-sensitive
~~~

Si queremos ver todas las constantes de una enumeracion, podemos ponerlas en una matriz usando el metodo **values()**:

~~~java
UserStatus[] statuses = UserStatus.values(); // [PENDING, ACTIVE, BLOCKED]
~~~

Otro metodo llamado **ordinal()** devuelve la posicion ordinal de una instancia de una enumeracion:

~~~java
System.out.println(active.ordinal()); // 1 (iniciando con 0)
System.out.println(UserStatus.BLOCKED.ordinal()); // 2
~~~

Aunque una enumeracion es un tipo de referencia, dos variables se pueden comparar correctamente utilizando tanto el metodo **equals** y el operador **==**.

~~~java
System.out.println(active.equals(UserStatus.ACTIVE)); // true
System.out.println(active == UserStatus.ACTIVE); // true
~~~

## Enumeraciones en la sentencia Switch

Una **enumeracion** se puede utilizar con exito en la declaracion **switch**. Dependiendo del estado, nuestro programa puede realizar diferentes accioens indicadas por la declaracion **switch**. En este caso, imprime diferentes respuestas:

~~~java
UserStatus status = ... // some status
 
switch (status) {
    case PENDING:
        System.out.println("You need to wait a little.");
        break;
    case ACTIVE:
        System.out.println("No problems, you may pass here.");
        break;
    case BLOCKED:
        System.out.println("Stop! You can't pass here.");
        break;
    default:
        System.out.println("Unsupported enum constant.");
}
~~~

El mensaje que emite nuestro programa depende del valor de la variable **status**.

## Iterando sobre una Enumeracion

Una de las mejores formas de iterar sobre una enumeracion es usar un **for** o un ciclo **for-each**. Apliquemoslo a nuestras enumeracion de muestra:

~~~java
for (UserStatus status : UserStatus.values()) {
    System.out.println(status);
}

/*
Imprime:
PENDING
ACTIVE
BLOCKED
*/
~~~

Aqui, usamos el metodo **values()** para devolver una matriz de valores de enumeracion. Este ciclo es util cuando se itera sobre enumeraciones con una gran cantidad de constantes.

---

## Ejercicios

1. Declare un enum que represente los dias de la semana, e imprimalos.

    ~~~java
    enum DayOfWeek {
        SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY
    }

    public class Main {

        public static void main(String[] args) {
            for (DayOfWeek day : DayOfWeek.values()) {
                System.out.println(day);
            }
        }
    }
    ~~~

2. Que va a imprimir el codigo.

    ~~~java
    public enum Commands {
        SIT, DOWN, STAY, COME
    }

    System.out.println(Commands.STAY.ordinal());
    // 2
    ~~~

3. Tiene un enum **Zodiac**, tiene dos instancias, ejecute el codigo y escriba el resultado.

    ~~~java
    public enum Zodiac {
        ARIES,
        TAURUS,
        GEMINI,
        CANCER,
        LEO,
        VIRGO,
        LIBRA,
        SCORPIO,
        SAGITTARIUS,
        CAPRICORN,
        AQUARIUS,
        PISCES
    }

    Zodiac capricorn = Zodiac.CAPRICORN;
    Zodiac leo = Zodiac.LEO;

    System.out.println(capricorn.name()); // CAPRICORN
    // Asigna Zodiac.TAURUS 
    Zodiac taurus = Zodiac.valueOf("TAURUS");
    // IllegalArgumentException
    Zodiac virgo = Zodiac.valueOf("virgo"); 
    // imprime false
    System.out.println(leo.equals(Zodiac.TAURUS));
    // imprime 10
    System.out.println(Zodiac.AQUARIUS.ordinal());
    ~~~

4. Declare un **enum** que incluya concurrencias.

    ~~~java
    enum Currency {
        USD, EUR, GBP, RUB, UAH, KZT, CAD, JPY, CNY
    }
    ~~~

5. Tiene un enum **Role** con dos constantes, **ADMIN** y **USER**, cuales lineas imprimen **true**.

    ~~~java
    enum Role {
        ADMIN, USER
    }

    Role role1 = Role.ADMIN;
    Role role1 = Role.ADMIN;
    Role role3 = Role.USER;

    System.out.println(role1 == Role.ADMIN);  // true
    System.out.println(role1.equals(role2)); // true
    System.out.println(role1 == role2);    // true
    System.out.println(role1 == role3);    // false
    ~~~

6. Por qué usamos enumeraciones en los programas?

    - La enumeracion ayuda a evitar errores en el codigo
    - Un tipo de enumeracion permite que una variable sea un conjunto de constantes definidas
    - Enum hace que el codigo sea legible y conciso

7. Tiene una enumeracion **Secret**, escriba un programa que cuente e imprima cuantas constantes comienzan con *"STAR"*.

    ~~~java
    enum Secret {
        STAR, CRASH, START, // ...
    }

    public class Main {

        public static void main(String[] args) {
            
            int counter = 0; 

            for (Secret secret : Secret.values()) {
                if (secret.name().startsWith("STAR")) {
                    counter++;
                }
            }
            System.out.println(counter);
        }
    }
    ~~~
