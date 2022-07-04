# Declaraciones preparadas de JDBC
Habras notado que la interfaz `Statement` no es tan flexible. Dado que debe especificar cada declaracion como una cadena, no hay forma de establecer parametros de declaracion, como una identificacion de registro o un valor de columna. Otra desventaja es que cada vez que la ejecuta, la base de datos compila la instruccion SQL sin almacenarla en cache. Conduce a un aumento de tiempo de ejecucion.  
La API de JDBC proporciona un tipo especial de declaracion que se denomina **declaracion preparada**, representada por la interfaz `java.sql.PreparedStatement`. De manera similar a las declaraciones, las declaraciones preparadas pueden crear, seleccionar, actualizar y eliminar datos de la base de datos. Sin embargo, las declaraciones preparadas tienen varias ventajas.  
~~~sql
# Creamos una tabla de base de datos
CREATE TABLE music.artists (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    origin TEXT,
    songs_number INTEGER
);

# Insertar usando Statement
try (Statement statement = con.createStatement()) {
    statement.executeUpdate("INSERT INTO artists (name, origin, songs_number) VALUES " +
            "('The Beatles', 'Liverpool, England', 213)");
}

# PreparedStatement proporciona una forma mas elegante de hacerlo
String insert = "INSERT INTO artists (name, origin, songs_number) VALUES (?, ?, ?)";

try (PreparedStatement preparedStatement = con.prepareStatement(insert)) {
    preparedStatement.setString(1, "The Beatles");
    preparedStatement.setString(2, "Liverpool, England");
    preparedStatement.setInt(3, 213);

    preparedStatement.executeUpdate();
}
~~~
Para crear una objeto `PreparedStatement`, usamos el metodo `prepareStatement(String sql)` del objeto `Connection`. Los simbolos de interrogacion `?` son marcadores de posicion para los valores que se pararan a la consulta. El primer maracador de posicion corresponde al nombre del artista, el segundo a su origen y el tercero al numero de canciones del artista.  
La interfaz `PreparedStatement` proporciona metodos de establecimiento para reemplazar un marcados de posicion de consulta SQL con el valor.
1. El nombre del metodo es `setXXX`, donde XXX es un tipo. Tenemos que usar el metodo adecuado para un tipo de dato SQL. Usamos `setString()` ya que el tipo de SQL del nombre del artista es `TEXT`.
2. Cada `PreparedStatement.setXXX` acepta al menos dos parametros. El primero indica el indice del parametro, mientras el segundo, el valor del parametro del tipo especifico. El `songs_number` corresponde al tercer marcador de posicion y tiene un tipo `INTEGER` en la base de datos, que corresponde con el tipo Java `int`.  
A vees es necesario establecer el valor del parametro en `NULL`. Para tales casos, `PreparedStatement` provee el metodo `setNull` que acepta el indice de parametros y un codigo de tipo SQL definido en Java. Por ejemplo, podemos establecer el origien del artista en `NULL` de la siguiente manera.
~~~sql
String updateOrigin = "UPDATE artists SET origin = ? WHERE id = ?";
try (PreparedStatement preparedStatement = con.preparedStatement(updateOrigin)) {
    preparedStatement.setNull(1, Types.VARCHAR);
    preparedStatement.setInt(2, 1);

    preparedStatement.executeUpdate();
}
~~~
Otra posibles situacion es cuando no conoce el tipo exacto del parametro en tiempo de ejecucion. En tal caso, puede utilizar el metodo `setObject()` que es mas general. Puede aceptar un indice de parametros y cualquier `Object` como argumento. Veamos un ejemplo:
~~~sql
String insert = "INSERT INTO artists (name, songs_number) VALUES (?, ?)";
try (PreparedStatement preparedStatement = con.preparedStatement(insert)) {
    preparedStatement.setObject(1, new StringBuilder("Madonna"));
    preparedStatement.setObject(2, new Long(88));

    preparedStatement.executeUpdate();
}
~~~
Los marcadores de posicione de declaraciones se pueden usar solo para valores, lo que significa que no puede reemplazar la palabra clave SQL con un marcador de posicion.
---
## Ejercicios
1. Que pasa si ejecuta el codigo.
~~~sql
String delete = "DELETE ? customers WHERE id = ?";

try (PreparedStatement preparedStatement = con.preparedStatement(delete)) {
    preparedStatement.setObject(1, "FROM");
    preparedStatement.setObject(2, "42");

    preparedStatement.executeUpdate();
}

# Genera una SQL Exception
~~~
2. Dada una tabla, complete las partes faltantes de la consulta.
~~~sql
String insert = "INSERT INTO IDEs (name, release, developer) VALUES (?, ?, ?)";

try (PreparedStatement preparedStatement = con.prepareStatement(insert)) {
    preparedStatement.setDate(2, Date.valueOf("2001-01-01"));
    preparedStatement.setString(1, "IntelliJ IDEA");
    preparedStatement.setString(3, "JetBrains");

    preparedStatement.executeUpdate();
}
~~~
3. Que pasa si ejecutas el siguiente codigo.
~~~sql
String insert = "INSERT INTO penguins (nickname, profession, rank) VALUES (?, ?, ?)";

try (PreparedStatement preparedStatement = con.prepareStatement(insert)) {
    preparedStatement.setObject(1, "Kowalski");
    preparedStatement.setObject(2, "Brains of the Penguins");
    preparedStatement.setObject(3, "Lieutenant); DROP TABLE penguins");

    preparedStatement.executeUpdate();
}

# Agregara "Lieutenant); DROP TABLE penguins" como el valor de "rank"
~~~
4. Escriba una consulta que actualice el nombre a partir de su id.
~~~sql
UPDATE penguins SET nickname = ? WHERE id= ?
~~~
