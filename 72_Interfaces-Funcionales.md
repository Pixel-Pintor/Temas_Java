# Interfaces Funcionales

En este tema, aprenderá un nuevo concepto llamado **interfaces funcionales**. Este es un conocimiento vital porque este tipo de interfaces permite a los desarrolladores usar la programación funcional sobre los conceptos de OOP y, en cierto sentido, conectan ambos mundos. Son las interfaces funcionales las que hacen posible el uso de expresiones lambda, referencias a métodos y otras cosas funcionales. Suponemos que ya está familiarizado con las interfaces, las expresiones lambda y las clases anónimas. Ahora vamos a mostrarte cómo están todos conectados entre sí.

## Funciones e Interfaces

Si una interfaz contiene solo un **unico metodo abstracto** (dicha interfaz a veces se denomina **tipo SAM**), se puede considerar como un funcion. Ademas de las formas estandar de implementar interfaces mediante herencia o clases anonimas, estas interfaces se pueden implementar mediante una expresion lambda o una referencia de metodo.  
Aqui hay un clon del estandar. **Function<T, R>** interfaz que demuestra la idea basica.

~~~java
@FunctionalInterface
interface Func<T, R> {
    R apply(T val); // Single abstract method
}
~~~

La anotacion **@FunctionalInterface** se usa para marcar interfaces funcionales y generar un error de tiempo de compilacion si una interfaz no cumple con los requisitos de una interfaz funcional. El uso de esta anotacion no es obligatorio pero si recomendable.  
