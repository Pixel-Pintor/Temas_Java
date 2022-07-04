# Proyecto basico on Gradle
Para comprobar que la instalacion se ha realizado correctamente use el comando `gradle -v`. Comencemos con una introduccion a los conceptos clave en Gradle:  
- Un **proyecto** puede representar algo que se va a construir (un archivo JAR o un archivo ZIP) o algo que hace (implementar la aplicacion). Cada compilacion de Gradle contiene uno o mas proyectos.
- Una **tarea** es una sola pieza de trabajo que realiza una compilacion. Esto puede incluir la compilacion de clases, la ejecucion de pruebas, la generacion de documentos, etc. Cada proyecto es esencialmente una coleccion de una o varias tareas.  
Inicialicemos un nuevo proyecto con Gradle usando una termina en su sistema operativo.  
1. Cree un nuevo directorio para almacenar archivos de su proyecto y accede a el.
~~~
mkdir gradle-demo
cd gradle-demo
~~~
2. Invocar `gradle init` para generar un proyecto simple. Elija `basic` como tipo de proyecto y `Groovy` como el script de compilacion DSL.
~~~
gradle init

> Task :init

BUILD SUCCESSFUL in 10s
2 actionable tasks: 2 executed
~~~
Ahora hay un proyecto simple con la estructura mas basica:
~~~
.
├── build.gradle
├── gradle
│   └── wrapper
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── gradlew
├── gradlew.bat
└── settings.gradle
~~~
Aqui hay informacion breve sobre todos los archivos generados:
- `build.gradle` es un archivo principal que especifica el proyecto de Gradle, incuidas sus tareas y bibliotecas externas. En proyecto reales, a menudo se actualiza con nueva informacion.
- Los archivos `gradle-wrapper.jar`, `gradle-wrapper.properties`, `gradlew` y `gradlew.bat` le permiten ejecutar Gradle sin su instalacion manual.
- El archivo `settings.gradle` especifica que proyectos incluir en su compilacion. Es opcional para una compilacion que tiene un solo proyecto, pero es obligatorio para una compilacion de varios proyectos.  
Construyamos nuestro proyecto usando `gradle build` desde la misma carpeta de `build.gradle`:
~~~
gradle build

> Task :buildEnvironment

------------------------------------------------------------
Root project
------------------------------------------------------------

classpath
No dependencies


BUILD SUCCESSFUL in 5s
1 actionable task: 1 executed
~~~
Hagamos que nuestra compilacion sea mas interesante agregando algunas propiedades y una tarea al archivo `build.gradle` utilizando Groovy DSL.
~~~
description = "A basic Gradle project"

task helloGradle {
    doLast {
        println 'Hello, Gradle!'
    }
}
~~~
Aqui, establecemos la propiedad `description` y definimos una tarea simple que imprime un mensaje. Ejecutamos la tarea usando `gradle -q helloGradle`:
~~~
gradle -q helloGradle

> Task :buildEnvironment

------------------------------------------------------------
Root project - A basic Gradle project
------------------------------------------------------------

...

> Task :helloGradle
Hello, Gradle!

BUILD SUCCESSFUL in 831ms
2 actionable tasks: 2 executed
~~~
El argumento `-q` simplifica la salida del comando. Para usar Kotlin como DSL debe especificarlo en el nombre del archivo `build.gradle.kt`.  
Si desea ver todas las posibles tareas de Gradle para realizar, simplemente ejecute `gradle tasks --all`. La lista incluira tambien nuestras tareas:
~~~
gradle tasks --all

> Task :tasks

------------------------------------------------------------
All tasks runnable from root project - A basic Gradle project
------------------------------------------------------------

Build Setup tasks
-----------------
init - Initializes a new Gradle build.
wrapper - Generates Gradle wrapper files.

Help tasks
----------
buildEnvironment - Displays all buildscript dependencies declared in root project 'gradle-demo'.
components - Displays the components produced by root project 'gradle-demo'. [incubating]
dependencies - Displays all dependencies declared in root project 'gradle-demo'.
dependencyInsight - Displays the insight into a specific dependency in root project 'gradle-demo'.
dependentComponents - Displays the dependent components of components in root project 'gradle-demo'. [incubating]
help - Displays a help message.
model - Displays the configuration model of root project 'gradle-demo'. [incubating]
projects - Displays the sub-projects of root project 'gradle-demo'.
properties - Displays the properties of root project 'gradle-demo'.
tasks - Displays the tasks runnable from root project 'gradle-demo'.

