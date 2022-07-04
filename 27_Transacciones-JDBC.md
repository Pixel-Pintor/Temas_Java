# Transacciones JDBC
Hay muchas situaciones en las que la logica comercial de su aplicacion requiere que no se ejecute un consulta a menos que se complete otra. Supongamos que solicita un automovil en linea. Para ello, una aplicacion de pedidos debe realizar los suguientes pasos: retirar dinero de su cuenta bancaria, depositar dinero en la cuenta del vendedor y realizar una orden de compra. Imagine que cada paso se ejecuta de forma independiente y que se produce un error del sistema despues de los dos primeros pasos, lo que significa que el dinero se retirara de su cuenta y se enviara al vendedor, pero nunca recibira su automovil.  
Para evitar tales situaciones, podemos tratar los tres pasos como una sola unidad que puede tener exito o fallar por completo. En ese caso, si ocurre un error del sistema o cualquier otra falla en cualquier etapa del pedido, su dinero no se transferira al vendedor y el vendedor no realizara una orden de compra. Esta unidad de varias sentencias SQL se denomina **transaccion**.  
Para que los cambios realizados por una transaccion sean permanentes en la base de datos, debemos confirmar la transaccion. Para deshabilitar el modo de confirmacion automatica, debemos dehabilitarlo explicitamente para la conexion actual llamando a `setAutoCommit(false)` del objeto `Connection`. Una vez que el modo de confirmacion automatica esta deshabilitado, llamar a los metodos de ejecucion de los objetos de declaracion no afectara a la base de datos hasta que **confirme la transaccion** llamando al metodo `commit()` del objeto `Connection`. Veamos el ejemplo y creemos una base de datos SQLite llamada `store.db` que tiene las tabla `order` y `invoice`.
~~~sql
CREATE TABLE "invoice" (
    id INTEGER PRIMARY KEY,
    shipping_address TEXT NOT NULL,
    total_cost INTEGER NOT NULL
);

CREATE TABLE "order" (
    id INTEGER PRIMARY KEY,
    invoice_id INTEGER NOT NULL,
    product_name TEXT NOT NULL,
    FOREIGN KEY (invoice_id) REFERENCES invoice (id)
);
~~~
Ahora podemos crear un objeto `Connection` y ejecutar `setAutoCommit(false)` para deshabilitar la confirmacion automatica de modo que crearemos tanto una factura como un pedido en caso de ejecucion exitosa o ninguno de de ellos en caso de falla:
~~~java
public class StoreDB {
    
