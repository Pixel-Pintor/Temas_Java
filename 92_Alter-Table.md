# Alter Table

A veces necesitamos modificar nuestra tabla: agregar una nueva columna, eliminar la existente o cambiar el tipo de columna. Podemos crear una nueva tabla con todos estos cambios y eliminar la anterior, pero es facil perder datos durante este proceso. Afortunadamente, SQL proporciona una instruccion que puede modificar las tablas existentes, **ALTER TABLE**. Se puede usar para crear, eliminar o cambiar las columnas.

## Agregar una nueva columna

Imagine que hay una nueva empresa internacional que tiene empleados de todo el mundo y usted esta tratando de ayudarlos a mantener sus registros. Hay una tabla empleados que contiene el primer nombre, apellido y ciudad natal.  
Por ahora, la tabla no almacena ninguna informacion de contacto, pero seria genial si realmente tuvieramos los correos electronicos de contacto de todos. No tenemos una columna para esta informacion, por lo que debemos agregar una.  
Puede agregar una nueva columna a su tabla con una simple consulta **ALTER TABLE** con **ADD COLUMN**.  
La siguiente consulta agregara la columna *employee_email* nuestra tabla *employee*:

~~~sql
ALTER TABLE employees
ADD COLUMN employee_email VARCHAR(10);
~~~

Como puede ver, especificamos el tipo de columna en nuestra consulta de la misma manera que lo hacemos al crear una tabla con nuevas columnas. En nuestro caso, la columna *employee_email* tendra el tipo de datos **VARCHAR(10)**.  
Despues de la ejecucion de la consulta, nuestra tabla tiene una columna vacia para los correos electronicos de contacto.

## Cambiar el tipo de datos

Creamos una columna *employee_email* con el tipo de datos **VARCHAR(10)**, pero algunas personas tiene correos electronicos muy largos que no se ajustan al limite de 10 caracteres, por lo que no podremos agregarlos a menos que cambiemos el tipo de datos de la columna.  
Para cambiar el tipo de datos, realice una consulta con **ALTER TABLE** y **MODIFY COLUMN**:

~~~sql
ALTER TABLE employees
MODIFY COLUMN employee_email VARCHAR(45);
~~~

Como resultado de la ejecucion de la consulta, la columna *employee_email* tendra el tipo **VARCHAR(45)**. Ahora podemos agregar correos electronicos largos a nuestra tabla.  
Si decide cambiar el tipo de columna, debe estar vacia antes de cambiarlo o el nuevo tipo de datos debe ser compatible con el anterior. De lo contrario, obtendra un error.

## Descartar una columna existente

En nuestra table *employees*, tenemos informacion sobre la ciudad natal. En aras de la concision, deshagamonos de esta columna.  
Para eliminar esta columnas se ejecuta la siguiente consulta:

~~~sql
ALTER TABLE employees
DROP COLUMN native_city;
~~~

## Cambiar el nombre de la columna

Hablando de concision, podemos cambiar el nombre de la columna *employee_email* ya que esta claro que los correos electronicos almacenados pertenecen a los empleados. Cambiemos el nombre de la columna a *email*.

~~~sql
ALTER TABLE employees
CHANGE employee_email email VARCHAR(45);
~~~

Ahora nuestra columna con correos electronicos tiene un nombre mas corto.  
Como puede ver, hay una declaracion de tipo en la consulta: si desea cambiar el nombre y el tipo, puede hacerlo al mismo tiempo. De lo contrario, simplemente agregue el tipo de columna anterior a la consulta como en el ejemplo anterior.

## Resumen

Para agregar una nueva columna a la tabla existente, use esta sencilla plantilla de consulta:

~~~sql
ALTER TABLE table_name
ADD COLUMN column_name DATATYPE;
~~~

La siguiente plantilla de consulta puede eliminar una columna de la tabla:

~~~sql
ALTER TABLE table_name
DROP COLUMN column_name;
~~~

Para cambiar el tipo de columna, puede usar esta plantilla:

~~~sql
ALTER TABLE table_name
MODIFY COLUMN column_name NEWDATATYPE;
~~~

Para cambiar el nombre de la columna (y, posiblemente, el tipo de datos), use la siguiente plantilla:

~~~sql
ALTER TABLE table_name
CHANGE old_column_name new_column_name NEWDATATYPE;
~~~

---

## Ejercicios

1. Encuentre todas las afirmaciones correctas sobre ALTER TABLE.

    - Puede usar ALTER TABLE para agregar una columna
    - Puede usar ALTER TABLE para cambiar el tipo de datos de la columna
    - No puede usar ALTER TABLE par eliminar una table

2. Cambie el tipo de datos de la columna *personal_number* a **INT*.

    ~~~sql
    ALTER TABLE students
    MODIFY COLUMN personal_number INT;
    ~~~

3. Borre la columna *cat* de la tabla *teachers*.

    ~~~sql
    ALTER TABLE teachers
    DROP COLUMN cat;
    ~~~

4. Seleccione la solicitud valida para arreglar la columna *nme* a *name VARCHAR(20)*.

    ~~~sql
    ALTER TABLE customers
    CHANGE nme name VARCHAR(20);
    ~~~

5. Agrega la columna *marital_status (VARCHAR(20))* a la tabla *persons*.

    ~~~sql
    ALTER TABLE persons
    ADD COLUMN marital_status VARCHAR(20);
    ~~~

6. Corrige la consulta.

    ~~~sql
    ALTER TABLE employees
    MODIFY COLUMN years_experience INT;
    ~~~