Other tasks
-----------
helloGradle


BUILD SUCCESSFUL in 2s
1 actionable task: 1 executed
~~~
En un proyecto real, la lista de tareas sera mucho mas grande porque, ademas de las tareas estandar, contendra muchas tareas de varios complementos.
---
# Creacion de aplicaciones con Gradle
En este tema, consideraremos la estructura basica del archivo `build.gradle` y a continuacion, cree y ejecute una pequeña aplicacion. Esto se puede usar para cualquier lenguaje de programacion basado en la JVM.
1. Instalar JDK.
~~~
sudo apt install openjdk-11-jdk
~~~
2. Crear el proyecto con `gradle init`.
~~~
gradle init

Starting a Gradle Daemon (subsequent builds will be faster)

Select type of project to generate:
  1: basic
  2: application
  3: library
  4: Gradle plugin
Enter selection (default: basic) [1..4] 2

Select implementation language:
  1: C++
  2: Groovy
  3: Java
  4: Kotlin
  5: Scala
  6: Swift
Enter selection (default: Java) [1..6] 3

Split functionality across multiple subprojects?:
  1: no - only one application project
  2: yes - application and library projects
Enter selection (default: no - only one application project) [1..2]

Select build script DSL:
  1: Groovy
  2: Kotlin
Enter selection (default: Groovy) [1..2]

Generate build using new APIs and behavior (some features may change in the next minor release)? (default: no) [yes, no]

Select test framework:
  1: JUnit 4
  2: TestNG
  3: Spock
  4: JUnit Jupiter
Enter selection (default: JUnit Jupiter) [1..4]

Project name (default: demo): org.hyperskill.gradleapp
Source package (default: org.hyperskill.gradleapp):

> Task :init
Get more help with your project: https://docs.gradle.org/7.4.2/samples/sample_building_java_applications.html

BUILD SUCCESSFUL in 2m 18s
2 actionable tasks: 2 executed
~~~
Una vez completada la inicializacion, la estructura del proyecto sera la siguiente:
~~~
.
├── app
│   ├── build.gradle
│   └── src
│       ├── main
│       │   ├── java
│       │   │   └── org
│       │   │       └── hyperskill
│       │   │           └── gradleapp
│       │   │               └── App.java
│       │   └── resources
│       └── test
│           ├── java
│           │   └── org
│           │       └── hyperskill
│           │           └── gradleapp
│           │               └── AppTest.java
│           └── resources
├── gradle
│   └── wrapper
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── gradlew
├── gradlew.bat
└── settings.gradle
~~~
El archivo mas importante `build.gradle`, que contiene tareas y bibliotecas externas, se encuentra dentro del directorio `app`. Esta carpeta existe porque hemos elegido `application` como el tipo de proyecto y la carpeta representa nuestra aplicacion.  
Tambien dentro de esta el directorio `src`. Contiene dos subdirectorios `main` y `test`. En nuestro caso, el paquete `org.hyperskill.gradleapp` tiene algo de codigo fuente de java en `App.java`.  
Si observas la lista de tareas disponibles para administrar el proyecto usando `gradle tasks --all`, veras que la lista es bastante larga. Existe el comando `run` que puede iniciar la aplicacion. Para hacerlo basta con el comando `gradle run`. Este comando compilara y ejecutara la aplicacion.
~~~
> Task :app:run
Hello World!

