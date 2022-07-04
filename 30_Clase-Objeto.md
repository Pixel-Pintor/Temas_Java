# La clase Objeto
Java tiene una clase llamada `Object` que es el padre de todas las clases estandar y sus clases personalizadas. Cada clase extiende esta implicitamente, por lo tanto es una raiz de herencia en los programas Java. Vamos a crear una instancia de la clase `Object`.
~~~java
Object anObject = new Object();
~~~
Los objetos de la clase `Object` pueden referirse a una instancia de cualquier clase porque cualquier instancia es un tipo `Object`.
~~~java
Long number = 1_000_000L;
Object obj1 = number; // una instancia de Long puede ser convertida a Object

String str = "str";
Object obj2 = str; // lo mismo con una instancia de String
~~~
Cuando declaramos una clase, podemos extender explicitamente la clase `Object`. Sin embargo, no tiene sentido, ya qye la extension ya esta implicita.  
La clase `Object` proporciona algunos metodos comunos a todas las subclases. Tiene nueve metodos de instancia (excluyendo metodos sobrecargados) que se pueden dividir en cuatro grupos:
- **sincronizacion de hilos**: `wait`, `notify`, `notifyAll`;
- **identidad de objeto**: `hashCode`, `equals`;
- **gestion de objetos**: `finalize`, `clone`, `getClass`;
- **representacion legible por humanos**: `toString`.  
Esta forma de organizacion no es perfecta, pero puede ayudarlo a recordarlos. Aqui hay una explicacion mas detallada de los metodos:
- El primer grupo **sincronizacion** son para trabajar en aplicaciones multiproceso.
- `hashCode` devuelve un valor de codigo hash para el objeto.
- `equals` indica si algun otros objeto es **"igual a"** este en particular.
- `finalize` es llamado por el recolector de basura (GC) en un objeto cuando el GC quiere limpiarlo.
- `clone` crea y devuelve una copia del objeto.
- `getClass` devuelve una instancia de `Class`, que tiene informacion sobre la clase de tiempo de ejecucion.
- `toString` devuelve una representacion de cadena del objeto.  
