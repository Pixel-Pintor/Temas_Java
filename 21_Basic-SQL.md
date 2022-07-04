# Literales
En casi cualquier programa o script de analisis de datos, debe operar con valores constantes llamados **literales**. Por ejemplo, si esta analizando datos del censo y necesita extraer filas del censo segun criterios especificos (personas llamadas Jessis nacidas entre 1920 y 2000), a menudo tendra que usar literales (Jessie, 1920 y 2000).  
Una constante de cadena en SQL es una secuencia de caracteres enter comillas simples o dobles. Para incluir caracteres de comillas simples dentro de un literal de cadena entre comillas simples, escriba dos comillas simples adyacentes, por ejemple 'Jessie''s birthday'. Tambien la puede encerrar entre comillas dobles "Sophie's choice".  
Los **literales numericos** pueden ser numeros positivos o negativos especificados como enteros, decimales o valores reales en exponencial. Si no especifica el signo, se asume un numero positivo por defecto. Los literales numericos pueden ser **INTEGER**, **REAL** y **DECIMAL**, el sistema de gestion de datos define automaticamente su tipo en funcion del contexto. Puedes especificar directamente el tipo de un literal mediante `CAST (value AS type)`. En lugar de marcadores de posicion de valor y tipo, puede usar su literal y tipo.
~~~sql
SELECT CAST(1 AS DECIMAL(20, 3));
~~~
En el ejemplo anterior, el numero 1 se interpreta como `DECIMAL(20, 3)` y 1.000 como resultado de la consulta. Este codigo SQL (en realidad es una consulta) implementa un programa basico:
~~~sql
SELECT 'Hello, World!';
~~~
El resultado de la evaluacion de la consulta es el siguiente:
~~~
Hello, World!
~~~
En realidad, la consulta establece declarativamente que queremos seleccionar esta cadena como resultado. La de declaracion consta de 3 partes: la palabra **SELECT**, el literal que queremos recibir y un punto y coma que define el final de la consulta. En resumen, una consulta SQL simple que extrae cualquier literal, ya sea una cadena, numerico o booleano, tiene el siguiente aspecto (pueden reemplazar `literal` con cualquier constante especificada correctamente que desee):
~~~sql
SELECT literal;
~~~
---
## Ejercicios
1. Escriba una consulta que retorne 256 como resultado del tipo `DECIMAL(10, 2)`.
~~~sql
SELECT CAST(256 AS DECIMAL(10, 2));

Query result:
+-----------------------------+
| CAST(256 AS DECIMAL(10, 2)) |
+-----------------------------+
| 256.00                      |
+-----------------------------+
Affected rows: 1
~~~
2. Escribe una consulta que retorne 1024 como resultado.
~~~sql
SELECT 1024;

Query result:
+------+
| 1024 |
+------+
| 1024 |
+------+
Affected rows: 1
~~~
3. Escriba una consulta que retorne FALSE como resultado.
~~~sql
SELECT FALSE;

Query result:
+-------+
| FALSE |
+-------+
| 0     |
+-------+
Affected rows: 1
~~~
---
# Expresiones Aritmeticas
El conjunto basico de operadores aritmeticos admitidos en SQL es el siguiente:
- `-` (menos unario que cambia el signo de un valor)
~~~sql
SELECT -2;
> -2
~~~
- `*` (multiplicacion), `/` (division), `%` (modulo)
~~~sql
SELECT 20*15;
> 300
SELECT 3/5;
> 0.60
SELECT 18%4;
> 2
~~~
- `+` (suma), `-` (sustraccion)
~~~sql
SELECT 30+234;
> 264
SELECT 3-5;
> -2
~~~
Tambien puede utilizar corchetes para mejorar la legibilidad del codigo incluso si no los necesita para la evaluacion correcta de la expresion. `-2+2*2-2/2` y `(-2)+(2*2)-(2/2)` son iguales.  
En SQL puede seleccionar no solo un literal sino tambien una expresion, por ejemplo el siguiente codigo evalua la expresion `(2+2)*15`.
~~~sql
SELECT (2+2)*15;
~~~
El resultado de la evaluacion de la consulta es `60`.
---
## Ejercicios
1. Escribir una consulta con el promedio de calificaciones.
~~~sql
SELECT (4+5+4+4+3+5+4) / 7;
~~~
2. Cual es resultado de la consulta.
~~~sql
SELECT (15+1)%9;

// 7
~~~
3. Encontrar el area.
~~~sql
SELECT 5 * 4 / 2;

Query result:
+-----------+
| 5 * 4 / 2 |
+-----------+
| 10.0000   |
+-----------+
Affected rows: 1
~~~
---
# Declaracion basica CREATE
Almacenamos informacion sobres estudiantes universitarios en una nueva base de datos. Podemos usar **CREATE DATABASE** para esto.
~~~sql
CREATE DATABASE students;
~~~
Podemos crear la tabla con algunas columnas.
~~~sql
CREATE TABLE students_info ( 
    student_id INT, 
    name VARCHAR(30), 
    surname VARCHAR(30), 
    age INT
);
~~~
Para eliminar una base de datos, puede usar la instruccion **DROP DATABASE**.
~~~sql
DROP DATABASE students;
~~~
Tenga en cuenta que si elimina la base de datos, perdera todas las tablas almacenadas en ella. Tambien puede eliminar solo una tabla de la base de datos.
~~~sql
DROP TABLE students_info;
~~~
---
## Ejercicios
1. Cuantas columnas en la tabla almacenan datos numericos.
~~~sql
CREATE TABLE products (
    title VARCHAR(60),
    manufacturer VARCHAR(40),
    manufacturer_address VARCHAR(100),
    vendor_code INT,
    shop VARCHAR(40),
    shop_address VARCHAR(100),
    weight INT,
    cost_price REAL,
    shop_price REAL,
    quantity INT,
    sell_online BOOLEAN,
    receipt_date DATE,
    index TINYINT
);

