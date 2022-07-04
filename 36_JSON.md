# JSON
**JSON** es un formato basado en texto para almacenar y transmitir datos estructurados. Proviene del lenguaje JavaScript, pero todavia se considera independiente del lenguaje: funciona con casi cualquier lenguaje de programacion. Con la sintaxis liviana de JSON, puede almacenar y enviar facilmente a otras aplicaciones todos, desde numeros y cadenas hasta matrices y objetos. Tambien puede crear estructuras de datos mas complejas vinculando matrices entre si.  
El texto **JSON** se puede construir en una de dos estructuras:
- Una coleccion de **clave:valor** (matriz asociativa).
- Un conjunto ordenado de valores (matriz o lista).  

Los objetos JSON se escriben entre llaves `{}`, y sus pares clave:valor estan separados por una coma `,`. La clave y el valor en el par estan separados por dos puntos `:`.
~~~json
{
    "first_name": "Sophie",
    "last_name": "Goodwin",
    "age": 34
}
~~~
Las claves de un objeto siempre son cadenas, pero los valores pueden ser cualquiera de los siete tipos de valores, incluido otro objeto o matriz.  
Las matrices se escriben entre corchetes. `[]` y sus valores estan separados por una coma `,`. El valor de la matriz, de nuevo puede ser de cualquier tipo, incluida otra matriz u objeto. Aqui hay un ejemplo de una matriz.
~~~json
["night", "street", false, [ 345, 23, 8, "juice"], "fruit"]
~~~
## Objetos anidados
JSON es un formato muy flexible. Puede anidar objetos dentro de otros objetos como propiedades:
~~~json
{
    "persons": [
        {
            "firstName": "Whitney",
            "lastName": "Byrd",
            "age": 20
        },
        {
            "firstName": "Eugene",
            "lastName": "Lang",
            "age": 26
        },
        {
            "firstName": "Sophie",
            "lastName": "Lang",
            "age": 34
        }
    ]
}
~~~
Si los objetos y las matrices contienen otros objetos o matrices, los datos tienen una estructura similar a un arbol. Los objetos anidados son completamente independientes y pueden tener diferentes propiedades.
~~~json
{
    "persons": [
        {
            "firstName": "Whitney",
            "age": 20
        },
        {
            "firstName": "Eugene",
            "lastName": "Lang"
        }
    ]
}
~~~
Pero en la practica, tales objetos a menudo se ven similares.  
La eleccion de convencion de nomenclatura JSON correcta depende directamente de su lenguaje de programacion y bibliotecas. Puede usar **camelCase** y **snake_case**, cualquier opcion es valida, pero no las mezcle en un JSON.  
## Las ventajas de JSON
JSON esta muy difundido par el intercambio de datos en internet debido a sus grandes ventajas.
- compacidad.
- flexibilidad.
- alta legibilidad.
- la mayoria de los lenguajes de programacion tienen funciones y bibliotecas para leer y crear estructuras JSON.  

La principal ventaja de JSON en comparacion con el texto sin formato es la capacidad de describir relaciones entre objetos a traves de pares clave-valor y anidamiento. Por lo tanto, es muy probable que los sitios que visita con frecuencia tambien usen JSON.
---
## Ejercicios
1. Estudie cuidadosamente el objeto JSON y especifique que linea contiene un error.
~~~json
{  
  persons: [  # la clave debe ser una cadena "persons"
    {  
      "firstName": "Stanislaw",
      "lastName": "Lem",
      "book": [  
        {  
          "title": "Summa Technologiae",
          "year": 1964
        }
      ]
    },
    {  
      "firstName": "Isaac",
      "lastName": "Asimov",
      "book": [  
        {  
          "title": "The End of Eternity",
          "year": 1955
        }
      ]
    }
  ]
}
~~~
2. Crea un objeto JSON que tenga un solo campo `country`. El campo guarda el nombre de un pais.
~~~json
{ "country": "Colombia" }
~~~
3. Crea un objeto JSON que represente una biblioteca. Una biblioteca tiene solo un campo `books`, pero cada libro tiene dos campos: `title` y `author`. Llene la biblioteca con al menos tres libros e ingrese el JSON resultante.
~~~json
{
    "books": [
        {
            "title": "Roots",
            "author": "Alex Haley"
        },
        {
            "title": "Lolita",
            "author": "Vladimir Nabokov"
        },
        {
            "title": "Postman",
            "author": "Charles Bukowski"
        }
    ]
}
~~~