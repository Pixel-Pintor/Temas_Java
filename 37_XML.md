# XML
**XML (lenguaje de marcado extensible)** es un formato basado en texto para almacenar e intercambiar datos estructurados en internet. En este formato, los datos se presentan como documentos con una estructura clara y flexible. Un documento XML se puede almacenar en la computadora con la extension `.xml`.  
## Etiquetas y elementos
Cada documentos XML consta de **etiquetas** y **elementos**.  
Una **etiqueta** es una cadena con significado asignado, como un libro, una persona o algo por el estilo. XML no proporciona ninguna etiquetas, permite al desarrollador inventar etiquetas de forma independiente.  
Un **elemento** es un bloque de construccion de una estructura XML: puede contener texto, etiquetas, otros elementos y atributos. Este es un ejemplo de un documento XML que describe un libro con un titulo y un autor.
~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<book>
    <title>The Three-Body Problem</title>
    <author>Liu Cixin</author>
</book>
~~~
Este documento tiene tres etiquetas entres parentesis angulares: `<book>`, `<title>` y `<author>`.  
Por `element` entendemos la combinacion de una etiqueta de inicio con la correspondiente etiqueta de finalizacion junto con su contenido. Los elementos establecen la estructura de un documento, ya que se pueden anidar entre si.  
La primera linea en el documento XML se llama **prologo**:
~~~xml
<?xml version="1.0" encoding="UTF-8"?>
~~~
## Elementos secundarios
Cada documento XML siempre tiene un solo elemento llamado `root`. Este elemento puede contener otros elementos llamados elementos **secundarios** que a su vez pueden tener sus propios secundarios.
~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<library>
    <book>
        <title>The Three-Body Problem</title>
        <author>Liu Cixin</author>
    </book>
    <book>
        <title>Modern Operating Systems</title>
        <author>Andrew S. Tanenbaum</author>
    </book>
</library>
~~~
Aqui, el elemento raiz `<library>` tiene dos elemento hijos `<book>`, mientras que los elementos `<title>` y `<author>` son los hijos de `<book>`. Entonces, los documentos XML representan estructuras jerarquicas que se usan a menudo en la programacion.
## Atributos
Los elementos XML pueden tener **atributos** que proporcionan informacion adicional sobre el elemento.  
El valor del atributo siempre se establece entre comillas simples o dobles. Por ejemplo, el nombre de una imagen se puede escribir de la siguiente manera.
~~~xml
<picture name="The Black Square"/>
~~~
Si el valor del atributo contiene comillas dobles, debe usar comillas simples. Por ejemplo:
~~~xml
<picture name='"Sunset at Sea", Ivan Aivazovsky'/>
~~~
A veces, tambien puede ver comillas reemplazadas por simbolos de entidades especiales (`&quot;`):
~~~xml
<picture name="&quot;Sunflowers&quot;, Vincent van Gogh"/>
~~~
Un elemento puede contener mas de un atributo:
~~~xml
<picture name='Sunset at Sea' author='Ivan Aivazovsky'/>
~~~
El siguiente documento XML representa una galeria de arte:
~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<gallery>
  <picture name='Sunset at Sea' painter='Ivan Aivazovsky'/>
  <picture name='The Black Square' painter='Kazimir Malevich'/>
  <picture name='Sunflowers' painter='Vincent van Gogh'/>
</gallery>
~~~
Como puede ver, en algunos casos, los atributos pueden reemplazar elementos secundarios. No hay consenso sobre que es mejor usar. Por lo general, depende de los datos que intenta modelar, sus herramientas para el procesamiento de XML y, por supuesto, las personas con las que trabaja. Tenga en cuenta que un elemento puede tener atributos y elementos secundarios juntos.
## Pros y contras de XML
XML ha ganado popularidad debido a sus aparentes ventajas:
- puede ser entendido facilmente tanto por maquinas como por personas.
- el formato se basa en estandares internacionales.
- tiene una estructura bien definida que facilita la busqueda y extraccion de informacion.
- los lenguajes de programacion modernos tienen bibliotecas para procesar documentos XML automaticamente.  

Al mismo tiempo, XML tiene una desventaja importante. Su sintaxis redundante provoca un mayor costo de almacenamiento y transporte. Es especialmente importante cuando necesitamos almacenar o transferir una gran cantidad de datos.
---
## Ejercicios
1. Escribe una XML que represente un conjunto de productos en una tienda en linea.
~~~xml
<products>
    <product>
        <name>Cellphone</name>
        <cost>123.4</cost>
    </product>
    <product>
        <name>Tablet</name>
        <cost>4567.3</cost>
    </product>
</products>
~~~
2. Escribe un XML que represente tu pais.
~~~xml
<country>
    <name>Colombia</name>
    <population>40000000</population>
</country>
~~~