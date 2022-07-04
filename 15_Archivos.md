# Archivos
Puede considerar un archivo como una coleccion de datos que se almacenan en un disco u otro dispositivo, y que puede manipularse como una sola unidad cuando se le llama por su nombre. Los archivos se pueden organizar en directorios que actuan como carpetas para otros archivos y directorios.  
Hay una clase `File` en el paquete `java.io`. Un objeto de esta clase representa un archivo o directorio existente o inexistente. La clase se puede usar para manipular archivos y directorios: crear, eliminar, acceder a propiedades y mas.  
La forma mas sencilla de crear un objeto de archivo es pasar una ruta de cadena a su constructor. Si su sistema operativo es windows, no olvide usar el caracter de escape `\`. Vamos a crear dos objetos de la clase `File` para diferentes plataformas.
~~~java
File fileOnUnix = new File("/home/username/Documents");    // a directory on a UNIX-like system
File fileOnWin = new File("D:\\Materials\\java-materials.pdf"); // a file on Windows
~~~
El codigo funcionara incluso si un archivo o directorio no existe en sus sistema. No crear un nuevo archivo o directorio. Simplemente representa un archivo o directorios "virtual" que ya existe o puede crearse en el futuro. Los objetos `File` son inmutables; es decir, una vez creado, el nombre de ruta abstractor representado por un objeto nunca cambiara.  
Una **ruta absoluta** comienza con el elemento raiz del sistema de archivos. Tiene la informacion completa sobre la ubicacion del archivo, incluido el tipo de sistema operativo.  
Una **ruta relativa** es una ruta que no incluye el elemento raiz del sistema de archivos. Esto siempre comienza desde su directorio de trabajo. Este directorio esta representado por un punto `.`. Una ruta relativa no esta completa y debe combinarse con la ruta del directorio actual para llegar al archivo solicitado.
~~~java
File fileOnUnix = new File("./images/picture.jpg");
File fileOnWin = new File("./images/picture.jpg");
~~~
Para acceder al directorio principal, simplemente escriba doble punto `..`. Asi que `../picture.jpg` es un archivo ubicado en el directorio principal del directorio de trabajo. La ruta relativa `images/../images/picture.jpg` significa el directorio padre de `images`, entonces la carpeta `images` de nuevo. Y `picture.jpg` es el archivo dentro de la carpeta `images`.  
---
## Metodos Basicos
Una instancia de `File` tiene una lista de metodos:
- `String getPath()` retorna la ruta de este archivo o directorio.
- `String getName()` retorna el nombre de este archivo o directorio (solo el apellido de la ruta).
- `boolean isDirectory()` retorna `true` si es un directorio y existe, de lo contrario `false`.
- `boolean isFile()` retorna `true` si es una archivo que existe, de lo contrario `false`.
- `boolean exists()` retorna `true` si este archivo o directorio realmente existe en el sistema de archivos, de lo contrario `false`.
- `String getParent()` retorna la ruta de la cadena al directorio principal que contiene este archivo o directorio.  
Vamos a crear una instancia de un archivo existente e imprimir informacion sobre el.
~~~java
File file = new File("/home/username/Documents/javamaterials.pdf");

System.out.println("File name: " + file.getName());
System.out.println("File path: " + file.getPath());
System.out.println("Is file: " + file.isFile());
System.out.println("Is directory: " + file.isDirectory());
System.out.println("Exists: " + file.exists());
System.out.println("Parent path: " + file.getParent());
~~~
Como esperabamos, el codigo imprime lo siguiente:
~~~java
File name: javamaterials.pdf
File path: /home/username/Documents/javamaterials.pdf
Is file: true
Is directory: false
Exists: true
Parent path: /home/username/Documents
~~~
Supongamos que ahora tenemos una instancia que representa un archivo que no existe e imprime los detalles al respecto:
~~~java
File name: javamaterials1.pdf
File path: /home/art/Documents/javamaterials1.pdf
Is file: false
Is directory: false
Exists: false
Parent path: /home/art/Documents
~~~
El archivo no existe, por lo que la aplicacion no conoce su tipo.  
Tambien existen los metodos, `canRead()`, `canWrite()` y `canExecute()` para probar si la aplicacion puede **leer/modificar/ejecutar** el archivo indicado por la ruta. Se recomienda utilizar estos metodos, de lo contrario, puede encontrar errores de acceso a archivos cuando su usuario no tiene suficientes permisos para realizar una operacion con un archivo.
---
## Ejercicios
1. Esta es una forma de recorrer todos los subdirectorios y recopilar todos los archivos en una lista. El metodo `listFiles` devuelve los archivos y directorios anidados de un directorio como una matriz.
~~~java
public List<File> getAllFiles(File rootDir) {
    File[] children = rootDir.listFiles();
    if (children == null || children.length == 0) {
        return Collections.emptyList();
    }

    List<File> files = new ArrayList<>();
    for (File child : children) {
        if (child.isDirectory()) {
            files.addAll(getAllFiles(child));
        } else {
            files.add(child);
        }
    }
    
    return files;
}
~~~
2. Caracteres iniciales.
~~~java
D: -> Inicia una ruta absoluta a un archivo en Windows.
/ -> Inicia una ruta absoluta a una archivo en Unix.
. -> Comienza una ruta relativa a un archivo desde este directorio.
.. -> Comienza una ruta relativa a un archivo desde el directorio principal.
~~~
3. Implementar el metodo `areSibling()` que comprueba si los archivos tienen el mismo padre.
~~~java
class Siblings {

    public static boolean areSiblings(File f1, File f2) {
        return Objects.equals(f1.getParent(), f2.getParent());
    }
}
~~~
4. El metodo `getChildren()` toma una directorio como argumento pasado y devuelve una lista de archivos y directorios anidados. Si el directorio esta vacio, devuelve una lista vacia. Un archivo se considera antiguo si se modifico por ultima vez antes de la pasada `thresholdDate`.
~~~java
public void deleteOldFiles(File rootDir, long thresholdDate) {
    Deque<File>  files = new ArrayDeque<>(getChildren(rootDir));

    while (files.size() != 0) {
        File file = files.pop();

        if (file.isFile()) {
            if (file.lastModifies() < thresholdDate) {
                file.delete();
            }
        } else {
            files.addAll(getChildren(file));
        }
    }
}

private List<File> getChildren(File dir) {
    File[] children = dir.listFiles();

    return children == null || children.length == 0 ?
            Collections.emptyList();
            Arrays.asList(children);
}
~~~
