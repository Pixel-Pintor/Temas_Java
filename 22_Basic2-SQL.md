# Declaracion SELECT basica
En una declaracion `SELECT` puede especificar mas de un valor separado por una coma. Por ejemplo, la siguiente consulta selecciona un literal de cadena, un literal numerico y una expresion aritmetica.
~~~sql
SELECt 'Alice', 170, 170 * 0.393701;
~~~
Tal conjunto de valores se denomina **tupla**. En realidad, el resultado de `SELECT 'Hello, World!';` es tambien una fila con un solo atributo.  
En una consulta, puede especificar un nombre **(alias)** para cada atributo de la tupla. Para hacerlo, debe usar la palabra clave `AS` seguida de un nombre para el valor. Si el alias del aributo consta de varias palabras o coincide con una palabra de SQL, debe indicarse entre comillas dobles:
~~~sql
SELECT
    'Alice' AS name,
    170 AS height_in_centimeters,
    170*0.393701 AS "height in inches"
;
~~~
Es buena idea especificar alias legibles, alternativamente su sistema de gestion de datos puede generar algunos por usted. En realidad, el resultado de la consulta de ejemplo tambien es una tabla con nombres de columna especificados en alias y que consta de una sola fila.  
Estas son tres versiones de formato validas para una consulta SQL:
~~~sql
SELECT 'Bob' AS name, 160 AS "height in centimeters", 160*0.393701 AS "height in inches";

SELECT 
  'Bob' AS "name", 
  160 AS "height in centimeters", 
  160*0.393701 AS "height in inches"
;

SELECT 
  'Bob'        AS "name", 
  160          AS "height in centimeters", 
  160*0.393701 AS "height in inches"
;
~~~
---
## Ejercicios
1. Seleccione todas las consultas validas.
~~~sql
select 10, 2020 as year;
SELECT 'Alice';
~~~
2. Escriba una consulta que seleccione toda la informacion de Bob. Usa nombres de columnas como alias.
~~~sql
SELECT 
    542 AS "ID",
    'Bob' AS "name",
    'B0b' AS "username",
    4.3 AS "karma",
    'IT' AS "topic"
;

Query result:
+-----+------+----------+-------+-------+
| ID  | name | username | karma | topic |
+-----+------+----------+-------+-------+
| 542 | Bob  | B0b      | 4.3   | IT    |
+-----+------+----------+-------+-------+
Affected rows: 1
~~~
3. Cual sera el resultado de la consulta?.
~~~sql
SELECT 'Hello' AS greeting, 'Data' AS name;

// Produce una tupla con dos atributos
~~~
4. Escriba una consulta que haga un calculo.
~~~sql
SELECT 2+2, 2-2, 2*2, 2/2;
~~~
5. Cuales son los atributos nombre de la siguiente consulta.
~~~sql
SELECT 'Alice' AS Bob, 'Bob' AS Alice, 1 AS Two, 2 AS One;

// Bob, Alice, Two, One
~~~
---
# Declaracion SELECT FROM
Sabes que para extraer todos los datos de una tabla llamada "table_name" debes usar la siguiente consulta:
~~~sql
SELECT * FROM table_name;
~~~
Suponga que tiene una tabla "weather" que almacena informacion sobre el clima en Londres durante los ultimos 5 dias. Escribamos una consulta que selecciones solo la informacion basica que se mostrara en la pantalla de un telefono movil.
~~~sql
SELECT
    day, 
    hour,
    phenomena,
    temperature as "temperature in Celsius",
    feels_like as "feels like in Celsius",
    wind_speed as "wind speed in m/s"
FROM
    weather
;
~~~
Despues de `SELECT`, enumeramos las columnas que queremos seleccionar y especificamos el alias donde sea necesario. Aqui hay un esquema general para tales consultas: palabra `SELECT`, lista de nombres de columna con alias opcionales, palabra `FROM`, el nombre de la tabla y un punto y coma para marcar el final de la instruccion.
~~~sql
SELECT
    col1 [AS alias1], ..., colN [AS aliasN]  
FROM
    table_name
;
~~~
Ahora, imaginemos que por alguna razon necesitamos obtener diferentes resultados basados en los mismos datos, por ejemplo, agregar columnas que indiquen el ligar, muestren la temperatura en Fahrenheit y calculen si se siente mas frio que eso. La siguiente consulta hace exactamente eso.
~~~sql
SELECT
    'London' as place,
    day, 
    hour,
    phenomena,
    temperature*9/5+32 as "temperature in Fahrenheit",
    feels_like < temperature as "feels colder",
    wind_speed as "wind speed in m/s"