    public static void main(String[] args) {
        String url = "jdbc:sqlite:path-to-store.db";

        SQLiteDataSource dataSource = new SQLiteDataSource();
        dataSource.setUrl(url);

        String insertInvoiceSQL = "INSERT INTO \"invoice\" " +
            "(id, shipping_address, total_cost) VALUES (?, ?, ?)";
        String insertOrderSQL = "INSERT INTO \"order\" " +
            "(id, invoice_id, product_name) VALUES (?, ?, ?)";
            
        try (Connection con = dataSource.getConnection()) {
            // desactiva el modo auto-commit
            con.setAutoCommit(false);

            try (PreparedStatement insertInvoice = con.prepareStatement(insertInvoiceSQL);
                PreparedStatement insertOrder = con.prepareStatement(insertOrderSQL)) {
                
                // inserta una factura
                int invoiceId = 1;
                insertInvoice.setInt(1, invoiceId);
                insertInvoice.setString(2, "Dearborn, Michigan");
                insertInvoice.setInt(3, 100500);
                insertInvoice.executeUpdate();

                // inserta una orden
                int orderId = 1;
                insertOrder.setInt(1, orderId);
                insertOrder.setInt(2, invoiceId);
                insertOrder.setString(2, "Ford Model A");
                insertOrder.executeUpdate();

                con.commit();
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
~~~
Cada vez que realiza un confirmacion, todas las declaraciones de transaccion se ejecutan y permanecen permanentemente en la base de datos. Significa que despues de llamar al metodo `con.commit`, se crea una factura y el pedido correspondiente. Si el metodo `setAutoCommit(true)` se llama durante una transaccion, la transaccion de confirmara.  
---
## Retroceder
Imagine que ocurre un error durante la ejecucion de la instruccion y se arroja un `SQLException`. Para evitar errores podemos deshacer todos los cambios realizados en la transaccion actual.  
En JDBC, puede revertir una transaccion invocando el metodo `roolback`. Se recomienda llamarlo en el bloque `catch` si su codigo arroja una excepcion durante la transaccion. Reescribimos nuestro codigo:
~~~java
catch (SQLException e) {
    if (con != null) {
        try {
            System.out.print("Transaction is being rolled");
            con.roolback();
        } catch (SQLException excep) {
            excep.printStackTrace();
        }
    }
}
~~~
No finalizar explicitamente la transaccion tambien pude hacer que la transaccion dure mas de los necesario.  
A veces tenemos que guardar algunos cambios y descartar otros. Para eso, tenemos que crear un **guardado** en el codigo. Todos los cambios introducidos hasta el punto de guardado se guardaran y podran confirmarse mas tarde.  
Para crear un punto de guardado en JDBC, podemos llamar al metodo `setSavepoint()` del objeto `Connection` que devuelve un objeto `Savepoint`. Podemos revertir la transaccion al punto de guardado para que todos los cambios realizados antes del punto de guardado se conserven y todos los cambios posteriores al punto de guardado se descarten. Para revertir la transaccion al punto de guardado especificado, podemos usar el metodo `rollback(Savepoint savepoint)` que acepta un objeto `Savepoint`. Para hacerlo mas concreto, consideremos el siguiente ejemplo:
~~~java
String insertInvoiceSQL = "INSERT INTO \"invoice\" " +
    "(id, shipping_address, total_cost) VALUES (?, ?, ?)";

String selectAddressSQL = "SELECT shipping_address " +
    "FROM \"invoice\" WHERE id = ?";

try (Connection con = dataSource.getConnection()) {

    // desactiva modo auto-commit
    con.setAutoCommit(false);

    try (PreparedStatement insertInvoice = con.prepareStatement(insertInvoiceSQL)) {
        // crea un savepoint
        Savepoint savepoint = con.setSavepoint();

        // inserta una factura
        int invoiceId = 1;
        insertInvoice.setInt(1, invoiceId);
        insertInvoice.setString(2, "Dearborn, Michigan");
        insertInvoice.setInt(3, 100500);
        insertInvoice.executeUpdate();

        PreparedStatement selectAddress = con.prepareStatement(selectAddressSQL);
        selectAddress.setInt(1, invoiceId);
        ResultSet resultSet = selectAddress.executeQuery();

        if (resultSet.getString(1).equals("Dearborn, Michigan")) {
            con.rollback(savepoint);
        }

        con.commit();
    }
} catch (SQLException e) {
    e.printStackTrace();
}
~~~
En el codigo anterior, creamos un objeto `Savepoint` llamado `savepoint` y revertimos los cambios de transaccion al punto de guardado si la factorua con `id` que es igual a 1 tiene una direccion de envio igual a `"Dearborn, Michigan"`.  
Es posibles liberar un punto de guardado, o en otras palabras, eliminarlo de la transaccion actual. Una forma de liberar un punto de guardado es usando el metodo `Connection.releaseSavepoint` que toma un objeto `Savepoint` como parametro. Cualquier punto de guardado que se haya creado en una transaccion se libera automaticamente y deja de ser valido cuando se confirma la transaccion o cuando se revierte toda la transaccion.
---
## Ejercicios
1. Dado el siguiente codigo, cuales paises se agregaran a la base de datos.
~~~java
String insertCountriesSQL = "INSERT INTO countries " +  "(id, name, capital) VALUES (?, ?, ?)";

try (Connection con = dataSource.getConnection()) {
    con.setAutoCommit(false);

    Savepoint savepoint0 = con.setSavepoint();

    try (PreparedStatement insertCountries = con.prepareStatement(insertCountriesSQL)) {
        insertCountries.setInt(1, 1);
        insertCountries.setString(2, "Spain");
        insertCountries.setString(3, "Madrid");
        insertCountries.executeUpdate();

        Savepoint savepoint1 = con.setSavepoint();

        insertCountries.setInt(1, 2);
        insertCountries.setString(2, "England");
        insertCountries.setString(3, "London");
        insertCountries.executeUpdate();

        Savepoint savepoint2 = con.setSavepoint();

        insertCountries.setInt(1, 3);
        insertCountries.setString(2, "Italy");
        insertCountries.setString(3, "Rome");
        insertCountries.executeUpdate();

        con.rollback(savepoint1);
        con.rollback(savepoint2);

        con.commit();
    } catch (SQLException e) {
        e.printStackTrace();
        con.rollback(savepoint0);
    }
} catch (SQLException e) {
    e.printStackTrace();
}

// No agrega ninguno, arroja una SQLException
~~~
2. Cuantos registros se agregaran en la tabla `retweets` despues de ejecutar el codigo.
~~~java
String insert = "INSERT INTO retweets VALUES (?, ?)";

try (Connection con = dataSource.getConnection()) {
    con.setAutoCommit(false);

    Savepoint savepoint0 = con.setSavepoint();

    try (PreparedStatement retweetsPreparedStatement = con.prepareStatement(insert)) {
        for (int i = 0; i < 10; i++) {

            Savepoint savepoint1 = con.setSavepoint();

            String text = "Congratulations to the Astronauts that left Earth today. Good choice";

            retweetsPreparedStatement.setInt(1, i);
            retweetsPreparedStatement.setString(2, text);
            retweetsPreparedStatement.executeUpdate();

            Savepoint savepoint2 = con.setSavepoint();

            if (i < 5) {
                con.rollback(savepoint1);
            } else if (i < 9) {
                con.rollback(savepoint2);
            } else {
                con.commit();
            }
        }
    } catch (SQLException e) {
        con.rollback(savepoint0);
    }
} catch (SQLException e) {
    e.printStackTrace();
}

// agrega 5 registros a la table retweets
~~~
3. Revisa el codigo y selecciona las declaraciones correctas.
~~~java
String insertDinosaur = "INSERT INTO dinosaurs (id, name, domain) VALUES (?, ?, ?)";
String insertPlant = "INSERT INTO plants (id, name, kingdom) VALUES (?, ?, ?)";

try (Connection con = dataSource.getConnection()) {
    con.setAutoCommit(false);

    try (PreparedStatement dinosaurPreparedStatement = con.prepareStatement(insertDinosaur);
         PreparedStatement plantPreparedStatement = con.prepareStatement(insertPlant)) {

        dinosaurPreparedStatement.setInt(1, 1);
        dinosaurPreparedStatement.setString(2, "Triceratops");
        dinosaurPreparedStatement.setString(3, "Eukaryota");
        dinosaurPreparedStatement.executeUpdate();

        con.commit();

        plantPreparedStatement.setInt(1, 1);
        plantPreparedStatement.setString(2, "Rose");
        plantPreparedStatement.setString(3, "Plantae");
        plantPreparedStatement.executeUpdate();
    }
} catch (SQLException e) {
    con.rollback();
}

// insertara un registro en la tabla dinosaurs
~~~
4. Cual es el proposito del metodo `Connection.rollback`.  
Se usa para deshacer todos los cambios en la base de datos realizados en la transaccion actual.
