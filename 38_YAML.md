# YAML
Imagina objetos con estructuras complejas. Por ejemplo, tiene un diccionario grande o una lista con muchos otros valores. Necesita preservar dicho objeto sin perder su estructura. En otras palabras, desea serializarlo. Tambien es bueno si el formato de serializacion tiene una sintaxis simple y es legible por humanos. Entonces, familiaricemonos con una famoso formato de datos llamado YAML.
## Tipos de datos basicos
YAML admite todos los tipos de datos esenciales, como numeros, cadenas, booleanos, etc. Reconoce algunos tipos de datos especificos del idioma, como fechas, marchas de tiempo y valores numericos especiales. Entonces, la lista de tipos de datos incluye:
- enteros como 15, 123
- cadenas como "15, "Hola"
- flotantes como 15.033
- booleanos (true o false)
- null type  

YAML detecta automaticamente el tipo de datos, pero los usuarios pueden especificar el tipo que necesitan usando `!!`. Por ejemplo, si necesita especificar la cadena `yes`, debe escribir `!!str yes`.
## Maps
El mapeo consta de pares clave-valor. Por ejemplo:
~~~yaml
---
object: Book

metadata:
    name: Three Men in a Boat
    author: Jerome K Jerome
    genre: humorous account

published:
    year: 1889
    country: United Kingdom
~~~
La primera linea es un separador. Es opcional a menos que intente definir varias estructuras en un solo archivo. Entonces hay un conjunto de `key: value` pares como un bloque. Los pares se llaman **escalares**. La sintaxis es limpia y simple; los simbolos de formato habituales, como llaves, corchetes, etiquetas de cierre o comillas, son innecesarios. Los escalares estan separados por dos puntos y deben haber un espacio entre los elementos del mapa. Tenga en cuenta que en YAML, la sangria siempre se realiza con espacios, no con tabulaciones.
## Listas
Las listas en YAML son secuencias de objetos, como muestras el siguiente ejemplo:
~~~yaml
animals:
  - cat
  - dog
  - bird
~~~
El numero de elementos de la lista no esta limitado. Cada elementos de la lista debe comenzar con un guion. Los elementos se separan del padre con espacios; despues del nombre de un padre debe haber dos puntos. El ejemplo anterior representa un estilo de bloque. En el estilo de flujo, la lista se ve asi: `[cat, dog, bird]`.
## Combinacion
Los mapas y las listas se pueden combinar, de modo que uno puede tener mapas de mapas, o mapas de listas, o listas de listas, o listas de mapas. Consideremos un ejemplo de un mapa de tareas pendientes, donde las clave son fines de semana y los valores son listas de cosas para hacer durante cada dia:
~~~yaml
weekend:
  saturday:
    - order cleaning
    - order a pizza
    - watch new series
  sunday:
    - go to yoga
    - hang out with a friend
~~~
Ademas, si necesita indicar una cadena que conserva lineas nuevas en lugar de una lista, use `|`:
~~~yaml
saturday: |
  order cleaning
  order a pizza
  watch new series
~~~
## Comentarios
Uno puede agregar comentarios al archivo YAML. Los comentarios comienzan con `#` y van hasta la nueva linea. Se pueden hacer en cualquier parte de la linea, por ejemplo:
~~~yaml
# The comment
metadata: # this is metadata
  name: Three Men in a Boat
  author: Jerome K Jerome
  genre: humorous account
~~~