FROM
    weather
;
~~~
Si, tambien puede usar nombres de columnas en expresiones. Cuando el sistemas de gestion de datos ejecuta la consulta, sustituira los nombres de columna con el valor del atributo correspondiente para cada fila procesada.
---
## Ejercicios
1. Cual seria una consulta valida para la tabla.
~~~sql
CREATE TABLE Impression (
    banner_id INTEGER,
    background_color VARCHAR(6),
    banner_width REAL,
    banner_height REAL,
    cost_per_click DECIMAL(3,3),
    is_click BOOLEAN
);

// consulta valida
SELECT banner_id, cost_per_click FROM Impression;
~~~
1. Escriba una consulta que seleccione todo de la tabla sin usar *.
~~~sql
SELECT
    id,
    name,
    birth_year,
    gender
FROM
    Census
;

Query result:
+----+--------+------------+--------+
| id | name   | birth_year | gender |
+----+--------+------------+--------+
| 1  | Jessie | 2000       | M      |
| 2  | Kelly  | 1880       | F      |
| 3  | Willie | 1985       | M      |
| 4  | Taylor | 2018       | M      |
| 5  | Jessie | 1946       | F      |
+----+--------+------------+--------+
Affected rows: 5
~~~
2. Escriba una consulta que cree una **proyeccion** con los datos modificados.
~~~sql
SELECT
    year,
    discipline,
    (place + 1) AS "place",
    concat(concat(first_name, ' '), last_name) AS "full_name"
FROM
    winners
;
~~~
3. Cuantas columnas de la primera tabla se referncias en la consulta.
~~~sql
CREATE TABLE Weather (
    day INTEGER,
    hour INTEGER,
    temperature INTEGER,
    feels_like INTEGER,
    wind_speed INTEGER,
    wind_direction VARCHAR(2),
    wind_gusts INTEGER,
    humidity DECIMAL(3,2),
    cloudiness DECIMAL(3,2),
    phenomena VARCHAR(50),
    pressure INTEGER
);

SELECT
    'Chicago' as place,
    day, 
    hour,
    phenomena,
    temperature*9/5+32 as "temperature in Fahrenheit",
    feels_like*9/5+32 as "feels like in Fahrenheit", 
    feels_like < temperature as "feels colder",
    wind_speed > 7 as "windy",
    humidity,
    cloudiness
FROM
    Weather
;

// Referencia en total 8 columnas de Weather
~~~
4. Escribe una consulta que seleccione todos los datos relacionados con el viente y la hora. Las columnas de su consulta deben ir en orden alfabetico.
~~~sql
CREATE TABLE weather (
    hour INTEGER,
    temperature INTEGER,
    feels_like INTEGER,
    wind_speed INTEGER,
    wind_direction VARCHAR(2),
    wind_gusts INTEGER,
    humidity DECIMAL(3,2),
    phenomena VARCHAR(50),
    pressure INTEGER
);

SELECT
    hour,
    wind_direction,
    wind_gusts,
    wind_speed
FROM
    weather
;

Query result:
+------+----------------+------------+------------+
| hour | wind_direction | wind_gusts | wind_speed |
+------+----------------+------------+------------+
| 3    | NW             | 7          | 7          |
| 9    | NW             | 7          | 2          |
| 15   | NW             | 6          | 2          |
| 21   | E              | 2          | 2          |
+------+----------------+------------+------------+
Affected rows: 4
~~~
5. Escribe una consulta que modifique los datos.
~~~sql
CREATE TABLE Weather (
    hour INTEGER,
    temperature INTEGER,
    feels_like INTEGER,
    wind_speed INTEGER,
    wind_direction VARCHAR(2),
    wind_gusts INTEGER,
    humidity DECIMAL(3,2),
    phenomena VARCHAR(50),
    pressure INTEGER
);

SELECT
    hour AS "The Current Hour",
    feels_like / temperature AS "Weather Feel",
    pressure AS "The Current Pressure"
FROM
    Weather;

 Query result:
