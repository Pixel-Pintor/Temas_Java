# Administrar Archivos

Como ya sabes, **java.io.File** representa una ruta abstracta a un archivo o directorio que puede que ni siquiera exista. Además de simplemente atravesar la jerarquía de archivos, también podemos administrar archivos y directorios creándolos, eliminándolos y renombrándolos. Consideremos varios métodos para hacerlo. Puede usar métodos considerados en diferentes sistemas operativos, pero en el siguiente ejemplo, trabajamos con un sistema operativo similar a UNIX.  

## Crear archivos

Para crear un archivo en el sistema de archivos, tendremos que hacer lo siguiente:

1. Crear una instancia de **java.io.File** con la ruta abstracta especificada.
2. Invocar el metodo **createNewFile** de esta instancia.

Despues de crear una instancia de **File** debemos invocar el metodo **createNewFile**. El metodo vuelve **true** si el archivo se creo con exito y **false** si ya existe. No borra el contenido de un archivo existente.

~~~java
File file = new File("/home/username/Documents/file.txt");
try {
    boolean createdNew = file.createNewFile();
    if (createdNew) {
        System.out.println("The file was successfully created.");
    } else {
        System.out.println("The file already exists.");
    }
} catch (IOException e) {
    System.out.println("Cannot create the file: " + file.getPath());
} 
~~~

Puede preguntar: "¿por qué el método devuelve falso en lugar de generar una excepción cuando el archivo ya existe"? La respuesta es que a veces no le importa al programa si el archivo se creó ahora mismo o ya existía.  

## Crear directorios

Para crear directorios, tambien debemos comenzar creando una instancia de **java.io.File**. Despues de eso, deberiamos llamar a uno de los dos metodos de esta instancia:

- **boolean mkdir** crear el directorio; retorna **true** solo si se creo el directorio, de lo contario **false**.
- **boolean mkdirs** crea el directorio que incluye todos los directorios principales necesarios que no existen; vuelve **true** solo si el directorio se creo junto con todos los directorios principales especificados.

Ambos metodos no arrojan **IOException**, a diferencia del metodo **createNewFile**.  
El siguiente ejemplo demuestra el metodo **mkdir**.

~~~java
File file = new File("/home/art/Documents/dir");

boolean createdNewDirectory = file.mkdir();
if (createdNewDirectory) {
    System.out.println("It was successfully created.");
} else {
    System.out.println("It was not created.");
}
~~~

Generalmente, el código funciona de la siguiente manera: si el directorio no existe, este código lo crea. Si el directorio existe, el código no lo creará. Si hay un directorio principal que no existe en la ruta, el directorio tampoco se creará. No se producen excepciones en ningún caso.  
He aqui otro ejemplo. que demuestra el metodo **mkdirs**. Crea el directorio de destino y todos los directorios principales si no existen.  

~~~java
File file = new File("hom/art/Documents/dir/dir/dir");

boolean createdNewDirectory = file.mkdirs();
if (createdNewDirectory) {
    System.out.println("It was successfully created.");
} else {
    System.out.println("It was not created.");
}
~~~

La variable booleana es **true**, si se creo el directorio de destino, independientemente de la existencia de los directorios principales.  

## Eliminacion de archivos y directorios

Ahora que sabemos cómo crear un directorio, veamos cómo deshacernos de uno. Hay un método llamado **delete** para eliminar un archivo o un directorio. Vuelve **true** si y solo si el archivo o directorio se elimina con éxito, de lo contrario, devuelve **false**. El método vuelve **false** si el archivo o directorio no existe. Es importante recordar que también regresa **false** si el directorio contiene subdirectorios o archivos. Significa que el método no eliminará una jerarquía, solo un archivo en particular o un directorio vacío.  

~~~java
File file = new File("/home/art/Documents/dir/dir/dir");
        
if (file.delete()) {
    System.out.println("It was successfully removed.");
} else {
    System.out.println("It was not removed.");
}
~~~

Para eliminar un directorio que no está vacío, primero debe eliminar todos los archivos y directorios anidados. Echa un vistazo al código de abajo. Borra recursivamente directorios con su contenido. Tenga en cuenta que el método asume que el directorio pasado **dir** existe. De lo contrario, **children == null** y **NullPointerException** será un arrojado.  

