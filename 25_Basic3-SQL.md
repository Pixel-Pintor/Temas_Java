# Ordenar resultados
Cuando consultas datos, SQL no proporciona ningun orden predeterminado de filas en el resultado de la evaluacion de la consulta. Para especificar el roden de las filas resultantes, debe utilizar `ORDER BY`.
~~~sql
SELECT
    hotel_id,
    hotel_name,
    price_per_night,
    price_for_early_check_in,
    rating,
    stars
FROM
    hotels
ORDER BY
    price_per_night
;
~~~
Al final de la declaracion `SELECT`, hemos especificado que las filas resultantes deben ordenarse por `price_by_night`.  
Tambien puede ordenar las filas por expresiones. Por ejemplo, en la consulta a continuacion, ordenamos hoteles por precio para dos noches con check-in anticipado.
~~~sql
SELECT
    hotel_id,
    hotel_name,
    price_per_night,
    price_for_early_check_in,
    rating,
    stars
FROM
    hotels
ORDER BY
    price_per_night*2 + price_for_early_check_in
;
~~~
La clasificacion se basa en la definicion del operador de comparacion `<` para el tipo de expresion. Puede especificar si los valores mayores o menores deben colocarse mas arriba en la lista. Consideremos el ejemplo:
~~~sql
SELECT 
    hotel_id,
    hotel_name,
    price_per_night,
    price_for_early_check_in,
    rating,
    stars
FROM
    hotels
ORDER BY
    rating DESC
;
~~~
Las palabras `ASC` o `DESC`, especifican si el orden es ascendente o descendente. Por defecto, se supone que el orden es ascendente.  
Escribamos una consula que ordene los hoteles por precio y clasificacion:
~~~sql
SELECT
    hotel_id, 
    hotel_name, 
    price_per_night,
    price_for_early_check_in,
    rating, 
    stars
FROM 
    hotels
ORDER BY
    rating DESC,
    price_per_night*2 + price_for_early_check_in 
;
~~~
Los ultimos valores se utilizan para ordenar las filas que son iguales segun los valores anteriores. Cada expresion puede ir seguida de un `ASC` o `DESC`. En nuestro ejemplo, los hoteles deben ordenarse por calificacion (de mayor a menor), y aquellos son calificaciones iguales deben ordenarse por precio.  
Si ordena las filas resultantes por una expresion que define un atributo de resultado, puede abordarlos en la clausula `ORDER BY` por un numero o alias de columna. Por ejemplo, en la consulta a continuacion, ordenamos las filas por precio total y calificacion:
~~~sql
SELECT 
    hotel_name,
    price_per_night*2 + price_for_early_check_in AS total_price,
    rating,
    stars
FROM 
    hotels
ORDER BY
    total_price, 3 DESC
;
~~~
## Ejercicios
1. Considere la consulta y establezca una declaracion de order que cumpla con los parametros de busqueda de du enunciado.
~~~sql
SELECT 
    hotel_name,
    2 * price_per_night + price_for_early_check_in AS price,
    rating,
    stars
FROM
    hotels
ORDER BY
    stars DESC, // Hoteles con muchas estrellas al principio
    rating DESC, // Hoteles con bajo rating al final
    price_per_night, // Hoteles con precio bajo por noche al principio
    price // Hoteles con un precio total alto al final
;
~~~
2. Completa la consulta para que ordene los resultados por estrellas y por precio por noche.
~~~sql
CREATE TABLE hotels (
    hotel_name VARCHAR(100),
    price_per_night DECIMAL(10, 2),
    price_for_early_check_in DECIMAL(10, 2),
    rating FLOAT,
    stars INTEGER
);

SELECT 
    hotel_name,
    2 * price_per_night + price_for_early_check_in AS price,
    rating,
    stars
FROM
    hotels
ORDER BY
    stars DESC,
    price_per_night
;
~~~
3. Crear una consulta que obtenga toda la informacion de los pasajeros pero la organice por el tamaño de sus familias en orden descendente.
~~~sql
SELECT
    *
FROM
    Titanic_passengers
ORDER BY
    parch + sibsp DESC
;

Query result:
+--------------+----------+-----------------+------------+---------------+---------+-----+-------+-------+
| passenger_id | survived | passenger_class | first_name | last_name     | is_male | age | sibsp | parch |
+--------------+----------+-----------------+------------+---------------+---------+-----+-------+-------+
| 8            | 0        | 3               | Palsson    | Gosta Leonard | 1       | 2   | 3     | 1     |
| 1            | 0        | 3               | Braund     | Owen Harris   | 1       | 22  | 1     | 0     |
| 4            | 1        | 1               | Futrelle   | Jacques Heath | 0       | 35  | 1     | 0     |
| 10           | 1        | 2               | Nasser     | Nicholas      | 0       | 14  | 1     | 0     |
| 3            | 1        | 3               | Heikkinen  | Laina         | 0       | 26  | 0     | 0     |
+--------------+----------+-----------------+------------+---------------+---------+-----+-------+-------+
Affected rows: 5
~~~
3. Define la tabla y filtra ciertos datos de los pasajeros, ordene los datos por clase con los mas jovenes al principio.
~~~sql
CREATE TABLE Titanic_passengers (
    passenger_id INTEGER,
    survived BOOLEAN,
    passenger_class INTEGER,
    first_name VARCHAR(20),
    last_name VARCHAR(20),
    is_male BOOLEAN,
    age INTEGER,
    sibsp INTEGER,
    parch INTEGER
);

