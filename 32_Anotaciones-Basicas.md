# Conceptos basicos de las anotaciones
Una **anotacion** es un instrumento especial de Java que proporciona informacion sobre un programa. Su objetivo principal es facilitar la vida de los programadores. Son una forma de metadatos, lo que significa que no son parte del programa en si. Puede marcar clases, metodos, campos, variables y otras partes de un programa con anotaciones. Cuando lo hace, las anotaciones brindan informacion para el compilador, para algunas herramientas de desarrollo o para marcos y bibliotecas en tiempo de ejecucion.  
Todas las anotaciones comienzan con el simbolo `@` seguido del nombre de la anotacion, y suelen estar marcados con un color diferente en el codigo.
~~~java
@Override
public void printName() {
    System.out.println(this.name);
}
~~~
Java tiene varias anotaciones integradas. Si desea utilizar otras anotaciones, debera incluir bibliotecas o marcos, o incluso crear sus propias anotaciones.
- `@Deprecated` es una anotacion simple que significa que el metodo marcado esta en desuso, es decir obsoleto.
- `@SuppressWarnings` ordena al compilador que deshabilite algunas advertencias en tiempo de compilacion. Usted especifica en los parametros que advertencias no desea ver.
- `@Override` marca un metodo que anula un metodo de superclase. Esta anotacion solo se puede aplicar a los metodos.  
Algunas anotaciones tienen **elementos**, donde un elemento es similar a un atributo o un parametro.
~~~java
@SuppressWarnings("unused")
public void printHello() {
    System.out.println("Hello!");
}
~~~
Un elemento de anotacion tambien puede ser una matriz. De hecho, el tipo real de `value` en `@SuppressWarnings` es `String[]`:
~~~java
@SuppressWarnings({"unused", "deprecation"})
public void printHello() { ... }
~~~
`"deprecation"`, indica al compilador que suprima las advertencias sobre el uso de codigo obsoleto. Algunas anotaciones tienen un valor predeterminado para un elemento y otras no.
~~~java
@SuppressWarnings // wrong syntax, there is no default value!
public void printHello() {
    System.out.println("Hello!");
}
~~~
Desde Java 9 `@Deprecated` tiene dos elementos: `since` y `forRemoval`. 
- `since` requiere la version (String) en la que el elemento anotado ha quedado obsoleto. El valor predeterminado es una cadena vacia.
- `forRemoval` indica si el elemento anotado se eliminara en una version futura. El valor predeterminado es `false`.
~~~java
@Deprecated(since = "5.3", forRemoval = true)
public void printHello() {
    System.out.println("Hello!");
}
~~~
El ejemplo anterior significa que `printHello` ha quedado obsoleto desde la version 5.3 de nuestra biblioteca y se eliminara en la proxima version.
- La anotacion `@NotNull` indica que una variable no puede ser `null`, un metodo no puede retornar `null`.
- La anotacion `@Range` indica que una variable siempre pertenece al rango especificado, un metodo devuelve un numero entero que pertenece al rango especificado.  
Ahora veamos nuestra clase llamada `GameCharacter`.
~~~java
class GameCharacter {

    @NotNull
    private String login;

    @Range(min = 1, max = 100)
    private int level = 1;

    public GameCharacter(
        @NotNull String login,
        @Range(min = 1, max = 100) int level) {
        this.login = login;
        this.level = level;
    }

    @NotNull
    public String getLogin() {
        return login;
    }

    @Range(min = 1, max = 100)
    public int getLevel() {
        return level;
    }
}
~~~
Aqui estas anotaciones te ayudaran mostrandote advertencias si `login` contiene `null`, si el `level` de tu personaje es menor que 1 o mayor que 100.
---
## Ejercicios
1. Cuales son las reglas para marcar un elemento con una anotacion Java.
- Algunas anotaciones pueden marcar clases, metodos, campos y variables
- Es posible mezclar anotaciones integradas y externas en un `.java`.
2. Seleccione todas las formas correctas de marcar un metodo usando `@SuppressWarnings`
~~~java
@SuppressWarnings(value = "deprecation")
@SuppressWarnings({"deprecation", "unused"})
~~~
3. Como seria la forma correcta de implementar la anotacion `@Entity`.
~~~java
@Entity
@Entity(name = "MyEntity")
~~~