~~~java
public void deleteDirRecursively(File dir) {
    File[] children dir.listFiles();
    for (file child : children) {
        if (child.isDirectory()) {
            deleteDirRecursively(child);
        } else {
            child.delete();
        }
    }

    dir.delete();
}
~~~

El metodo **delete** nunca lanza un **IOException**.  
Tambien hay otro metodo para eliminar archivos. Se llama **deleteOnExit** y elimina un archivo o un directorio cuando su programa se detiene. Tenga en cuenta que una vez que se ha solicitado la eliminacion, no hay forma de cancelarla.  

## Cambiar el nombre y mover archivos y directorios

Ahora, echemos un vistazo al cambio de nombre de archivos y directorios.  
El metodo **renameTo** cambia el nombre del archivo editandolo en la ruta abstracta. Retorna **true** si y solo si el cambio de nombre fue exitoso, de lo contrario **false**.  

~~~java
File file = new File("/home/art/Documents/dir/filename.txt");
boolean renamed = file.renameTo(new File("/home/art/Documents/dir/newname.txt"));
~~~

Se puede usar el mismo metodo para mover el archivos o directorio de la ubicacion actual a otra:

~~~java
File file = new File("/home/art/Documents/dir/file.txt");
boolean renamed = file.renameTo(new File("/home/art/Documents/another/file.txt"));
~~~

Muchos aspectos del comportamiento de este método siguen dependiendo de la plataforma. Es posible que no pueda mover un archivo de un sistema de archivos a otro y que no tenga éxito si ya existe un archivo con el mismo destino. El valor de retorno siempre debe verificarse para asegurarse de que la operación se haya realizado correctamente.

~~~java
File file = new File("/home/art/Documents/dir/filename.txt");
File renamedFile = new File("/home/art/Documents/dir/newname.txt");

boolean renamed = file.renameTo(renamedFile);
if (renamed) {
    System.out.println("It was successfully renamed.");
} else {
    System.out.println("It was not renamed.");
}
~~~

El método **renameTo** lanza **NullPointerException** en caso de que un archivo de destino sea nulo.  
La mayoría de los métodos considerados devuelven falso en caso de que no tenga el permiso para realizar la operación correspondiente: renombrar, mover o eliminar archivos y directorios. Sin embargo, el método create lanza la excepción java.lang.IOException en casos similares.  

---

## Ejercicios

1. Sabemos que **dir.isDirectory() == true** y el metodo **dir.delete()** retorna **false**. Selecciona las posibles razones de esto.

    - **dir** contiene otros directorios vacios
    - El sistema operativos no permite la operacion de eliminacion del directorio **dir**
    - **dir** contiene otros directorios vacios
    - **dir** contiene un archivo

2. Completa el codigo para que funcione y cree una nueva caja de correo.

    ~~~java
    public void createMailbox(String username) throws IOException {
        String pathToUnread = username + File.separator + "unread";

        File unreadDirectory = new File(pathToUnread);
        unreadDirectory.mkdirs();

        File greetingMsg = new File(pathToUnread + File.separator "greeting.msg");
        greetingMsg.createNewFile();
        writeGreeting(greetingMsg, username);
    }
    ~~~

3. El metodo comprueba que el directorio **${username}/spam** existe y lo elimina con todos su contenido, despues de la finalizacion del metodo, el directorio spam debe eliminarse por completo.

    ~~~java
    public void cleanSpam(String username) {
        File spamDir = new File(username + File.separator + "spam");

        if (spamDir.exists()) {
            for (File msg : spamDir.listFiles()) {
                msg.delete();
            }
            spamDir.delete();
        }
    }
    ~~~

4. Implemente un metodo que mueva los mensajes al directorio de spam. El metodo comprueba **${username}/spam** exista y luego mueve el archio especificado a el.

    ~~~java
    public void moveToSpam(String username, File msg) {
        String pathToSpam = username * File.separator + "spam/";

        File spamDirectory = new File(pathToSpam);
        if (!spamDirectory.exists()) {
            spamDirectory.mkdir();
        }

        File spamMsg = new File(pathToSpam + File.separator + msg.renameTo(spamMsg);
    }
    ~~~

5. Cual es el resultado del codigo.

    ~~~java
    File file = new File("dir/file.txt");
    if (file.createNewFile()) {
        System.out.println("File is created");
    } else {
        System.out.println("File is not created");
    }

    // El archivo se crea con el mismo nombre, mensaje de salida: File is created
    ~~~