+------------------+--------------+----------------------+
| The Current Hour | Weather Feel | The Current Pressure |
+------------------+--------------+----------------------+
| 3                | 0.4000       | 763                  |
| 9                | 0.3333       | 766                  |
| 15               | 0.8333       | 764                  |
| 21               | 0.6000       | 765                  |
+------------------+--------------+----------------------+
Affected rows: 4   
~~~
5. Crea una tabla con los datos de los sobrevivientes del Titanic.
~~~sql
SELECT 
    CONCAT(first_name , " " , last_name) AS 'full name',
    1912-age AS 'birth year',
    passenger_class AS 'class'
FROM
    Titanic_passengers
;
~~~
---
# Logica y expresiones de comparacion
En la mayoria de los casos cuando procedemos con una consulta queremos extraer solo aquellos registros que cumplan con ciertos criterios. Para filtrar la seleccion esta el operador `WHERE`. Esta es sus sintaxis.
~~~sql
SELECT * 
FROM table
WHERE conditions
~~~
En `conditions` podemos insertar cualquier parametro con el que queramos que nuestros datos extraidos sean consistentes. Imaginemos que su primer cliente quiere comprar un libro de Charles Dickens. Escribamos una consulta que seleccione libros que cumplan con los criterios:
~~~sql
SELECT id, title, rating
FROM books
WHERE author = 'Charles Dickens';
~~~
Podemos usar operadores de comparacion **=, <, >, <=, =>, <>, !=**, por lo general se hacen comparaciones con valores numericos. En caso de que queramos hacer una seleccion por literales de cadena o fechas, debemos ponerlos entre comillas `""`.  
Digamos que tenemos una table de productos y queremos saber que productos de nuestra tabla cuestan mas de 250.
~~~sql
SELECT *
FROM products
WHERE price > 250;
~~~
Digamos que queremos seleccionar todos los productos de la tabla de la categoria `vegetables`.
~~~sql
SELECT *
FROM products
WHERE category = 'vegetables';
~~~
A continuacion, vamos a tratar con expresiones logicas que nos ayudan a formar consultas SQL mas complejas. Estos son los tres operadores del algebra booleana:
- `NOT` retorna `True` si el argumento es igual o `False` si no.
- `AND` compara operandos y retorna `True` solo si todos son `True`. Alternativamente retorna `False`.
- `OR` retorna `True` si al menos uno de los operandos es `True`. De lo contrario, retorna `False`.  
Tenemos una tabla llamada `personel` que contiene informacion sobre los programadores que trabajan en nuestra empresa. Imagina que queremos hacer una seleccion de los que encajan en nuestro proximo proyecto.
~~~sql
SELECT *
FROM personel
WHERE (status="Middle" OR status="Senior") AND skills="SQL"
~~~
Los parentesis se usan con el `OR` para priorizar el orden sobre `AND`. Podemos organizar la misma seleccion de criterios usando `NOT` en lugar de `OR`:
~~~sql
SELECT *
FROM personel
WHERE NOT(status="Junior") AND skills="SQL";
~~~
---
## Ejercicios
1. Selecciona los empleados que pertenezcan al departamento 'IT' y que tengan 3 o mas años de experiencia.
~~~sql
SELECT *
FROM staff
WHERE department = 'IT' AND experience >= 3;
~~~
2. Selecciona los carros que tengan una capacidad mayor que 2 y un rango nominal al menos de 200.
~~~sql
SELECT *
FROM cars
WHERE capacity > 2 AND nominal_range >= 200;

Query result:
+--------------+-------+-----------+----------+--------------+---------------+
| manufacturer | model | top_speed | capacity | acceleration | nominal_range |
+--------------+-------+-----------+----------+--------------+---------------+
| manufacturer | model | 90.0      | 7        | 8.0          | 200           |
| manufacturer | model | 50.0      | 5        | 6.0          | 300           |
+--------------+-------+-----------+----------+--------------+---------------+
Affected rows: 2
~~~
3. Cual es el resultado de la siguiente declaracion.
~~~sql
NOT (100 < 0) AND (17 < 20)

// True
~~~
3. Selecciona los empleados que tengan un sueldo al menos de 60000.
~~~sql
SELECT *
FROM staff
WHERE salary >= 60000;

