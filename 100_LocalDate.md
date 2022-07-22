# LocalDate

El tiempo es una nocion fundamental no solo en la vida humana sino tambien en la programacion. Por supuesto, Java proporciona algunas herramientas para trabajar con entidades temporales. Comenzaremos con unidades de medida de tiempo como dias (o fechas).  
La clase **LocalDate** representa una sola fecha en un formato *AAAA-MM-dd*, como **2017-11-25** o **2025-01-23**. Podria usarse para almacenar cualquier fecha: desde tu cumpleaños hasta el dia del apocalipsis.

## Creando LocalDate y hora actual

Despues de importar la clase, se puede crear una instancia que almacene la fecha actual de la siguiente manera.

~~~java
LocalDate now = LocalDate.now();
~~~

Por supuesto, tambien es posible crear una instancia de **LocalDate** que representa un dia especifico de un año. Se puede obtener usando cualquiera de los dos metodos estaticos especiales: **of** y **parse**. Aqui hay dos ejemplos.

~~~java
LocalDate date1 = LocalDate.of(2017, 11, 25); // 2017-11-25 (25 November 2017)
LocalDate date2 = LocalDate.parse("2017-11-25"); // 2017-11-25 (25 November 2017)
~~~

El sistema de numeracion es intuitivo: el numero de un mes es un numero del 1 al 12 inclusive, el primer dia de un mes tiene el numero 1.  
La otra forma util de crear una instancia de **LocalDate** es indicando un año y el numero correlativo de un dia en ese año asi:

~~~java
LocalDate.ofYearDay(2017, 33); // 2017-02-02 (2 February 2017)
~~~

Un numeor de un dia en el año es un **int** del 1 al 356-366.

~~~java
LocalDate.ofYearDay(2016, 365); // 2016-12-30 (30 December 2016)
LocalDate.ofYearDay(2017, 365); // 2017-12-31 (31 December 2017)
~~~

Sin embargo, tenga cuidado, porque puede ocurrir una excepcion cuando tratamos con el dia 366:

~~~java
LocalDate.ofYearDay(2016, 366); // 2016-12-31 (31 December 2016)
LocalDate.ofYearDay(2017, 366); // here is an exception occurs, because the year is not leap
~~~

## LocalDate: año, mes, dia y duracion

Consideremos ahora el siguiente ejemplo de **LocalDate**:

~~~java
LocalDate date = LocalDate.of(2017, 11, 25); // 2017-11-25 (25 November 2017)
~~~

Podemos obtener el año, el mes, el dia de un mes, el dia de un año:

~~~java
int year = date.getYear(); // 2017
int month = date.getMonthValue(); // 11
int dayOfMonth = date.getDayOfMonth(); // 25
int dayOfYear = date.getDayOfYear();  // 329
~~~

Tambien es posible obtener un longitud del año y del mes:

~~~java
int lenOfYear = date.lengthOfYear(); // 365
int lenOfMonth = date.lengthOfMonth(); // 30
~~~

## Metodos aritmeticos de LocalDate

La clase tiene otraos metodos para sumar, restar y alterar un dia, mes y año. Vamos a crear otra instancia de **LocalDate**:

~~~java
LocalDate date = LocalDate.of(2017, 1, 1); // 2017-01-01 (1 January 2017)
~~~

Y eche un vistazo a como podemos aplicar estos metodos:

~~~java
LocalDate tomorrow = date.plusDays(1);    // 2017-01-02 (2 January 2017)
LocalDate yesterday = date.minusDays(1);  // 2016-12-31 (31 December 2016)
LocalDate inTwoYears = date.plusYears(2); // 2019-01-01 (1 January 2019)
LocalDate in2016 = date.withYear(2016);   // 2016-01-01 (1 January 2016)
~~~

---

## Ejercicios

1. Escribe un programa que imprima todas las fechas del año dado con una compensacion especifica aplicada. Debe leer una fecha de inicio y un valor de compensacion (en dias) sin que pase al siguiente año.

    ~~~java
    class Main {
        public static void main(String[] args) {
            Scanner scanner = new Scanner(System.in);
            LocalDate date = LocalDate.parse(scanner.nextLine());
            int days = scanner.nextInt();
            int thisYear = date.getYear();

            do {
                System.out.println(date);
                date = date.plusDays(days);
            } while (thisYear == date.getYear());
        }
    }
    ~~~

2. Escriba un program que imprima el dia n desde el final de un mes. El programa debe leer el año, el mes y el numero restante de dias hasta el final del mes.

    ~~~java
    class Main {
        public static void main(String[] args) {
            Scanner scanner = new Scanner(System.in);
            int year = scanner.nextInt();
            int month = scanner.nextInt();
            int remaining = scanner.nextInt();

            LocalDate date = LocalDate.of(year, month, 1);
            date = date.plusDays(date.lengthOfMonth()).minusDays(remaining);
            System.out.println(date);
        }
    }
    ~~~

3. Seleccione las declaraciones correctas sobre los metodos de **LocalDate**.

    - La clase **LocalDate** tiene metodos para recuperar la duracion del año y el mes.
    - La invocacion **LocalDate.parse("2017-09-21")** crea una instancia que representa el *21 de septiembre de 2017*.

4. Seleccione las formas correctas de crear una instancia de **LocalDate** que represente el *2 de enero de 2018*.

    ~~~java
    LocalDate.of(2018, 3, 2).minusMonths(2);
    LocalDate.ofYearDay(2018, 2);
    LocalDate.of(2018, 1, 1).plusDays(1);
    ~~~

5. Escribe un programa que lea una fecha e imprima esa fecha diez dias antes.

    ~~~java
    class Main {
        public static void main(String[] args) {
            Scanner scanner = new Scanner(System.in);
            LocalDate date = LocalDate.parse(scanner.nextLine());
            System.out.println(date.minusDays(10));
        }
    }
    ~~~

6. 