// 6
~~~
2. Crea la tabla staff.
~~~sql
CREATE TABLE staff (
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    badge_id INT,
    department VARCHAR(30),
    position VARCHAR(15)
);
~~~
3. Crear tabla persons.
~~~sql
CREATE TABLE persons (
    person_id INT,
    name VARCHAR(20),
    surname VARCHAR(20),
    birth_date DATE,
    city VARCHAR(25)
);
~~~
4. Consulta que elimina la base de datos shop_info.
~~~sql
DROP DATABASE shop_info;
~~~
5. Crea la tabla schools.
~~~sql
CREATE TABLE schools (
    school_num INT,
    city_id INT,
    foundation_date DATE
);
~~~
---
# El valor NULL
El valor **NULL** se usa en SQL para indicar que algun valor de datos es desconocido o no esta definido. Las expresiones aritmeticas o de cadena con **NULL** entre los operandos se evaluan como **NULL**, por ejemplo, `2 + 2 * NULL` es igual a **NULL**.  
Un valor NULL se puede almacenar en una columna de cualquier tipo. Sin embargo, un ingeniero de software puede usar una restriccion `NOT NULL` en la creacion de la tabla para especificar que una columna no debe almacenar valores NULL. Por ejemplo, de acuerdo con este codigo, solo la columna "winner_birth_year" puede contener valores NULL.
~~~sql
CREATE TABLE winners (
    year INTEGER NOT NULL,
    field VARCHAR(20) NOT NULL,
    winner_name VARCHAR(100) NOT NULL,
    winner_birth_year INTEGER
);
~~~
El valor NULL basicamente significa "El valor no esta presente". Debido a esto, las comparaciones con NULL nunca pueden resulta en TRUE o FALSE, sino que en un tercer resultado logico, **UNKNOWN**. Por ejemplo, el resultado de cada una de las siguientes comparaciones es **UNKNOWN**:
- NULL = 1
- NULL <> 1
- NULL > 1
- NULL = '1'
- NULL = NULL  
SQL permite predicados especiales: `IS NULL` y `IS NOT NULL`. Por ejemplo, las consultas a continuacion devuelven TRUE como resultado:
~~~sql
SELECT 0+NULL IS NULL;
SELECT '' IS NOT NULL;
~~~
Las siguientes son iguales a FALSE:
~~~sql
SELECT NULL IS NOT NULL;
SELECT 1-1 IS NULL;
~~~
En expresiones logicas con operandos **UNKNOWN**, el resultado es **UNKNOWN** si depende de un operando **UNKNOWN**. Por lo tando, a diferencias de las comparaciones, el resultado de una expresion logica puede ser algo distinto a **UNKNOWN** aunque un operando sea **UNKNOWN**. Consideremos los siguientes ejemplos:
- `(NULL = 1) AND TRUE` se evalua como `UNKNOWN` el resultado sera `TRUE` solo si ambos operandos son `TRUE`.
- `(NULL = 1) OR TRUE` es igual a `TRUE` el resultado es `TRUE` por que al menos un operando es `TRUE`.
---
## Ejercicios
1. Escribe una consulta que contenga TRUE, FALSE y NULL, pero que igual retorne TRUE.
~~~sql
SELECT TRUE OR FALSE OR NULL;
SELECT TRUE AND FALSE IS NOT NULL;
SELECT (FALSE AND NULL) OR TRUE;

Query result:
+-----------------------+
| true or false or null |
+-----------------------+
| 1                     |
+-----------------------+
Affected rows: 1
~~~
2. Conecte las expresiones con sus valores correspondientes.
~~~sql
(NULL = '') OR TRUE; // TRUE
FALSE AND (NULL = 0); // FALSE
(NULL <> NULL) AND TRUE; // UNKNOWN
256 - 16*16 + NULL; NULL
~~~
3. Escriba una consulta que contenga, TRUE, FLASE y NULL, y retorne UNKNOWN.
~~~sql
SELECT NULL > false AND true;
SELECT NULL > true < false;
SELECT true AND (NULL = 1) OR false;
SELECT true AND false + NULL;

Query result:
+-----------------------+
| TRUE AND FALSE + NULL |
+-----------------------+
| NULL                  |
+-----------------------+
Affected rows: 1
~~~
3. Escribe una consulta que divida 1 por NULL.
~~~sql
SELECT 1 / NULL;

Query result:
+----------+
| 1 / NULL |
+----------+
| NULL     |
+----------+
Affected rows: 1
~~~
4. Cuales columnas pueden tener valores NULL.
~~~sql
CREATE TABLE impression( 
    banner_id INTEGER NOT NULL,
    is_click BOOLEAN NOT NULL, 
    user_id VARCHAR(100), 
    geo VARCHAR(3),
    site_id INTEGER NOT NULL
);

// user_id puede ser NULL
// geo puede ser NULL
~~~
