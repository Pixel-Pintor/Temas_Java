# Campos y Metodos en Enumeraciones

Usamos enumeraciones para definir conjuntos de variables inmutables. Despues de definirlos, es posible que necesitemos ampliar la funcionalidad de la enumeracion y agregar valores a las constantes. Al igual que una clase, una enumeracion puede tener campos, constructores y metodos. Es por eso que una enumeracion es util cuando se trabaja con valores que no se van a cambiar.

## Enumeracion de muestra

Supongamos que tenemos que escribir un programa que muestre el nivel de bateria de un telefono inteligente, un banco de energia o cualquier dispositivo con una escala discreta.  
En primer lugar, creemos una enumeracion con varios niveles de umbral que representen el nivel de carga de la bateria.

~~~java
public enum ChargeLevel {
    FULL, HIGH, MEDIUM, LOW
}
~~~

Supongamos que necesitamos visualmente el nivel de carga de la bateria. Queremos que se divida en varios segmentos que tengan una indicacion de color.  
Para hacer esto, agregaremos los campos y valores correspondientes a nuestra enumeracion. Cuando los definimos, debemos proporcionar valores al constructor de la enumeracion. Aqui, creamos un constructor en el enum **ChargeLevel** y agrego dos capos **sections** y **color**. Ademas, hay dos metodos, **getSections()** y **getColor()** que devuelven los valores de los campos respectivamente.

~~~java
public enum ChargeLevel {

    FULL(2, "green"),
    HIGHT(3, "green"),
    MEDIUM(2, "yellow"),
    LOW(1, "red");

    int sections;
    String color;

    ChargeLevel(int sections, String color) {
        this.sections = sections;
        this.color = color;
    }

    public int getSections() {
        return sections;
    }

    public String getColor() {
        return color;
    }
}
~~~

Tenga en cuenta que todas las instancias de enumeracion son creadas por la JVM de la misma manera que un campo estatico de una clase. Esta es la razon por la que una enumeracion *no puede* conteneer un constructor publico. Esto siginifica que *no podemos* crear objetos de enumeracion invocando un constructor de enumeracion con la palabra **new** pero tiene que elegir una de las instancias predefinidas en su lugar. Tenga en cuenta que si su enumeracion contiene campos y metodos, siempre debe definirlos *despues* de la lista de constantes en la enumeracion.  
Ahora tenemos una clase con informacion adicional reunida en un solo lugar: la cantidad de secciones para resaltar y el color.

~~~java
System.out.println(ChargeLevel.LOW.sections); // 1
System.out.println(ChargeLevel.LOW.color); // red
~~~

Es posible extender un enum agregando metodos estaticos personalizados. Por ejemplo, vamos a agregar un metodo que encuentre una instancia de **ChargeLevel** por el numero dado de secciones.

~~~java
public enum ChargeLevel {

    FULL(4, "green"),
    HIGH(3, "green"),
    MEDIUM(2, "yellow"),
    LOW(1, "red");

    int sections;
    String color;

    ChargeLevel(int sections, String color) {
        this.sections = sections;
        this.color = color;
    }

    public int getSections() {
        return sections;
    }

    public String getColor() {
        return color;
    }

    public static ChargeLevel findByNumberOfSections(int sections) {
        for (ChargeLevel value: values()) {
            if (value.sections == sections) {
                return value;
            }
        }
        return null;
    }
}
~~~

Dentro del metodo **findByNumberOfSections()**, iteramos sobre los valores posibles usando un ciclo **for-each**. Aqui hay un ejemplo de la salida de nuestro metodo:

~~~java
System.out.println(ChargeLevel.findByNumberOfSections(2)); // MEDIUM
~~~

---

## Ejercicios

1. Tiene un enum **SI** con tres constantes. Cada constante almacena un string de cantidad, implementa el metodo de instancia **getQuantityName()** que retorne el nombre de la cantidad.

    ~~~java
    enum SI {

        M("lenght"),
        KG("mass"),
        S("time");

        public final String quantityName;

        SI(String quantityName) {
            this.quantityName = quantityName;
        }

        String getQuantityName() {
            return quantityName;
        }
    }
    ~~~

2. Tiene un enum **DangerLevel**, agregue un campo entero para almacnar niveles de peligro y haga coincidir el numero con cada constante, tambien debe agregar el metodo de instancia **getLevel** que devuelve el numero entero asociado.

    ~~~java
    enum DangerLevel {

        HIGH(3),
        MEDIUM(2),
        LOW(1);

        private final int level;

        DangerLevel(int level) {
            this.level = level;
        }

        int getLevel() {
            return level;
        }
    }
    ~~~