BUILD SUCCESSFUL in 623ms
2 actionable tasks: 1 executed, 1 up-to-date
~~~
Com puede ver, la aplicacion generada automaticamente ya puede mostrar una cadena de bienvenida. Si observa la estructura de la aplicacion, vera que tiene algunos archivos nuevos, incluidos los archivos de codigos de bytes `App.class` y `AppTest.class`.  
El archivo de compilacion `build.gradle` lo usamos para construir nuestra aplicacion con exito y ejecutarla usando Gradle. Este archivo especifica la estructura del proyecto y agrega algunas tareas y bibliotecas externas al proyecto.
---
## Estructura de build.gradle
La seccion `plugins` agrega algunos complementos para ampliar las capacidades del proyecto. Aqui, `id` es un identificador unico global, o nombre para complementos. Los complementos Core Gradle son especiales porque proporcionan nombre cortos, como `"java"` o `"application"`.  
Por lo general, no necesita escribir su programa desde cero: utiliza piezas de codigo ya escritas, ya sea suyas o de otros desarrolladores. Aqui es donde el sistema de dependencias es util. La seccion `repositories` declara las ubicaciones desde las que se descargan y agregan las dependencias del proyecto.
~~~
repositories {
    jcenter()
}
~~~
Hay muchos repositorios publics: **JCenter, Maven Central, Google** y otros.  
La seccion `dependencies` se utiliza para agregar bibliotecas externas al proyecto. Gradle los descargara automaticamente de los repositorios y los archivara en la aplicacion.  
El auto generador `build.gradle(kts)` tiene una seccion que configura al complemente `application` gracias al cual la aplicacion se ejecuta con el comando `gradle run`.
~~~
application {
    // Defines the main class for the application
    mainClassName = "org.hyperskill.gradleapp.App"
}
~~~
La propiedad `mainClassName` define una clase con el punto de entrada de la aplicacion. Nos permite ejecutarla aplicacion con el comando `gradle run`.
---
## Generando y ejecutando el archivo JAR
La forma clasica de ejecutar una aplicacion basada en JVM es usando `java -jar`. Este comando se puede utilizar sin Gradle, solo necesita tener un JAR. Entonces, construyamos el archivo JAR para nuestra aplicacion:
~~~
gradle jar