SELECT 
    first_name,
    last_name,
    age,
    passenger_class
FROM 
    Titanic_passengers
ORDER BY
    passenger_class, 
    age 
;

Query result:
+------------+---------------+-----+-----------------+
| first_name | last_name     | age | passenger_class |
+------------+---------------+-----+-----------------+
| Futrelle   | Jacques Heath | 35  | 1               |
| Nasser     | Nicholas      | 14  | 2               |
| Palsson    | Gosta Leonard | 2   | 3               |
| Braund     | Owen Harris   | 22  | 3               |
| Heikkinen  | Laina         | 26  | 3               |
+------------+---------------+-----+-----------------+
Affected rows: 5
~~~
4. Escriba una consulta que seleccione todos los hoterels ordenados por estrellas, calificacion y precio total por 5 noches con check-in anticipado.
~~~sql
SELECT *
FROM hotels
ORDER BY
    stars,
    rating DESC,
    price_per_night * 5 + price_for_early_check_in
;
~~~sql

Query result:
+--------------------------------------------+-----------------+--------------------------+--------+-------+
| hotel_name                                 | price_per_night | price_for_early_check_in | rating | stars |
+--------------------------------------------+-----------------+--------------------------+--------+-------+
| ibis London Canning Town                   | 55.00           | 0.00                     | 8.9    | 3     |
| ibis London Docklands Canary Wharf         | 70.00           | 20.00                    | 8.5    | 3     |
| Native Hyde Park                           | 160.00          | 50.00                    | 8.4    | 4     |
| Holiday Inn London Kensington Forum        | 100.00          | 50.00                    | 7.7    | 4     |
| Britannia International Hotel Canary Wharf | 50.00           | 0.00                     | 6.8    | 4     |
| M by Montcalm Shoreditch London Tech City  | 145.00          | 45.00                    | 9.4    | 5     |
| Intercontinental London - The O2           | 130.00          | 1000.00                  | 9.4    | 5     |
+--------------------------------------------+-----------------+--------------------------+--------+-------+
Affected rows: 7
~~~
5. Escriba una consulta que organice los datos de la table por año de nacimiento descendiente y nombre ascendiente.
~~~sql
CREATE TABLE Census (id INTEGER, 
                     name VARCHAR(20), 
                     birth_year INTEGER, 
                     gender VARCHAR(1));

SELECT *
FROM Census
ORDER BY
    birth_year DESC,
    name;

Query result:
+----+--------+------------+--------+
| id | name   | birth_year | gender |
+----+--------+------------+--------+
| 4  | Taylor | 2018       | M      |
| 1  | Jessie | 2000       | M      |
| 6  | Allie  | 1985       | M      |
| 3  | Willie | 1985       | M      |
| 5  | Jessie | 1946       | F      |
| 2  | Kelly  | 1880       | F      |
+----+--------+------------+--------+
Affected rows: 6
~~~
---
# Declaracion basica DELETE
En la vida real no solo coleccionamos registros, a veces queremos deshacernos de ellos. Para eliminar los datos de la tabla, pero no la tabla en si, use una consulta con la declaracion `DELETE FROM`. Por ejemplo eliminemos toda la informacion de la tabla llamada `books`.
~~~sql
DELETE FROM books;
~~~
Como resultado de la consulta, aun teneos la tabla `books`, pero ahora esta vacia.  
Tambien puede eliminar las filas seleccionadas de una tabla. Podemos eliminar registros sobre libros especificos usando `WHERE`.
~~~sql
DELETE FROM books
WHERE quantity = 0
~~~
Esta consulta eliminara todas las filas de los libros donde sea cierta la expresion logica `quantity = 0`.
---
## Ejercicios
1. Borra todos los registros sobre Jane Smith de la tabla `guests`.
~~~sql
DELETE FROM guests
WHERE name = 'Jane Smith';
~~~
2. Tenemos una tabla de peliculas, borrar todas las peliculas que se hayan lanzado despues de 1980 y que tengan un rating menor que 8.5.
~~~sql
DELETE FROM movies
WHERE year >= 1980 AND rating < 8.5;
~~~
3. Cuantas filas de la tabla permaneceran despues de ejecutar la consulta.
~~~sql
DELETE FROM employees WHERE last_name = 'Davis';