Query result:
+----+---------------+--------+
| id | name          | salary |
+----+---------------+--------+
| 3  | Jack Wilson   | 60000  |
| 5  | Tedd Jonson   | 68000  |
| 6  | Tracy Walberg | 75000  |
| 8  | John Preston  | 62000  |
+----+---------------+--------+
Affected rows: 4
~~~
4. Selecciona los clientes que son de Berlin.
~~~sql
SELECT *
FROM customers
WHERE city = 'Berlin';

Query result:
+----+------------+--------+
| id | name       | city   |
+----+------------+--------+
| 4  | Victoria D | Berlin |
| 6  | Martin G   | Berlin |
+----+------------+--------+
Affected rows: 2
~~~
5. Escribe una consulta para la tabla books.
~~~sql
SELECT id, author, title
FROM Books
WHERE amount < 2;
~~~
6. Selecciona autos por su velocidad maxima y aceleracion.
~~~sql
CREATE TABLE cars (
    manufacturer VARCHAR(20),
    model VARCHAR(20),
    top_speed FLOAT,
    capacity INTEGER,
    acceleration FLOAT
);

SELECT *
FROM cars
WHERE top_speed > 240 AND acceleration < 3;
~~~
---
# Declaracion INSERT basica
Para usar una base de datos, debe saber como insertar nuevos registros en una tabla de base de datos. Puede insertar un nuevo registro en un tabla con una consulta usando `INSERT INTO`.  Por ejemplo, agreguemos un registro sobre un nuevo cliente.
~~~sql
INSERT INTO customers (name, surname, zip_code, city)
VALUES ('Bobby', 'Ray', 60601, 'Chicago');
~~~
Como puede ver, lo que debe hacer es escribir una lista de valores para insertar despues de la palabra `VALUES`. Una vez que hayamos ejecutado esta consulta, nuestra tabla clientes tendra una nueva fila.  
Si conoce el orden exacto de las columnas de la tabla, puede usar la version mas corta `INSERT INTO` sin especificar el nombre de las columnas.
~~~sql
INSERT INTO customers
VALUES ('Bobby', 'Ray', 60601, 'Chicago');
~~~
Si quiere insertar varias filas, no tiene que agregarlas una por una: puede agregar varias filas simultaneamente.
~~~sql
INSERT INTO customers (name, surname, zip_code, city)
VALUES ('Mary', 'West', 75201, 'Dallas'),
       ('Steve', 'Palmer', 33107, 'Miami') 
~~~
En este ejemplo, escribimos dos listas de valores separados por comas en lugar de solo una.  
A veces hay que insertar un registro sin ningun tipo de informacion. Supongamos que tenemos una tabla de gatos con columnas que no tienen un valor predeterminado. Queremos agregar informacion sobre Felix, un gato sin hogar de tres años. Como Felix no tiene dueño, podemos omitir esta columna en la consulta de insercion.
~~~sql
INSERT INTO cats (name, age)
VALUES ('Felix', 3);
~~~
El resultado es una fila que tiene un valor `NULL` para la fila de propietario. Si una columna tiene un valor predeterminado y omite esta columna durante la insercion, su valor se establecera en el valor predeterminado.
---
## Ejercicios
1. Seleccione todas las consultas validas para la table waiters.
~~~sql
CREATE TABLE waiters (
    name VARCHAR(30) NOT NULL,
    surname VARCHAR(30),
    hourly_wage INT NOT NULL DEFAULT 15,
    age INT NOT NULL CHECK (age >= 17)
);

INSERT INTO waiters (name, hourly_wage, age) VALUES ('Ann', 16, 20); // valido
INSERT INTO waiters (name, surname, age) VALUES ('Tom', 'Black', 20); // valido
INSERT INTO waiters VALUES (NULL, 'Smith', 15, 21); // no valido
INSERT INTO waiters (name, surname, age) VALUES ('John', 'Rose', 16); // no valido
INSERT INTO waiters VALUES ('Carl', 'Smith', 15, 17); // valido
~~~
2. Reordena la consulta.
~~~sql
INSERT INTO countries (name, capital, population) VALUES ('USA', 'Washington',  329500000), ('United Kingdom', 'London', 67220000);
~~~
3. Corrige la consulta.
~~~sql
INSERT INTO pupils (name, age, city) VALUES ('Laura', 15, 'Moscow');
~~~
4. Insertar Anna Baker en la tabla users.
~~~sql
INSERT INTO users VALUES ("Anna", "Baker");
~~~
5. Selecciona la consulta correcta.
~~~sql
CREATE TABLE animals (
    name VARCHAR(10) NOT NULL,
    owner VARCHAR(20),
    age INT NOT NULL
);

