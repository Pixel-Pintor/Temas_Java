# Abstract Class VS Interface

La *clase abstracta* e *interfaz* son herramientas para lograr la abstraccion que nos permite declarar los metodos abstractos. No podemos crear instancias de clases e interfaces abstractas directamente, solo podemos hacerlo a traves de clases que las heredan.  
Desde Java 8, una interfaz puede tener metodos predeterminados y estaticos que contienen una implementacion. Hace que la interfaz sea mas similar a una clase abstracta. Entonces, la pregunta importante es: ¿cual es la diferencia entre interfaces y clases abstractas?.  
A continuacion puede ver una lista de algunas diferencias importantes entre estos dos conceptos.

- Una *clase abstracta* puede tener abstractos y no abstractos, mientras que una *interfaz* puede tener abstractos o predeterminados
- Una *clase abstracta* puede extender otra clase abstracta o regular y una *interfaz* solo puede extender otra interfaz
- Una *clase abstracta* puede extender solo una clase mientras que una *interfaz* puede extender cualquier numero de interfaces
- Una *clase abstracta* puede tener finales, no finales, estaticas y no estaticas variables (campos regulares), mientras que una interfaz solo puede tener variables finales estaticas
- Una *clase abstracta* puede proporcionar una implementacion de una interfaz, pero una *interfaz* no puede proporcionar una implementacion de una clase abstracta
- Una *clase abstracta* puede tener un constructor y una *interfaz* no
- En una *clase abstracta*, la palabra clave **abstract** es obligatorio declarar un metodo como abstracto mientras que en una *interfaz* esta palabra clave es opcional

Recuerde, una clase *extiende* otra clase, una clase *implementa* una interfaz, pero una interfaz *extiende* otra interfaz.  
La lista de diferencias provista no es de ninguna manera completa. Las clases e interfaces tienen muchas otras diferencias, pero la principal es su proposito.  
Por lo general, las interfaces se usan para desacoplar la interfaz de un componente (clase) de la implementacion, mientras que las clases abstractas se usan a menudo como clases bases con campos comunes para ser ampliados por subclases.

## Usando clases abstractas e interfaces juntas

A veces, las interfaces y las clases abstractas se usan juntas para hacer que una jerarquia de clases sea mas flexible. En este caso, una clase abstracta contiene miembros comunes e implementa una o varias interfaces, y las clases concretas amplian la clase abstracta y posiblemente implementen otras interfaces. Vea el siguiente ejemplo sencillo.

~~~java
interface ManagedDevice {

    void on();
    void off();
}

abstract class AbstractDevice implements ManagedDevice {

    protected String serialNumber;
    protected boolean on;

    public AbstractDevice(String serialNumber) {
        this.serialNumber = serialNumber;
    }

    protected void setOn(boolean on) {
        this.on = on;
    }
}

class Kettle extends AbstractDevice {

    protected double volume;

    public Kettle(String serialNumber, double number) {
        super(serialNumber);
        this.volume = volume;
    }

    @Override
    public void on() {
        // logica para activar los componentes electronicos
        setOn(true);
    }

    @Override
    public void off() {
        // logica para detener los componentes electronicos
        setOn(false);
    }
}
~~~

El uso de ambos conceptos (interfaces y clases abstractas) hace que su codigo sea mas flexible. Utilice abstracciones adecuadas o su combinacion al diseñar sus jerarquias de clases.  
Como ejemplo, puede ver jerarquias de clases en la biblioteca de clases estandar de Java. Un ejemplo de eso es la jerarquia de las colecciones. Combina clases e interfaces abstractas para hacer que la jerarquia sea mas facil de mantener y flexible para usar en su codigo.

---

## Ejercicios

1. Cuales son las diferencias entre *clases abstractas* e *interfaces*?

    - Una clase abstracta puede tener metodos de instancia abstractos y no abstractos, mientras que una interfaz solo puede tener metodos de instancia abstractos y predeterminados
    - En una clase abstracta, la palabra clave **abstract** es obligatorioa para declarar un metodo como resumen, mientras que en una interfaz esta palabra clave es opcional

2. Que es cierto acerca de las *clases* e *interfaces abstractas*?

    - Una clase puede extender solo una unica clase abstracta pero puede implementar cualquier cantidad de interfaces

3. Cuales son las diferencias y similitudes entre las clases *abstractas* e *interfaces*?

    - Una clase abstracta implementa una interfaz mientras que una interfaz extiende otra interfaz
    - Una clase abstracta y una interfaz no se puede instanciar directamente

4. Que correcciones se deben hacer para que el codigo compile?

    ~~~java
    interface Resizable {

        void resize(float scale);
        Resizable() { }
    }

    abstract class GraphicObject {
        int xPos, yPos;

        abstract void draw();
    }

    class Rectangle extends GraphicObject {

        // campos y metodos

        @Override
        public void resize(float scale) { ... }
    }

    class Circle extends GraphicObject {

        // campos y metodos

        @Override
        public void resize(float scale) { ... }
    }
    ~~~

    - Anular el metodo **draw()** en **Rectangle** y **Circle**
    - Borrar **Resizable()** de la interfaz **Resizable**
    - Agregar **implements Resizable** a la declaracion de **GraphicObject** o sus dos subclases

5. Corrige el codigo para que compile.

    ~~~java
    interface Message {

        protected String sender;
        protected String dest;

        String getSender();
        String getDestination();
        String getText();
    }

    abstract class BaseMessage extends Message {

        public BaseMessage(String sn, String dst) {
            sender = sn;
            dest = dst;
        }
    }

    class CipherMessage extends BaseMessage {

        protected String text;

        public CipherMessage(String sender, String destination, String text) {
            super(sender, destination);
            this.text = CryptoUtils.encrypt(text);
        }

        @Override
        public String getText() {
            return text;
        }
    }
    ~~~

    - Cambie la palabra **extends** a la palabra **implements** en la declaracion de **BaseMessage**
    - Mueve los campos **sender** y **dest** al cuerpo de **BaseMessage**
    - Anula el metodo **getDestination()** en **BaseMessage** o **CipherMessage**
