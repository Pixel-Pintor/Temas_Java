# LocalTime

Hay una clase especial para representar la hora del dia en Java. La clase **LocalTime** representa el dia en el formato horas-minutos-segundos, como **06:30** o **11:45:30**. No almacena informacion sobre la fecha o la zona horaria. El tiempo se almacena sobre el fecha o la zona horaria. El tiempo se almacena con precision de nanosegundos (por ejemplo, **13:45:30.123456789**). La clase **LocalTime** podria usarse para almacenar cosas como el horario de apertura y cierre de una tienda o el horario de un tren.

## Crear LocalTime, hora actual y constantes

En primer lugar, veamos como creamos una instancia de la clase **LocalTime**. Se puede crear una instancia que almacene la hoa actual como se muestra a acontinuacion:

~~~java
LocalTime not = LocalTime.now();
~~~

Si queremos pasar un tiempo especifico a una instancia, debemos emplear cualquier de los dos metodos estaticos **of** y **parse** para crear una instancia de **LocalTime**:

~~~java
LocalTime.of(11, 45); // 11:45
LocalTime.of(11, 45, 30); // 11:45:30
LocalTime.parse("11:45:30"); // 11:45:30 (horas, minutos, segundos)
~~~

En la primera linea, los campos segundo y nanosegundo se estableceran en cero.  
La hora de un dia es un numero **int** del 0 al 23, mientras que los minutos y los segundos son numeros del 0 al 59. Los nanosegundos pueden ser cualquier numero entero del 0 999,999,999. El siguiente codigo lanza una excepcion.

~~~java
LocalTime.of(24, 1, 1); // DataTimeException (24 es an invalid value for hours)
~~~

Tambien es posible crear una instancia de **LocalTime** usando metodos estaticos **ofSecondOfDay** y **ofNanoOfDay**. En este caso, deberiamos indicar segundos y nanosegundos de un dia respectivamente.

~~~java
LocalTime time = LocalTime.ofSecondOfDay(12345); // 03:25:45
LocalTime nanoTime = LocalTime.ofNanoOfDay(1234567890); // 00:00:01.234567890
~~~

Tambien algunas constantes predefinidas en la clase **LocalTime**:

~~~java
LocalTime.MIN; // 00:00
LocalTime.MAX; // 23:59:59.999999999
LocalTime.NOON; // 12:00
LocalTime.MIDNIGHT; // 00:00
~~~

## LocalTime: horas, minutos y segundos

Ahora vamos a discutor un monton de metodos utiles de la clase **LocalTime**.  
Supongamos que tenemos una instancia de **LocalTime**:

~~~java
LocalTime time = LocalTime.of(11, 45, 30); // 11:45:30
~~~

Al usar los siguientes metodos, podemos obtener horas, minutos, segundos y nanosegundos.

~~~java
time.getHour(); // 11
time.getMinute(); // 45
time.getSecond(); // 30
time.getNano(); // 0, nanosegundos
~~~

Otro metodo util es **toSecondOfDay()**. Retorna el tiempo en segundos del dia desde el dado en la instancia **LocalTime**. Para nuestro ejemplo sera:

~~~java
time.toSecondOfDay(); // 42330
~~~

## Metodos aritmeticos de LocalTime

La clase tambien tiene metodos para sumar y restar horas, minutos, segundos y nanosegundos:

~~~java
LocalTime time1 = time.plusHours(5); // 16:45:30
LocalTime time2 = time.plusHours(22); // 09:45:30
LocalTime time3 = time.minusMinutes(10); // 11:35:30
LocalTime time4 = time.minusSeconds(30); // 11:45
~~~

Los siguientes metodos retornan una copia de una instancia con una parte alterada:

~~~java
LocalTime time1 = time.withHour(23); // 23:45:30
LocalTime time2 = time.withMinute(50); // 11:50:30
LocalTime time3 = time.withSecond(0); // 11:45
~~~

---

## Ejercicios

1. Implemente un metodo que tome dos instancias de **LocalTime** y determine cuantos segundos hay entre ellas.

    ~~~java
    public class Main {

        public static void main(String[] args) {
            Scanner scanner = new Scanner(System.in);
            LocalTime time1 = LocalTime.parse(scanner.nextLine());
            LocalTime time2 = LocalTime.parse(scanner.nextLine());
            int difference = Math.abs(time1.toSecondOfDay() - time2.toSecondOfDay());
            System.out.println(difference);
        }
    }
    ~~~

2. Seleccione las manera correctas de crear una instancia de **LocalTime** que represente *23:50*.

    ~~~java
    LocalTime.of(23, 50, 30).withSecond(0);
    LocalTime.of(23, 50, 0).plusHours(24);
    LocalTime.of(23, 50);
    ~~~

3. Cual sera el resultado del codigo.

    ~~~java
    nt time = LocalTime.of(0, 0, 2).plusSeconds(6078).getSecond();
        System.out.println(time); // 20