// elimina 2 filas y quedan 3
~~~
4. Seleccione las consultas sintacticamente correctas.
~~~sql
DELETE FROM renters WHERE lease_agreement = 'unsigned';
DELETE FROM renter;
~~~
5. Borrar todo de la tabla `users`.
~~~sql
DELETE FROM users;
~~~
6. Borrar todos los gatos siameses de la tabla.
~~~sql
DELETE FROM cats
WHERE breed = 'Siamese';
~~~
7. Restaure la consulta que eliminara todos los registros de la tabla productos en los que la cantidad sea inferior a 10, el precio superior a 150 y la calificacion inferior a 3,5.
~~~sql
DELETE FROM products
WHERE quantity < 10 AND price > 150 AND rating < 3.5;
~~~
8. Selecciona la declaracion correcta sobre la consulta.
~~~sql
DELETE FROM tasks WHERE task_status = 'Cancelled';

// La consulta borrar todas las filas con tareas canceladas
~~~
9. Complete la consulta que borre las filas que tengan un salario mayor que 40.000 y experiencia menor que 10.
~~~sql
DELETE FROM staff
WHERE salary > 40000 AND experience < 10;
~~~
---
# Actualizar filas seleccionadas
El filtrado de filas que no nos interesan se pueden hacer con la clausula `WHERE`. El unico requisito de una declaracion `WHERE` es que debe producir un valor `BOOLEAN` para cada fila de la tabla. Solo se actualizaran aquellas filas para las que la expresion produzca **VERDADERO**.  
Por ejemplo, imagina qe hay una table llamada `groups`. Esta tabla almacena informacion sobre los grupos de estudiantes existentes en la universidad del condado North-Western.  
Debido a la politica de la universidad la cantidad de estudiantes en todos los grupos que toman algebra debe reducirse a 40. Y los estudiantes de la señorita `Laura Gibbs` deben aumentarse hasta los 40.
~~~sql
UPDATE groups
SET capacity = 40
WHERE 
    course LIKE '%Algebra%'
    OR tutor = 'Laura Gibbs'
~~~
La clausula `WHERE` no cambia mucho en la plantilla basica para las consultas `UPDATE`, pero juego un papel crucial. No desea introducir cambios innecesarios en sus datos y tener que corregirlos despues. Como primer paso para redactar su consulta, piense que filas exactas deben actualizarse y escriba la parte `WHERE`; despues de hacer eso, sientase libre de cambiar los datos de la forma que desee.
Aqui esta la plantilla general de una declaracion `UPDATE`:
~~~sql
UPDATE table_name
SET     
    col1 = expr1,
    col2 = expr2,
    ...,
    colN = exprN
WHERE
    logical_expression;
~~~
---
## Ejercicios
1. Nombre las partes de la consulta con sus significados.
~~~sql
UPDATE music SET is_trending = false WHERE genre = 'Alternative rock';

# false -> nuevo valor para la columna especificada
# UPDATE -> una tipo especificador de consulta
# WHERE -> inicio de la clausula de condicion
# is_trending -> una columna para actualizar sus valores
# 'Alternative rock' -> un valor que las filas que seran actualizadas debe tener
# music -> nombre de la tabla
~~~
2. Escribe una consulta que actualice el sponsor de todos los estadios que tengan una capacidad mayor a 76000 personas.
~~~sql
UPDATE stadiums
SET
    sponsor = 'Nike'
WHERE
    capacity > 76000;
~~~
3. Seleccione una consulta correcta.
~~~sql
UPDATE store SET employees = 100 WHERE city = 'Boston';
~~~
3. La longitud de Water Sports Arena debe ser el doble y no deberia ser en un solo sentido. Tambien deberia agregar dos cruces y una parada de auto-bus. Escriba una consulta que refleje todos esos cambios.  
~~~sql
UPDATE streets
SET
    length = length * 2,
    crossings = crossings + 2,
    bus_stops = bus_stops + 1,
    is_one_way = false
WHERE
    street_name = 'Water Sports Arena';
~~~
4. Los juegos "Bridges" y "Ship Simulator 2" ahora seran distribuidos por "Macrofiber". Pero el rating de los usuarios se redujo 2.5 puntos, actualiza la tabla.
~~~sql
UPDATE games
SET 
    publisher = 'Macrofiber',
    users_rating = users_rating - 2.5
WHERE
    title = 'Bridges' OR title = 'Ship Simulator 2';
~~~
5. Mueve a Estanbul a los agentes que tengan mas de 10 misiones y tengan conocimiento de mas de 3 lenguajes.
~~~sql
UPDATE agents
SET 
    current_location = "Istanbul",
    missions = missions + 1
WHERE 
    knows_languages > 3
    AND
    missions > 10
~~~
