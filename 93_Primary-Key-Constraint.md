# PRIMARY KEY Constraint

A veces necesitamos asegurarnos de que todas las filas de nuestras tabla sean unicas. Por ejemplo, queremos almancenar informacion sobre los participantes de la conferencia: su nombre, correo, fecha de nacimiento y ciudad; queremos asegurarnos de que nadie se registre dos veces. En este caso, deberiamos encontrar una combinacion de datos que sea unica para cada participante. Tal vez algunas personas tengan el mismo nombre, pero seguramente no compartiran el mismo correo electronico personal, por lo que podemos usar este campo como clave unica para evitar la creacion de duplicados. Esta clave unica se denomina comunmente como *clave primaria*.

## Restriccion de CLAVE PRIMARIA

La **PRIMARY KEY** restringe un conjunto de columnas con valores que pueden ayudar a identificar cualquier registro de tabla.  
Esta restriccion se puede especificar en el proceso de creacion de una tabla. Vamos a crear una table llamada *chefs*. Suponemos que todos los chefs tienen identificados individuales, por lo que haremos que nuestras *chef_id* sea la clave primaria.

~~~sql
CREATE TABLE chefs (
    chef_id INT PRIMARY KEY,
    first_name VARCHAR(20),
    last_name VARCHAR(20)
);
~~~

Dado que la clave principal tiene que identificar cada fila de la tabla, debe ser unica y no puede ser nula.  
Otra cosa importante es que una tabla puede tener una y solo una clave principal, pero se le permite incluir varias columnas en ella.  
Por ejemplo, considere la tabla *employees*. Supongemos que es posible tener dos empleados con identificadores identicos en diferentes departamentos, pero es imposible tener varios empleados con identificaciones identicas en un solo departamento. Entonces, podemos tener tuplas (42, 15, 'Ann Brown') y (43, 15, 'Bob Freud') en la tabla, pero no podemos agregar una tupla (42, 15, 'John Smith') a esa table pues ya hay una Ann Brown.  
En este caso podemos definir una *restriccion de PRIMARY KEY con nombre en varias columnas* cuando creamos las table *empleados*:

~~~sql
CREATE TABLE employees (
    department_id INT NOT NULL,
    employee_id INT NOT NULL,
    name varchar(50) NOT NULL,
    CONSTRAINT pk_employee PRIMARY KEY (department_id, employee_id)
);
~~~

## Agregar una PRIMARY KEY a una tabla existente

Si ya tenemos una table sin clave principal, podemos agregarla usando **ALTER TABLE*.  
Supongamos que tenemos una table con el nombre de *countries* que se creo de la siguiente manera.

~~~sql
CREATE TABLE countries (
    country_name VARCHAR(40) NOT NULL UNIQUE,
    population INT CHECK (population > 0),
    area REAL NOT NULL
);
~~~

Queremos que *country_name* sea nuestra clave principal. Para agregar una **PRIMARY KEY** sin nombre, usamos **ALTER TABLE ADD PRIMARY KEY**:

~~~sql
ALTER TABLE countries
ADD PRIMARY KEY (country_name);
~~~

La columna *country_name* ya es unica y no puede contener valores nulos, por lo que es seguro convertirla en una clave principal en la table *countries*.  
Tenga cuidado al agregar esta restriccion a una tabla que no este vacia: obtendremos un erro si ya tenemos valores duplicados o nulos en la clave principal potencial.  
Tambien podemos agregar una restriccion **PRIMARY KEY** con nombre a una tabla existente usando **ALTER TABLE ADD CONSTRAINT**. Definamos una restriccion **PRIMARY KEY** en multiples columnas para una tabla de *students*.  
La siguiente consulta agregara una clave principal *pk_students*. Esta clave principal tendra dos columnas: *name* y *birth_date*.

~~~sql
ALTER TABLE students
ADD CONSTRAINT pk_students PRIMARY KEY (name, birth_date);
~~~

## Borrar PRIMARY KEY

Otra cosa que debemos poder hacer es eliminar una clave primaria de una tabla.

~~~sql
ALTER TABLE students
DROP PRIMARY KEY;
~~~

Dado que una tabla solo puede tener una clave principal, no tenemos que especificar el nombre de la restriccion.

---

## Ejercicios

1. Cree una clave primaria sin nombre en la columna *client_id* de la tabla *clients*. La columna *client_id* es unica y no nula.

    ~~~sql
    ALTER TABLE clients
    ADD PRIMARY KEY (client_id);
    ~~~

2. Que tiene que tener una columna para que pueda ser una clave primaria?

    - **UNIQUE**
    - **NOT NULL**

3. Que restricciones se pueden reemplazar con la restriccion **PRIMARY KEY**?

    - **UNIQUE**
    - **NOT NULL**

4. Como puede agregar una clave primaria a una table existente?

    - **ADD CONSTRAINT ...**
    - **ADD PRIMARY KEY ...**

5. Crea una clave primaria llamada *pk_manager* en las columnas *first_name* y *last_name* de la table *managers*.

    ~~~sql
    ALTER TABLE managers
    ADD CONSTRAINT pk_manager PRIMARY KEY (first_name, last_name);
    ~~~

6. Como puede borrar una clave primaria de una tabla.

    - **DROP PRIMARY KEY**
