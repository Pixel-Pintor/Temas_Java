# Custom Exceptions
Las excepciones en Java cubren muchas excepciones estandar con las que tenemos que lidiar en la programacion. Sin embargo, a veces es posible que necesitemos crearlas nosotros mismos. Una de las principales razones es para manejar excepciones logicas que son especificas de su programa. Imagine que esta implementando un aplicacion cliente-servidor. El servidor procesa la informacion del usuario y valida la exactitud, o lanza una excepcion. Una condicion indispensables para que el servidor funcione correctamente es que valide todos los campos con los datos del usuario y no se detenga en el primero incorrecto. Si algunos campos resultan ser incorrectos, queremos recibir un informe detallado sobre lo que salio mal. En este caso, una excepcion estandar como `IllegalArgumentException`, no sera suficiente, lo que significa que es hora de crear una excepcion personalizada.  

## Como crear y lanzar una excepcion personalizada
Para crear una excepcion personalizada, debe extender las clases `Exception` o `RuntimeException`. Aqui hay un ejemplo:
~~~java
public class MyAppException extends Exception {

    public MyAppException(String msg) {
        super(msg);
    }

    public MyAppException(Exception cause) {
        super(cause);
    }
}
~~~
En el ejemplo anterior, se declara una nueva clase de excepciones. Es una excepcion comprobada porque extiende la clase `Exception`. La clase declarada tiene dos constructores para crear instancias y llaman al constructor correspondiente de la clase base.  
Ahora, podemos lanzar una instancia de la clase:
~~~java
public static void someMethod() throws MyAppException {
    throw new MyAppException("Something bad");
}
~~~
Ahora aprendamos algunas reglas para crear excepciones personalizadas.

## Mejores practicas para excepciones personalizadas
Lo primero es lo primero, asegurese de que su aplicacion se beneficiara de la creacion de una excepcion personalizada. De lo contrario, utilice las excepciones estandar de Java.  
En segundo lugar, siga la convencion de nomenclatura: finalice el nombre de la clase con "Exception", por ejemplo `MyAppException`.  
Por ejemplo, veamos el fragmento de codigo acontinuacion. Aqui capturamos la causa raiz de la excepcion con el argumento `Throwable`, que se pasa al constructor de la clase principal.
~~~java
public class CustomException extends Exception {
    public CustomException(String message, Throwable cause) {
        super(message, cause);
    }
}
~~~
¿Crear una excepcion personalizada siempre es una buena idea? Aunque la funcion de excepcion personalizada mejora en gran medida el mecanismo de manejo de errores, su uso no siempre esta justificado. Le recomendamos que utilice excepciones estandar siempre que sea posible por varias razones, como:
- Las excepciones estandar son ampliamente conocidas por otros programadores. Uno puede entender el tipo de problema simplemente mirando el nombre de la excepcion.
- Al optar por las excepciones estandar, sigue el principio de reutilizacion. Hace que su codigo sea mas clase y mas profesional.  

## Conclusion
Las excepciones personalizadas son una gran herramienta para manejar las inconsistencias en su programa. En este tema, aprendimos como crearlos y lanzarlos, junto con algunas practicas recomendadas a seguir. Sin embargo, recomendamos crear una excepcion personalizada solo cuando este justificada. Las excepciones estandar de Java son a menudo una opcion mas segura y no menos eficiente.
---

## Ejercicios
1. Cual clase debe extender tu excepcion personalizada para arrojar una excepcion verificada?
- `java.lang.Exception`
2. Que es verdad acerca de las excepciones personalizadas.
- Es recomendable poner la palabra "Exception" al final del nombre
- Debe declarar un constructor que tome el argumento de excepcion si hay casos de creacion de excepciones personalizadas ademas de las estandar
- Extienda `RuntimeException` para crear una excepcion personalizada sin marcar
3. Tienes una clase llamada `Square` con un solo constructor, que toma un solo argumento. Si el argumento es positivo, el constructor los asigna al campo, sino lanza una `SquareSizeException`.
~~~java
class Square {
    int a;

    public Square(int a) throws SquareSizeException {
        if (a > 0) {
            this.a = a;
        } else {
            throw new SquareSizeException("zero or negative size");
        }
    }
}

class Main {
    public static void main(String[] args) {
        Scanner scn = new Scanner(System.in);
        int a = scn.nextInt();
        //put your code here
        try {
            new Square(a);
        } catch (SquareSizeException e) {
            System.out.println(e.getMessage());
        }
    }
}

class SquareSizeException extends Exception {
    public SquareSizeException(String message) {
        super(message);
    }
}
~~~
4. Escribe un programa que crear una excepcion verificada llamada `CustomException` y tenga un constructor con un argumento `Exception`.
~~~java
class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        Exception e = new Exception(sc.nextLine());
        Exception customException = new CustomException(e);

        System.out.println(customException.getMessage());
    }
}

class CustomException extends Exception {

    public CustomException(Exception e) {
        super(e);
    }
}
~~~
5. Necesitas escribir un metodo `drive()` que imprima en consola el mensaje `Here we go` si ambos argumentos son verdaderos, de otra manera que arroje una excepcion `DriverNotReady` con el mensaje `Close the door or fasten your seatbelt`.
~~~java
class Driver {
    String name;
    Boolean isDoorLocked;
    Boolean isBeltFastened;

    public Driver(String name, Boolean isDoorLocked, Boolean isBeltFastened) {
        this.name = name;
        this.isDoorLocked = isDoorLocked;
        this.isBeltFastened = isBeltFastened;

    }

    public void closeDoor() {
        this.isDoorLocked = true;
    }

    public void fastenBelt() {
        this.isBeltFastened = true;
    }

    public void drive(boolean isBeltFastened, boolean isDoorLocked) throws DriverNotReadyException {
        if (isBeltFastened && isDoorLocked) {
            System.out.println("Here we go");
        } else {
            throw new DriverNotReadyException("Close the door or fasten your seatbelt");
        }
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String name = scanner.next();
        Boolean isDoorLocked = scanner.nextBoolean();
        Boolean isBeltFastened = scanner.nextBoolean();
        Driver driver = new Driver(name, isDoorLocked, isBeltFastened);
        try {
            driver.drive(isBeltFastened, isDoorLocked);
        } catch (DriverNotReadyException e) {
            System.out.println(e.getMessage());
        }

    }
}

class DriverNotReadyException extends Exception {
    public DriverNotReadyException(String message) {
        super(message);
    }
}
~~~