BUILD SUCCESSFUL in 7s
2 actionable tasks: 1 executed, 1 up-to-date
~~~
Ahora, el archivo JAR esta en el directorio `app/build/libs`. Si desea limpiar la carpeta del proyecto de todos los artefactos generador, simplemente ejecute `gradle clean`.  
Sin embargo, si ahora intenta ejecutar nuestra aplicacion generada unsando el enfoque clasico, habra un problema:
~~~
java -jar app/build/libs/app.jar
no main manifest attribute, in app/build/libs/app.jar
~~~
Lo que pasa es que la aplicacion no contiene el atributo `Main-Class` en el archivo `MANIFEST.MF`. Entonces, la JVM no conoce la ruta al punto de entrada de la aplicacion.  
Para solucionar esto, debemos agregar el atributo requerido al generar un archivo para la aplicacion. Simplemente agregue la siguiente declaracion al archivo `build.gradle(kts)`:
~~~
jar {
    manifest {
        attributes("Main-Class": "org.hyperskill.gradleapp.App")   // for Groovy DSL
        attributes("Main-Class" to "org.hyperskill.gradleapp.App") // for Kotlin DSL
    }
}
~~~
Este codigo agrega el atributo `Main-Class` a la propiedad manifest de la tarea jar. Entonces, ahora cuando ejecutamos `gradle jar` seguido por `java -jar app/build/libs/gradleapp.jar` todo deberia funcionar y vera la linea de salida `Hello world!`.  Lo bueno de esta forma de ejecutar aplicaciones es que `java -jar` se puede ejecutar sin Gradle, solo necesita tener un JAR.  
Si desea generar un paquete de la aplicacion con todas sus dependencias y un script para iniciar la aplicacion, utilice `gradle build`. Si esta todo bien, Gradle habra producido el archivo en dos formatos: `app/build/distributions/app.tar` y `app/build/distributions/app.zip`.
---
# Manejo de Dependencias
Es bastante dificil desarrollar una aplicacion real que no use bibliotecas porque ahorran mucho tiempo y brindan soluciones probadas por millones de personas en todo el mundo.  
En la terminologia de Gradle, todas las bibliotecas externas se demoniman **dependencias**. Por regla general, se empaquetan en archivos JAR. Gradle puede descargarlos automaticamente y agregarlos al proyecto. Ahorran mucho tiempo y resuelve posibles conflictos entre versiones de bibliotecas.  
Para agregar una biblioteca externa a una proyecto, debe seguir exactamente dos paso:
- **Defina un repositorio** donde buscar dependencias.
- **Defina una dependencias** que desee incluir en su proyecto.  
Los repositorios son solo lugares donde se almacenan las bibliotecas. Cualquier proyecto puede usar cero o mas repositorios. Gradle tiene cuatro alias que puede usar al agregar repositorios compatibles con Maven al proyecto.
- `mavenCentral()`: obtiene las dependencias del repositorio central de Maven.
- `jcenter()`: obtiene dependencias del repositorio JCenter Maven de Bintray.
- `mavenLocal()`: obtiene dependencias del repositorio local de Maven.
- `google()`: obtiene dependencias del repositorios de Google Maven.  
Para definir una repositorio debe agregar esto al `build.gradle`:
~~~
repositories {
    mavenCentral()
    jcenter()
}
~~~
Puede descargar los archivos JAR que necesita y colocarlos en algun directorio directo de su computadora, comunmente en la carpeta `libs` de su proyecto.
~~~
repositories {
    flatDir {
        dirs 'lib'
    }
}
~~~
Para agregar una nueva dependencias a su proyecto, primero debe identificarla con los siguientes atributos: `group`, `name` y `version`. Todos estos atributos son necesarios cuando utiliza repositorios compatibles con Maven.  
Los complementos `'java'` y `'kotlin'` agregan varias de estas configuraciones a su proyecto. Hay cuatro de ellos:
- `implementation` significa que la dependencias esta disponibles en tiempo de compilacion y no puede exponerse a las personas que usan su codigo compilado como una biblioteca externa de sus propios proyectos.
- `compileOnly` se usa para definir dependencias que solo deberian estar disponibles en tiempo de compilacion, pero no las necesita en tiempo de ejecucion.
- `runtimeOnly` se usa para definir dependencias que necesita solo durante el tiempo de ejecucion y no en el momento de la compilacion.
- `api` es similar a `implementation`, pero estara expuesto a los programadores que usan su codigo compilado como biblioteca de proyectos.
- `test` dado que las pruebas compilan y ejecutan por separado y no se incluyen en el JAR final, tienen su propio conjunto de dependencias.  
Cuando decidimos que dependencias queremos y nos decidimos por sus configuraciones, estamos listos para agregarlas a `build.gradle`, que es tan simple como agregar repositorios.
~~~
dependencies {
    // This dependency is used by the application.
    implementation group: 'com.google.guava', name: 'guava', version: '28.0-jre'

    // Use JUnit test framework only for testing
    testImplementation 'junit:junit:4.12'

    // It is only needed to compile the application
    compileOnly 'org.projectlombok:lombok:1.18.4'
}
~~~
Como habras notado, hay dos formas de declarar dependencias: una en la que declaramos explicitamente el grupo, el nombre y la version, y otra en la que simplemente los enumeramos separados por dos puntos. La sintaxis de Groovy es flexible y puede usar comillas simples o dobles para la cadena de dependencias, y opcionalmente, encerrarla entre parentesis. Todas las siguientes declaraciones son validas.
~~~
// 1
implementation("com.google.guava:guava:28.0-jre")

// 2
implementation "com.google.guava:guava:28.0-jre"

// 3
implementation 'com.google.guava:guava:28.0-jre'

// 4
def guava_version = "28.0-jre"
implementation "com.google.guava:guava:$guava_version"
~~~
Como ejemplo del uso de bibliotecas externas, echamos un vistazo a un programa que imprime mensajes de texto en color.
1. En la deccion **dependencias** de `build.gradle` incluimos la biblioteca JCDP:
~~~
implementation group: 'com.diogonunes', name: 'JCDP', version: '2.0.3.1'
~~~
2. Y luego importalo y usalo dentro del codigo fuente.
~~~java
package org.hyperskill.gradleapp;

import com.diogonunes.jcdp.color.ColoredPrinter;
import com.diogonunes.jcdp.color.api.Ansi;

public class App {
    public static void main(String[] args) {
        ColoredPrinter printer = new ColoredPrinter
                .Builder(1, false).build();

        printer.print("Hello, colorful world!",
                Ansi.Attribute.BOLD, Ansi.FColor.BLUE, Ansi.BColor.WHITE);
    }
}
~~~
