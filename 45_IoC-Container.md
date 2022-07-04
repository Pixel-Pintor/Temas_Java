# Contenedor IoC
**IoC**, que significa **Inversion of Control**, es el mecanismo utilizado por Sprint para implementar la inyeccion de dependencias. Cuando creamos aplicaciones, a menudo necesitamos diferentes objetos para implementar varias funcionalidades. Algunos objetos necesitaran usar otros objetos como sus dependencias, lo que a su vez puede requerir otros objetos, y asi sucesivamente. Para simplificar este largo y complejo proceso, Spring utiliza la inyeccion de dependencias.  
Mediante el proceso de IoC, los objetos pueden definir las dependencias que necesitan para ejecutarse correctamente. Estas dependencias se definen a traves de argumentos de constructor, argumentos de metodos de fabrica o propiedades establecidas en la instancia del objeto.  

## Spring Container
Cuando necesitamos tener una aplicacion Spring lista para usar, necesitamos algunos componentes para ayudar a implementar la funcionalidad requerida. La siguiente imagen muestra una estructura tipica de una aplicacion Spring.
!(ibid)[img/04.svg]
Comencemos mirando el contenedor Spring, ya que es el nucleo del Spring. El contenedor Spring gestiona la aplicacion del ciclo de vida de principio a fin. Administra varios componentes creados para la aplicacion y maneja las inyecciones de dependencias requeridas. El contenedor Spring se puede configurar a traves de metadatos de varias maneras. Hay dos tipos de metadatos que se utilizan en Spring: **XML** y **anotaciones**. El enfoque XML implica definir datos relacionados con la clase en un archivo XML externo, que luego se puede cargar y usar en la aplicacion Spring.  
El enfoque basado en anotaciones implica agregar anotaciones a las clase de Java para proporcionar contexto y funcionalidad para Spring. Estas anotaciones comenzaran con el caracter `@`, y proporcionaran un valor especifico que deseamos agregar a nuestra clase. Estos valores nos permitiran construir objetos con las caracteristicas y configuraciones requeridas. Estos objetos se conocen como **clases Java POJO**.

## POJO
El termino **POJO** significa **Plain Old Java Object**. Un POJO es el tipo mas basico de objeto Java y no contiene vinculos con marcos. Son validos para cualquier aplicacion Java. La idea de un POJO es que es la unidad de codigo mas simple posible disponibles para una aplicacion Java. Pueden implementar propiedades, asi como getters y setters para estas propiedades, pero no pueden extender o implementar clases especificas del marco.  
La simplicidad de POJO los convierte en bloques de construccion ideales para cualquier componente de aplicacion que necesitemos implementar. Ademas de los POJO, Spring puede usar un tipo especial de POJO llamado **JavaBean**. Con JavaBeans, agregamos algunos requisitos mas: por ejemplo, las clases deben ser serializables. Ademas, requieren que esten disponibles campos privados y un constructor sin argumentos. Estas clases tambien se pueden personalizar y configurar utilizando metadatos de Spring. Para ello, podemos añadir varias anotaciones a las clases que creamos en Spring, por ejemplo `@Bean`. Se puede agregar una anotacion a un metodo de fabrica para definir que la clase que produce es un **Spring Bean**, lo que significa un objeto administrado por el contenedor IoC. Con estas anotaciones, es posible agregar configuraciones a clases preexistentes sin necesidad de crear archivos adicionales. Esto le permite aprovechar al maximo las funciones proporcionadas por Spring.  
Para resumir todo: **POJO** es un objeto Java que no depende del marco; **JavaBean** es un POJO con algunos requisitos y restricciones adicionales; y Spring Bean es un POJO o **JavaBean** creado y administrado por una instancia del contenedor **Spring IoC**.

## Contextos y fabrica de Beans
Cuando trabajamos con Spring IoC, hay dos componentes que debemos tener en cuenta. El primero es el `BeanFactory`, una interfaz que permite la configuracion y gestion de objetos. `BeanFactory` se pude usar para producir objetos administrados por contenedores conocidos como beans, que pueden organizar la columna vertebral de su aplicacion. Estos beans parecen objetos Java normales, pero pueden crearse durante el inicio de la aplicacion, registrarse e inyectarse en diferentes partes de la aplicacion mediante el contenedor.  
El segundo componente es el `ApplicationContext`, que es una subinterfaz del `BeanFactory`. El objetivo de `ApplicationContext` es facilitar la integracion con la funcionalidad de programacion orientada a aspector (**AOP**) de Spring. Esta funcionalidad incluye una variedad de componentes, que van desde el manejo de recursos de mensajes hasta contextos especificos de capa de aplicacion. Hay tres principales implementaciones de `ApplicationContext`.
- `FileSystemXmlApplicationContext`
- `ClassPathXmlApplicationContext`
- `WebApplicationContext`  


El `FileSystemXmlApplicationContext` cargara definiciones de beans desde un archivo XML que se proporciona como una ruta completa del sistema de archivos al constructor. Esto significa que los beans se inicializan en funcion del contenido de un archivo del sistema de archivos de la aplicacion que se esta ejecutando. Para `ClassPathXmlApplicationContext`, los beans todavia se cargan desde un archivo XML, sin embargo, el archivo se proporciona como la propiedad CLASSPATH en lugar de una ruta completa del sistema en el constructor. Finalmente, `WebApplicationContext` generalmente se usa para establecer la configuracion de una aplicacion web en Spring. Cuando usas `WebApplicationContext`, a menudo establecera la configuracion del servlet dentro una archivo `web.xml`. Dentro de este archivo, puede especificar configuraciones para cada servlet que utiliza la aplicacion.  
Con nuestro `ApplicationContext`, podemos configurar el contenedor Spring IoC, lo que nos permite crear una aplicacion lista para usar.
---
1. Que dos componentes Java se proporcionan al contenedor Spring para crear una aplicacion?.  
- Metadatos y clases Java POJO
2. En el `FileSystemXmlApplicationContext`, ¿Como se proporciona el contexto al constructor?
- Usando un archivo XML proporcionado como ruta completa del archivo del sistema
3. Para que un POJO sea un `JavaBean` valido debe tener.
- Campos privados disponibles y un constructor sin argumentos.
4. Por que necesitamos el contenedor Spring IoC?
- El contenedor proporciona la funcionalidad principal de Spring, como la gestion del ciclo de vida y las dependencias de la aplicacion.