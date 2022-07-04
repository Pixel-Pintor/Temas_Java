# Introduccion a SQLite
Ejecutar un sistema de base de datos con MySQL o PostgreSQL requiere instalacion, configuracion y mantenimiento. A veces puede ser inadecuado para nosotros o incluso prohibido. En tales casos, SQLite viene al rescate.  
Una aplicacion con SQLite incrusta el codigo de la base de datos dentro de si misma, por lo que no es necesario un proceso de base de datos separado. Las sentencias SQL emitidas leen y escriben los archivos directamente en el disco local. La base de datos con todas las tablas y metadaso esta contenido en un solo archivo. A pesar de competir con los archivos regulares, SQLite se puede usar como una base de datos de back-end para una aplicacion web. Tambien es seguro acceder a la misma base de datos SQLite al mismo tiempo desde multiples aplicaciones y procesos.  
Para instalar y usar SQLite, debe agregar su dependencia a un administrador de paquetes o crear un sistema de su eleccion. Usaremos la interfaz de comandos `sqlite3`, vamos a crear una nueva base de datos `edu.db` y a llenarlo.
~~~sql
hs@laptop:~/sqlite$ ls
hs@laptop:~/sqlite$ sqlite3 edu.db
SQLite version 3.31.1 2020-01-27 19:55:54
Enter ".help" for usage hints.
sqlite> create table pets (pet_id integer, name text);
sqlite> insert into pets (pet_id, name) values (1, 'Harry'), (2, 'Elen');
sqlite> create table pet_weights (pet_id integer, weight integer);
sqlite> insert into pet_weights (pet_id, weight) values (1, 10), (2, 11);
sqlite>
~~~
Al finalizar una sesion interactiva vemos un nuevo archivo `edu.db`. Al iniciar de nuevo la sesion, podemos verificar que los datos fueron guardados.
~~~sql
hs@laptop:~/sqlite$ sqlite3 edu.db
SQLite version 3.31.1 2020-01-27 19:55:54
Enter ".help" for usage hints.
sqlite> select * from pets;
1|Harry
2|Elen
sqlite> select * from pet_weights;
1|10
2|11
~~~
La mayoria de bases de datos SQL utilizan tipos estaticos: el tipo de valor insertado en una columna debe coincidir con su tipo de declaracion de columna. No puede insertar in valor de String en una columna de enteros. SQLite utiliza tipos dinamicos, lo que significa que puede almacenar cualquier tipo de valor en cualquier columna, independientemente del tipo de columna. Incluso declarar tipos de columnas es opcional. La siguiente definicion funcionara bien:
~~~sql
CREATE TABLE pet_weights (pet_id, weight);
~~~
Las sentencias SQL que funcionan en bases de datos tipificadas estaticamente funcionan de la misma manera en SQLite, a pesar de su naturaleza dinamica.
---
## Ejercicios
1. Crear una tabla e insertar datos.
~~~sql
CREATE TABLE Album (name text, year integer);

INSERT INTO Album (name) VALUES ('Picnic');
INSERT INTO Album (name, year) VALUES ('Picnic', 2009), ('Hike', 2011);
INSERT INTO Album (name, year) VALUES ('Picnic', 2009);
~~~
