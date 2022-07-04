# Tipos de datos SQL basicos
Por lo general, los valores de datos de la misma columna en una tabla tienen el mismo significado y tipo. Las bases de datos SQL normalmente requieren que cada columna de una tabla de base de datos tenga un nombre y un **tipo de datos**. La columna **tipo de datos** restringe el conjunto de valores que se pueden almacenar en la columna y define un conjunto de posibles operaciones sobre ellos.  
**INTEGER** representa algun rango de numeros naturales. Es bueno para contadores, identificadores numericos y cualquier valor comercial entero que pueda imaginar que se ajuste al rango de escala.  
**DECIMAL(precision, escala)** nos permite trabajar con numeros decimales, **escala** es el conteo de digitos a la derecha del punto decimal y **precision** es el recuento total de digitos en el numero. **731.12787000** en este numero la escala es 8 (12787000) y la precision es 11 (731.12787000).  
**FLOAT** es un tipo de dato que se utiliza para numeros de punto flotante. Podemos almacenar numeros muy grandes o muy pequeños. Ademas, se utiliza para calculos que requieren un procesamiento rapido. Tiene un parametro opcional **n** que especifica la precision y el tamaño del almacenamiento (de 1 a 53).  
SQL admite una familia de tipos de datos diseñados para representar datos de texto. **VARCHAR(n)** representa una cadena de simbolos de longitud variable no mayor que **n**. Si intenta introducir una cadena mas larga de lo declarado el sistema las truncara o generara un error.  
**BOOLEAN** representa valores logicos **TRUE** o **FALSE**.  
Como usuario de la base de datos, solo debe conocer los tipos de columnas de la table que utiliza para poder procesarlas correctamente. Consideremos un ejemplo de una consulta SQL que define una tabla `census` con 5 columnas.
~~~sql
CREATE TABLE census (
    id INTEGER,
    name VARCHAR(20),
    birth_place_latitude REAL,
    year_income DECIMAL(20, 3),
    is_parent BOOLEAN
);
~~~
Uno puede ver el patron:
~~~sql
CREATE TABLE table_name (
    column_name_1 column_type_1,
    ..., 
    column_name_n column_type_n
);
~~~
---
## Ejercicios
1. Crear una base de datos `car`.
~~~sql
CREATE TABLE car (
    id INTEGER,
    color VARCHAR(20),
    price DECIMAL(20, 2),
    electricity BOOLEAN,
    annual_petrol_use REAL
);
~~~
2. Define un tipo decimal con 3 digitos fraccionales y 5 digitos totales.
~~~sql
DECIMAL(5, 3)
~~~
3. Crear table que represente a los pacientes.
~~~sql
CREATE TABLE Patients (
    patient_id INTEGER,
    first_name VARCHAR(30),
    last_name VARCHAR(30),
    temperature DECIMAL(3, 1),
    is_discharged BOOLEAN
);
~~~
4. Crear la table para "Impression".
~~~sql
CREATE TABLE Impression (
    banner_id INTEGER,
    background_color VARCHAR(6),
    banner_width REAL,
    cost_per_click DECIMAL(6, 2),
    is_click BOOLEAN
);
~~~