INSERT INTO animals (name, owner, age) VALUES ('Max', 'Ted', 1);
~~~
5. Cual sera el valor de la columna price despues de esta consulta.
~~~sql
INSERT INTO products (title, manufactures, price, quantity)
VALUES ('Vanilla Cake', 'Sweet INC', 10.99, 40);

// price = 10.99
~~~
6. Inserte la ciudad de nueva york en la tabla cities.
~~~sql
INSERT INTO cities (city, country, population) VALUES ('New York', 'USA', 8175133);
~~~
7. Inserte tres perros en la tabla dogs.
~~~sql
INSERT INTO dogs (name, dog_breed) 
VALUES ('Buddy', 'Akita'),
        ('Ruby', 'Mastiff'),
        ('Daisy', 'Bully');
~~~
8. Insertar un nuevo productos en products.
~~~sql
INSERT INTO products VALUES ('Laptop', 15, 'LG');
~~~
---
# Declaracion UPDATE basica
Que informacion es necesaria para hacer una actualizacion? Nombre de la **tabla** donde queremos cambiar los datos, **nombre(s)** de la columna donde residen los datos y una **expresion** para calcular un nuevo valor para cada columna especificada.
~~~sql
UPDATE table_name
SET col1 = expr1,
    col2 = expr2,
    ...,
    colN = exprN;
~~~
Generalmente, se permite utilizar cualquier expresion SQL valida. Puede escribir una combinacion correcta de literales, operadores, funciones y referencias de columna aqui, solo recuerda la **coherencia de tipo**.  
Tiene una tabla de personal llamada empleados. Para cada empleado tiene un id (entero), su apellido (texto), su salario (entero) y su limite superior (entero). Si por alguna razon, todos los trabajadores necesitan ser trasladados al departamento #14, podriamos escribir lo siguiente:
~~~sql
UPDATE employees
SET department_id = 14;
~~~
Dado que usamos un valor entero en la columna `department_id` de tipo entero, la consulta es correcta. Que pasa si queremos celebrar un cambio tan masivo en la estructura de la empresa y dar un aumento de sueldo a nuestros empleados. Los valores absolutos no serviran aqui, por lo que se deben usar sus salarios actuales.
~~~sql
UPDATE employees
SET salary = salary + 10000;
~~~
Durante la ejecucion de `UPDATE`, cada fila de una tabla se considera individualmente. Si quermos usar valores antiguos para calcular un nuevo valor para una celda, solo tendran en cuenta las celdas de la misma fila.  
Es posibles actualizar varias columnas simultaneamente, por lo que podemos lograr el mismo resultado usando solo una consulta en lugar de dos:
~~~sql
UPDATE employees
SET department_id = 14,
    salary = salary + 10000;
~~~
Tratemos de pensar en algo mas elaborado: fije los nuevos salarios al 80 por ciento de sus limites superiores y omita la parte fraccionario que pueda aparecer. Para la ultima parte del requisito, podemos usar la funcion `floor()` que toma un valor real y devuelve un valor entero.
~~~sql
UPDATE employees
SET salary = floor(0.8 * upper_limit);
~~~
---
## Ejercicios
1. Cuantas celdas afectara la consulta.
~~~sql
UPDATE flights SET departure = 'London', destination = 'Paris';

// afecta 9 celdas
~~~
2. Agregue el prefijo 'The' a los titulos de las peliculas y restele 1 a su año de publicacion.
~~~sql
UPDATE movies
SET title = CONCAT('The ' + title),
    release_year = release_year - 1;
~~~
3. Escriba una solicitud que reduzca 40 a la columna amount.
~~~sql
UPDATE governance
SET amount = amount - 40;
~~~
4. Cual consulta es correcta para actualizar una columna llamada "serial_number" tipo VARCHAR en una tabla llamada "warehouse".
~~~sql
UPDATE warehouse SET serial_number = '000120;
~~~
5. Organiza los elementos para que la consulta sea valida.
~~~sql
UPDATE storage
SET shelf_name = REVERSE(shelf_name);
~~~
6. Escriba una consulta que establezca en 90 la cantidad y en 224.5 el precio de la tabla stockes.
~~~sql
UPDATE stockes
SET amount = 90,
    price = 224.5;
~~~