3. Tiene un enum **Operation** y la clase **Account**. Su tarea es implementar el metodo **changeBalance** para controlar saldos, debe tener logica que controle los depositos teniendo en cuenta el saldo de la cuenta.

    ~~~java
    public class Main {

        public static boolean changeBalance(Account account, Operation operation, Long sum) {

            boolean changed = false;
            if (operation == Operation.DEPOSIT) {
                account.balance += sum;
                changed = true;
            } else if (operation == Operation.WITHDRAW) {
                if (sum > account.balance) {
                    System.out.println("Not enough money to withdraw.");
                } else {
                    account.balance -= sum;
                    changed = true;
                }
            }
            return changed;
        }

        enum Operation {

            DEPOSIT,
            WITHDRAW
        }

        static class Account {

            private String code;
            private Long balance;

            public String getCode() {
                return code;
            }

            public Long getBalance() {
                return balance;
            }

            public void setBalance(Long balance) {
                this.balance = balance;
            }
        }

        public static void main(String[] args) {

            Scanner scanner = new Scanner(System.in);
            String[] parts = scanner.nextLine().split("\\s+");

            Account account = new Account();
            account.setBalance(Long.parseLong(parts[0]));

            Operation operation = Operation.valueOf(parts[1]);

            Long sum = Long.parseLong(parts[2]);

            if (changeBalance(account, operation, sum)) {
                System.out.println(account.getBalance);
            }
        }
    }
    ~~~

4. Cuales planetas se imprimen con el codigo.

    ~~~java
    public enum Planet {

        VENUS   (4.869e+24, 6.0518e+6),
        EARTH   (5.976e+24, 6.37814e+6),
        MARS    (6.421e+23, 3.3972e+6),
        JUPITER (1.9e+27,   7.1492e+7),
        SATURN  (5.688e+26, 6.0268e+7),
        URANUS  (8.686e+25, 2.5559e+7);

        private final double m; // mass in kilograms
        private final double r; // radius in meters

        Planet(double mass, double radius) {
            this.m = mass;
            this.r = radius;
        }

        public double mass() { return m; }

        public double radius() { return r; }
    }

    for (Planet planet : Planet.values()) {
        if (planet.mass() > 5.0e+24 && planet.radius() > 6.0e+7) {
            System.out.println(planet);
        }
    }

    // Imprime: JUPITER y SATURN
    ~~~

5. Cuales declaraciones sobre este codigo son correctas.

    ~~~java
    enum Direction {

        EAST("E"),
        WEST("W"),
        NORTH("N"),
        SOUTH("S");

        private final String shortCode;

        Direction(String code) {
            this.shortCode = code;
        }

        public String getShortCode() {
            return this.shortCode;
        }
    }

    /*
    Direction.NORTH.name() retorna "NORTH" como String
    Direction.NORTH.getShortCode() retorna "N" como String
    Direction.valueOf("NORTH") retorna Direction.NORTH como objeto Direction
    */
    ~~~

6. Tiene una clase **Robot**, que tiene metodos para y una enumeracion **Direction** implemente los metodos para mover el robot.

    ~~~java
    class Move {
        
        public static void moveRobot(Robot robot, int toX, int toY) {
            robot.stepForward();
        }
    }

    enum Direction {

        UP(0, 1),
        DOWN(0, -1),
        LEFT(-1, 0),
        RIGHT(1, 0);

        private final int dx;
        private final int dy;

        Direction(int dx, int dy) {
            this.dx = dx;
            this.dy = dy;
        }

        public Direction turnLeft() {
            switch (this) {
                case UP:
                    return LEFT;
                case DOWN:
                    return RIGHT;
                case LEFT:
                    return DOWN;
                case RIGHT:
                    return UP;
                    default:
                        throw new IllegalStateException();
            }
        }

        public Direction turnRight() {
            switch (this) {
                case UP:
                    return RIGHT;
                case DOWN:
                    return LEFT;
                case LEFT:
                    return UP;
                case RIGHT:
                    return DOWN;
                default:
                    throw new IllegalStateException();
            }
        }

        public int dx() { return dx; }

        public int dy() { return dy; }
    }

    class Robot {

        private int x;
        private int y;
        private Direction direction;

        public Robot(int x, int y, Direction direction) {
            this.x = x;
            this.y = y;
            this.direction = direction;
        }

        public void turnLeft() {
            direction = direction.turnLeft();
        }

        public void turnRight() {
            direction = direction.turnRight();
        }

        public void stepForward() {
            x += direction.dx();
            y += direction.dy();
        }

        public Direction getDirection() {
            return direction;
        }

        public int getX() {
            return x;
        }

        public int getY() {
            return y;
        }
    }
    ~~~
