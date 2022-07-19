# Consistencia del Constraint

Cada columna de la tabla tiene un tipo de datos especifico, por lo que es imposible insertar texto en una columna con el tipo **INT** o un numero decimal en una columna con datos del tipo **BOOLEAN**. Las restricciones de tipo de datos nos ayudan a evitar muchos errores, pero a veces estas restricciones pueden ser muy especificas. Por ejemplo, todos los numeros de identificacion personal deben ser unicos o los clientes deben ser adultos. Estas restricciones especificas sobre los valores de las columnas se denominan *restricciones*.

## Ejemplo

Las restricciones mas comunes son **NOT NULL**, **UNIQUE**, **CHECK**, **DEFAULT**, **PRIMARY KEY** y **FOREIGN KEY**. En este tema hablaremos sobre las primeras cuatro restricciones de la lista.  
Tomemos como ejemplo una situacion sencilla de la vida. Supongamos que hemos creado una tabla *employees* con la siguiente consulta:

~~~sql
CREATE TABLE employees (
    personal_id INT,
    first_name VARCHAR(30),
    last_name VARCHAR(30),
    age INT
);
~~~

Modifiquemos esta table simple y agreguemos algunas restricciones.

## Restriccion NOT NULL

La restriccion **NOT NULL** no permitirar agregar *null* a una columna. En nuestra tabla *employees*, podemos hacer *age* no sea nula.

~~~sql
ALTER TABLE employees
MODIFY age INT NOT NULL;
~~~

Con esta consulta SQL, no sera posible agregar un nuevo empleado a la tabla sin indicar su edad. Si ya tenemos **NULL** en un valor de edad, obtendremos un error.  
Para eliminar la restriccion, simplemente use **ALTER TABLE MODIFY** otra vez sin el **NOT NULL**:

~~~sql
ALTER TABLE employees
MODIFY age INT;
~~~

Tambien puede utilizar esta restriccion en la declaracion **CREATE TABLE**. Simplemente agreguelo al final de la declaracion de tipo de columna:

~~~sql
CREATE TABLE employees (
    personal_id INT,
    first_name VARCHAR(30),
    last_name VARCHAR(30),
    age INT NOT NULL
);
~~~

## Restriccion UNIQUE

La restriccion **UNIQUE** prohibira agregar duplicados a la columna. Sabemos que el identificador de el empleado debe ser unico, por lo que podemos agregar esta restriccion a la columna *personal_id*.  

~~~sql
CREATE TABLE employees (
    personal_id INT UNIQUE,
    first_name VARCHAR(30),
    last_name VARCHAR(30),
    age INT
);
~~~

Podemos agregar **UNIQUE** a una tabla existente con una consulta simple usando **ALTER TABLE ADD UNIQUE**:

~~~sql
ALTER TABLE employees
ADD UNIQUE (personal_id);
~~~

Despues de la ejecucion de la consulta, la columna *personal_id* se vuelve unica. Si ya tenemos valores duplicados en esta columna, obtenedremos un error, asi que verifique su tabla antes de agregar esta restriccion.  
A veces tenemos que hacer que mas de una columna sea unica. En este caso, podemos definir una *restriccion* con nombre al final de la declaracion **CREATE TABLE**:

~~~sql
CREATE TABLE employees (
    personal_id INT,
    first_name VARCHAR(30),
    last_name VARCHAR(30),
    age INT,
    CONSTRAINT uq_id_last_name UNIQUE (personal_id, last_name)
);
~~~

Para descartar una restriccion con nombre, tambien puede usar **ALTER TABLE DROP INDEX**:

~~~sql
ALTER TABLE employees
DROP INDEX  uq_id_last_name;
~~~

## Restriccion CHECK

La restriccion **CHECK** nos permite agregar una expresion logica. Por ejemplo, podemos decir que todos nuestros empleados deben tener mas de dieciseis años:

~~~sql
CREATE TABLE employees (
    personal_id INT,
    first_name VARCHAR(30),
    last_name VARCHAR(30),
    age INT CHECK (age > 16)
);
~~~

Si desea agregar esta restriccion a una table existente, puede usar **ALTER TABLE ADD CHECK**. La sintaxis sera la misma que la de la restriccion **UNIQUE**.  
Para borrar un **CHECK** sin nombre use el siguiente codigo.

~~~sql
ALTER TABLE employees
DROP CHECK age;
~~~

La restriccion **CHECK** se puede nombrar y asignar a varias columnas. Similar el ejemplo anterior con **UNIQUE**, se puede especificar un nombre al crear la tabla. Tambien puede usar **ALTER TABLE ADD CONSTRAINT** y agregar un nombre a la restriccion **CHECK** para una o varias columnas a una table existente con la siguiente consulta.

~~~sql
ALTER TABLE employees
ADD CONSTRAINT chk_employee CHECK (age > 16 AND personal_id > 0);
~~~

Para eliminar una restriccion sin nombre, puede utilizar **ALTER TABLE DROP CHECK**.

## Restriccion DEFAULT

La restriccion **DEFAULT** define el valor inicial de una columna, el valor que aparecera si no insertas nada. Definas algunas valores por defecto en nuestra tabla:

~~~sql
CREATE TABLE employees (
    personal_id INT,
    first_name VARCHAR(30) DEFAULT 'John',
    last_name VARCHAR(30) DEFAULT 'Doe',
    age INT DEFAULT 17
);
~~~

Para agregar la restriccion **DEFAULT** a una table existente, use **ALTER TABLE ALTER SET DEFAULT**:

~~~sql
ALTER TABLE employees
ALTER first_name SET DEFAULT 'John';
~~~

Para eliminar una restriccion existente:

~~~sql
ALTER TABLE employees
ALTER first_name DROP DEFAULT;
~~~

## Combinar restricciones

Obviamente, una columna puede tener mas de una restriccion. Por ejemplo, es util combinar **NOT NULL** y **DEFAULT** para evitar errores al agregar nueva informacion.  
Si hemos elegido las restricciones que necesitamos, entonces podemos aplicarlas al crear una tabla usando la declaracion **CREATE**. Por ejemplo, volvamos a crear nuestra table *employees* usando todas estas restricciones utiles al mismo tiempo.

~~~sql
CREATE TABLE employees (
    personal_id INT NOT NULL UNIQUE,
    first_name VARCHAR(30) NOT NULL DEFAULT 'John',
    last_name VARCHAR(30) NOT NULL DEFAULT 'Doe',
    age INT DEFAULT 17,
    CHECK (age > 16)
);
~~~

---

## Ejercicios

1. Borre el constraint **CHECK** de *title* de la tabla *books*.

    ~~~sql
    ALTER TABLE books
    DROP CHECK title;
    ~~~

2. Corrige la consulta.

    ~~~sql
    ALTER TABLE candidates
    ADD CONSTRAINT chk_candidates CHECK (age >= 18 AND experience > 1);
    ~~~

3. Crear una tabla *users*, la columna *name* debe ser **NOT NULL**.

    ~~~sql
    CREATE TABLE users (
      name VARCHAR(40) NOT NULL,
        age INT,
        city VARCHAR(20)
    );
    ~~~

4. Seleccine las declaraciones correctas acerca de la consulta.

    ~~~sql
    CREATE TABLE cars (
        car_num INT NOT NULL UNIQUE,
        owner_surname VARCHAR(20) NOT NULL,
        country VARCHAR(30) DEFAULT 'Finland'
    );
    ~~~

    - La columna *country* tiene un valor por defecto
    - La columna *car_num* debe ser unica para cada fila

5. Agregue un valor por defecto a la columna *type* de la table *furniture*.

    ~~~sql
    ALTER TABLE furniture
    ALTER type SET DEFAULT 'chair';
    ~~~

6. Agregue un **UNIQUE** llamado *uq_names* a la columna *name* en la tabla *cats*.

    ~~~sql
    ALTER TABLE cats
    ADD CONSTRAINT uq_names UNIQUE (name);
    ~~~
