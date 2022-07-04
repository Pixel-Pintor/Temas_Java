# Conexion a una base de datos con JDBC
Java DataBase Connectivity **(JDBC)** es una API de Java diseñada para acceder a datos almacenados en una base de datos relacionales. Al usar, JDBC puede establecer una conexion a una base de datos, ejecutar instrucciones SQL y manejar los resultados recibidos de la base de datos en respuesta a su consulta.  
La API de JDBC proporciona interfaces para conectar la aplicacion Java a las bases de datos relacionales, **controladores de JDBC** implementan esas interfaces para el sistema de gestion de bases de datos concretas **(DBMS)**. El controlador JDBC emite la conexion a la base de datos e implementa el protocolo para transferir consultas y sus resultados entre su aplicacion y la base de datos. Este enfoque nos permite usar la API de JDBC de la misma manera para diferentes DBMS mediante el uso de diferentes controladores de JDBC.  
Para establecer una conexion entre su aplicacion y la base de datos, necesita un objeto instanciado por una clase que implemente la interfaz `DataSource`. La interfaz `DataSource` es implementada por un proveedor de controladores y representa un DBMS particular. Debemos especificar la **URL base** que es una cadena que contiene informacion sobre la base de datos.  
Cada DBMS tiene sus propias reglas y sintaxis para una conexion por URL. La mayoria de los DBMS requieren una nombre de la base de datos, un nombre de host, un numero de puerto y las credenciales de usuario especificadas en la URL.  
Finalmente, debemos llamar al metodo `DataSource.getConnection`. Retorna una objeto `Connection` que representa una conexion con la base de datos. Una vez establecida la conexion, puede interactuar con la base de datos.  
Primero descargue e instale SQLite, el siguiente paso es agregar el controlador SQLite JDBC a la aplicacion. Para eso, podemos usar Gradle y agregar la siguiente dependencia a **build.gradle**:
~~~
dependencies {
    implementation 'org.xerial:sqlite-jdbc:3.30.1'
}
~~~
Ahora, creemos una clase donde podamos probar nuestra conexion JDBC. Para empezar, creamos una cadena que contiene la URL de la base de datos SQLite:
~~~java
public class CoolJDBC {
    public static void main(String[] args) {
        String url = "jdbc:sqlite:path-to-database";
    }
}
~~~
`path-to-database` puede representar una ruta absoluta o relativa a la base de datos. Por ejemplo, si instala SQLite en la carpeta C:\sqlite su ruta puede ser `"jdbc:sqlite:C:/sqlite/fruits.db"`. Al mismo tiempo, si un archivo de base de datos se encuentra dentro de la carpeta del proyecto, su URL puede ser `"jdbc:sqlite:fruits.db"`.  
Ahora estamos listos para establecer una conexion entre la aplicacion Java y SQLite. Para ello, crearemos un objeto `SQLiteDataSource` que es una implementacion de la interfaz `DataSource` proporcionada por el controlador SQLite JDBC. Luego, debemos establecer una URL de conexion de fuente de datos llamando al metodo `SQLiteDataSource.setUrl`. Finalmente declaramos el objeto `Connection` y le asignamos el valor de retorno `DataSource.getConnection`.
~~~java
public class CoolJDBC {
    public static void main(String[] args) {
        String url = "jdbc:sqlite:path-to-database";

        SQLiteDataSource dataSource = new SQLiteDataSource();
        dataSource.setUrl(url);

        try (Connection con = dataSource.getConnection()) {
            if (con.isValid(5)) {
                System.out.println("Connection is valid.")
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
~~~
El metodo `isValid()` comprueba si la conexion no se ha cerrado y sigue siendo valida mientras espera el numero de segundos especificado para validar la conexion `5`. Si la URL es correcta, la salida de la consola de nuestra aplicacion contendra la declaracion "Connection is valid".  
En el ejemplo anterior, establecimos una URL de fuente de datos y luego llamamos al metodo `DataSource.getConnection` sin ningun parametro. Sin embargo, puede tomar el nombre de usuario y la contraseña con parametros. Estas variables se utilizan con fines de autenticacion y es muy probable que en el escenario de la vida real deba especificarlas.
---
## Ejercicios
1. Cuales parametros pueden aceptar el metodo `DataSource.getConnection()`.
~~~java
DataSource.getConnection(String username, String password);
~~~
2. Construye un programa para establecer una conexion a la base de datos.
~~~java
SQLiteDataSource dataSource = new SQLiteDataSource();
dataSource.setUrl("jdbc:sqlite:monster.db");

try (Connection con = dataSource.getConnection("admin", "admin")) {
    if (con.isValid(3)) {
        System.out.println("Connection is valid!");
    }
} catch (SQLException e) {
    e.printStackTrace();
}
~~~
3. Especifica la URL si utiliza MySQL, el nombre del servidor es "localhost", el puerto es "3306" y la base de datos se llama "dinosaurs".
~~~java
jdbc:mysql://localhost:3306/dinosaurs
~~~
4. Especifique la URL si utiliza PostgreSQL, el nombre del servidor es localhost, el puerto es "5432" y la base de datos se llama "test".
~~~java
jdbc:postgresql://localhost:5432/dinosaurs
~~~
5. Cual metodo se usa para cerrar la conexion a la base de datos.
~~~java
Connection.close;
~~~
6. Cual interface de JDBC le permite conectar tu aplicacion Java con los drivers de JDBC.
~~~java
DataSource;
~~~
7. Complete el codigo para que funcione.
~~~java
Connection con = null;
String url = "jdbc:sqlite:corgi42.db";

SQLiteDataSource ds = new SQLiteDataSource();
ds.setUrl(url);

try {
    con = ds.getConnection();
} catch (SQLException e) {
    e.printStackTrace();
} finally {
    if (con != null) {
        try {
            con.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
~~~
8. Cual interface JDBC representa la conexion con una DBMS.
~~~java
Connection;
~~~
