# Expresiones Switch
Las declaraciones `switch` se usan a menudo para evitar largas cadenas de `if-else`, haciendo que el codigo sea mas legible. Dicho esto, las declaraciones de cambio pueden ser detalladas a su manera, y los requisitos estrictos para colocar `break` a menudo los hace propensos a errores.
## Declaraciones Switch frente a expresiones Switch
La principal diferencia entre una expresion Switch y una declaracion Switch es que mientras que una declaracion se puede utilizar para actualizar el valor de una variable predefinida, una expresion se asigna a una variable. Esto es posible porque una expresion se evalua a un valor especifico. Ademas, las expresioens introdujeron la nueva **sintaxis de flecha** que condensa el codigo, lo hace mas legible y elimina la necesidad de declaraciones `break`. Veamos un ejemplo que muestra estas diferencias.  
Comenzamos con una enumeracion de varias cosas que van a ser probadas. Nuestra declaracion Switch y las expresiones Switch van a asignar una calificacion de sabor como un numero del 1 al 10. Primero, veremos como esto se escribira comunmente como una declaracion Switch:
~~~java
private enum ThingsToTaste {BROCCOLI, STEAK, SUGAR, DIRT, MEATBALLS, CHOCOLATE}

int tasteValue = 0;
ThingsToTaste taste = ThingsToTaste.DIRT;

switch (taste) {
    case SUGAR:
    case PIZZA:
    case CHOCOLATE:
        tasteValue = 10;
        break;
    case MEATBALLS:
    case STEAK:
        tasteValue = 7;
        break;
    case BROCCOLI:
        tasteValue = 4;
        break;
    case DIRT:
        tasteValue = 1;
        break;
    default:
        throw new IllegalStateException("Invalid tastable object: " + taste);
}
System.out.println(taste + ": " + tasteValue);
~~~
Ahora vamos a contrastar esto con una expresion Switch.
~~~java
int tasteValue = switch (taste) {
    case SUGAR, PIZZA, CHOCOLATE -> 10;
    case MEATBALLS, STEAK -> 7;
    case BROCCOLI -> 4;
    case DIRT -> 1;
    default -> throw new IllegalStateException("Invalid tastable object: " + taste);
}
~~~
Tenga en cuenta que no usa sentencias `break`, la nueva sintaxis de flecha lo reemplaza. La flecha indica que una vez alcanzado el valor se deba asignar a la variable `tasteValue` y luego se detiene. Todavia podemos tener un caso `default` que puede asignar una numero entero o arrojar una excepcion. De hecho, hay tres posibilidades de lo que puede venir despues de la flecha.
- Una valor del tipo con el que se declaro la expresion Switch.
- Lanzar una nueva excepcion.
- Un bloque de codigo que se evalua como un valor del tipo correcto.  

Dado que las expresiones Switch se evaluan en un valor especifico de un tipo especificado, debe tener en cuenta todos los casos posibles.
## Variaciones de las expresiones Switch
Java 13 introdujo la palabra `yield` que se puede usar dentro de dos puntos `case` para identificar el valor de `case` en la declaracion. Tambien reemplaza `break` y elimina la necesidad de mencionar explicitamente la variable al que se asigna el valor. Si va a utilizar dos puntos en su `case`, usar `yield` es la mejor opcion.
~~~java
int tasteValue = switch (taste) {
    case SUGAR:
    case PIZZA:
    case CHOCOLATE:
        yield 10;
    case MEABALLS:
    case STEAK:
        yield 7;
    case BROCCOLI:
        yield 4;
    case DIRT:
        yield 1;
    default:
        throw new IllegalStateException("Invalidad tastable object: " + taste);
}
~~~
La palabra `yield` no se puede usar dentro de una declaracion `switch`. Igualmente, `break` no se puede usar dentro de una expresion `switch`. Por lo tanto `yield` ayuda a garantizar que el lector de su codigo no olvide que tipo de `switch` esta leyendo.  
Tambien hay una opcion intermedia que usa la nueva sintaxis de flecha pero coloca un bloque de codigo con un `yield` en ella ala derecha de la flecha. Esto puede parecer innecesariamente detallado en comparacion con el primer ejemplo de sintaxis de mayusculas y minusculas que se meustra arriba, pero hay situaciones en las que esta forma larga tiene algunas ventajas. Los `yield` debe ser la ultima linea en el bloque de codigo, pero puede llamar a otras funciones en las lineas anteriores. Un ejempli simple de esto seria imprimir el valor que se va a entrgar a la consola justo antes de entregarla, como puede ver acontinuacion.
~~~java
tasteValue = switch (taste) {
    case SUGAR, PIZZA, CHOCOLATE -> {
        System.out.println(10);
        yield 10;
    }
    case MEATBALLS, STEAK -> {
        System.out.println(7);
        yield 7;
    }
    case BROCCOLI -> {
        System.out.println(4);
        yield 4;
    }
    case DIRT -> {
        System.out.println(1);
        yield 1;
    }
    default -> {
        throw new IllegalStateException("Invalid tasteable object: " + taste);
    }
}
~~~
## Ejercicios
1. Complete el codigo.
~~~java
int numberOfHamburgersToEat = switch (howHungryAmI) {
    case 1, 2, 3 -> 0;
    case 4, 5, 6 -> {
        yield 1;
    }
    case 7, 8, 9 -> {
        System.out.println(2);
        yield 2;
    }
    case 10 -> 3;
    default -> throw new Exception("The number must be between 1 and 10");
}
~~~
2. Reescribe la siguiente declaracion switch como si una expresion switch.
~~~java
private enum DaysOfTheWeek {MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY}