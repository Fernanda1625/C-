# *Proyecto Registro de Asistencias*
## Propósito 
El próposito principal de este proyecto es usar todas las herramientas aprendidas en el curso de Ingeniería de Software I, para crear un Registro de Asistencias para la Universidad Nacional de San Agustín, en el cual los alumnos y docentes podrán registrarlas y también los profesores podrán generar reportes, y así tener un mejor control de las inasistencias de los estudiantes. 
## Funcionalidades
Las principales funcionalidades son:
* Ingreso de alumnos con correo institucional
* Ingresar cursos al sistema y mostrarlos según el docente que lleva la asignatura
* Registrar asistencias por parte del docente y alumno
* Generar reportes de asistencias 

## Prácticas de Código Legibles
> Comentar y Documentar
Los comentarios que agregué en la definición de función se pueden observar en una vista previa siempre que use esa función, incluso desde otros archivos.

> Sangrado consistente
Manterner el estilo de sangrado de una manera coherente.

> Evitar Comentarios Obvios
Comentar el código es fantástico, sin embargo puede estar sobrecomentado o simplemente redundante.

> Agrupación de Código
Mantener las tareas dentro de bloques separados de código, con algunos espacios entre ellos.

>Principio DRY
DRY significa Don't Repeat Yourself - No se Repita a sí Mismo. También conocido como DIE: Duplication is Evil - Duplicar es Maligno.

> Evitando la Anidación Profunda
Demasiados niveles de anidamiento pueden hacer que el código sea más difícil de leer y seguir.

> Nombres Temporales Consistentes
Normalmente, las variables deben ser descriptivas y contener una o más palabras. Pero esto no necesariamente aplica a variables temporales. Estas pueden ser tan cortas como de un solo carácter. Es una buena práctica utilizar nombres coherentes para sus variables temporales que tengan el mismo tipo de rol. 

> Capitalizar Palabras Especiales de SQL
La interacción de bases de datos es una gran parte de la mayoría de las aplicaciones web. A pesar de que las palabras especiales y los nombres de funciones de SQL no distinguen entre mayúsculas y minúsculas, es una práctica común escribirlas en mayúsculas para distinguirlas de sus nombres de tabla y columna.

> Sintaxis Alterna Dentro de las Plantillas
Evitar un sinnúmero de llaves. Además, el código se ve y se siente similar a la forma en que HTML está estructurado y sangrado.

## Estilos de Programación
### MONOLITHIC
Es aquella en la que el software se estructura de forma que todos los aspectos funcionales del mismo quedan acoplados y sujetos en un mismo programa.
```javascript
function obtenerCursos() {
    var correo = document.getElementById('correo').value;
    var datos = 'correo='+correo;
    $.ajax({
        type: 'POST',
        url: "php/mostrarCursos.php",
        data: datos,
        success: function( response ) {
            if( response == "0") {
                alert("No se encontro persona");
            } else {
                var cursos = JSON.parse( response );
                cargarCursos( cursos );
            }
        }, error: function( response, status, error ) {
            alert("No encontrado");
        }
    });
    return false;
}
```
###  PIPELINE
Consiste en ir transformando un flujo de datos en un proceso comprendido por varias fases secuenciales, siendo la entrada de cada una la salida de la anterior.
```javascript
function revisarEmail( elemento ) {
    var data =elemento.value;
    var exp =/^[-\w.%+]{1,64}@(?:[A-Z0-9-]{1,63}\.){1,125}[A-Z]{2,63}$/i;
    if( exp.test(data)){
        elemento.className='input';
    } else {
        elemento.className='error';
        return false;
    }
    return true;
}

function validar() {
    var datosCorrectos=true;
    var error="";

    if(!(revisarTexto(document.getElementById("nombre")))){
        datosCorrectos=false;
        error="\nEl campo de Nombre esta vacio o incorrecto";
    }

    if(!(revisarEmail(document.getElementById("mail")))){
        datosCorrectos=false;
        error="\nEl campo de Email esta vacio o incorrecto";
    }

    if(!(revisarNumero(document.getElementById("telefono")))){
        datosCorrectos=false;
        error="\nEl campo telefono esta vacio o incorrecto";
    }

    var selec = n_form.grupo.selectedIndex;
    if( n_form.grupo.options[selec].value=="0") {
        datosCorrectos=false;
        error="\nFalta indicar el grupo del contacto";
    }

    if(!datosCorrectos){
        alert("Hay errores en el formulario" + error );
    } else {
        alert("Se ha agregado al usuario correctamente")
        location.href = "index.html";
        evt.preventDefault();
    }
}
```

## Principios SOLID
Entre los objetivos de tener en cuenta a la hora de escribir código encontramos:

* Crear un software eficaz: que cumpla con su cometido y que sea robusto y estable.
* Escribir un código limpio y flexible ante los cambios: que se pueda modificar fácilmente según necesidad, que sea reutilizable y mantenible.
* Permitir escalabilidad: que acepte ser ampliado con nuevas funcionalidades de manera ágil.

En  este sentido la aplicación de los principios SOLID está muy relacionada con la comprensión y el uso de patrones de diseño, que nos permitirán mantener una alta cohesión y, por tanto, un bajo acoplamiento de software.

### SRP - Single Responsibility Principle 
Según este principio “una clase debería tener una, y solo una, razón para cambiar”. El principio de Responsabilidad Única es el más importante y fundamental de SOLID, muy sencillo de explicar, pero el más difícil de seguir en la práctica.

El propio Bob resume cómo hacerlo: “Gather together the things that change for the same reasons. Separate those things that change for different reasons”, es decir: “Reúne las cosas que cambian por las mismas razones. Separa aquellas que cambian por razones diferentes”.
```javascript
class Historial {

    constructor( fecha, listaAsistencias ) {
        this.fecha = fecha;

        for( var i = 0; i < listaAsistencias.length(); ++i ) {
            temp = new Asistencia( listaAsistencias[i].fecha, listaAsistencias[i].control, listaAsistencias[i].estudiante, listaAsistencias[i].seccion );
            this.historial.push( temp );
        }
    }

    setFecha( fecha ) {
        this.fecha = fecha;
    }

    setHistorial( listaAsistencias ) {
        for( var i = 0; i < listaAsistencias.length(); ++i ) {
            temp = new Asistencia( listaAsistencias[i].fecha, listaAsistencias[i].control, listaAsistencias[i].estudiante, listaAsistencias[i].seccion );
            this.historial.push( temp );
        }
    }

    agregarAsistencia( asistencia ) {
        temp = new Asistencia( asistencia.fecha, asistencia.control, asistencia.estudiante, asistencia.seccion );
        this.historial.push( temp );
    }
};
```

### OCP - Open/Closed Principle
El segundo principio de SOLID lo formuló Bertrand Meyer en 1988 en su libro “Object Oriented Software Construction” y dice: “Deberías ser capaz de extender el comportamiento de una clase, sin modificarla”. En otras palabras: las clases que usas deberían estar abiertas para poder extenderse y cerradas para modificarse.

Es importante tener en cuenta el Open/Closed Principle (OCP) a la hora de desarrollar clases, librerías o frameworks.

```javascript
class checkValues{
    contructor(value,tipo){
        this.value = value; 
        this.tipo = tipo;
    }
    
    valueCorrecto(){
        switch (tipo) {
            case  'number':
                T = new isNumber(value);
                return T.comprobar();        
            case  'telephone':
                T = new isTelephone(value);
                return T.comprobar();        
            case 'email': 
                T = new isEmail(value);
                return T.comprobar();        
            case 'DNI':
                T = new isDNI(value);
                return T.comprobar();        
            default:
                return false;
          }
    }
}
```

## Conceptos DDD
El diseño guiado por el dominio, en inglés: domain-driven design (DDD), es un enfoque para el desarrollo de software con necesidades complejas mediante una profunda conexión entre la implementación y los conceptos del modelo y núcleo del negocio.
El DDD no es una tecnología ni una metodología, este provee una estructura de prácticas y terminologías para tomar decisiones de diseño que enfoquen y aceleren el manejo de dominios complejos en los proyectos de software.
### Entities
Las entidades son objetos del modelo que se caracterizan por tener identidad en el sistema, los atributos que contienen no son su principal característica. Representan conceptos con una identidad que se mantienen en el tiempo, y que con frecuencia también se mantienen bajo distintas representaciones de la entidad. Deben poder ser distinguidas de otros objetos aunque tengan los mismos atributos. Tienen que poder ser consideradas iguales a otros objetos aún cuando sus atributos difieren.

Por ejemplo, imaginemos un objeto Alumno con nombre y apellidos como atributos de una clase en un sistema donde dos objetos que representan a dos alumnos diferentes con los mismos nombres y apellidos deberían ser considerados diferentes. Como vemos, en este caso no podemos describir el objeto persona primariamente por sus atributos, si no que debemos de asignarle una identidad que se mantenga para cualquier representación de esa persona. Si nuestro sistema trabaja únicamente con personas de nacionalidad peruana podríamos considerar como identidad el DNI, en cambio si manejamos personas de cualquier nacionalidad quizás debamos de autogenerar este ID dentro de nuestro sistema. Cabe destacar que en nuestro sistema, Alumno es considerado una entidad.

La identidad tiene que ser declarada de tal manera que podemos rastrear la entidad de manera eficaz. Los atributos, responsabilidades y relaciones deben ser definidas en relación a la identidad que representa la entidad más que en los atributos que la componen.

Como podemos observar, el hecho de que necesitemos identificar y distinguir distintos objetos a lo largo de su ciclo de vida hace que la complejidad para manejarlos y diseñarlos sea bastante mayor que la de los que no la necesitan. Por este motivo debemos usar entidades únicamente para objetos que realmente lo requieran, lo cual tiene dos ventajas importantes. Por un lado, no incluiremos complejidad innecesaria en objetos que no requieran ser identificados, por otro lado al reducir el número de entidades en el sistema seremos capaces de identificarlas rápidamente.

### Value objects
Al contrario que las entidades los value objects representan conceptos que no tienen identidad. Simplemente describen características. Por lo tanto solo nos interesan sus atributos.

Los value object representan elementos del modelo que se describen por el QUÉ son, y no por QUIÉN o CUÁL son.

Pongamos por ejemplo, un objeto Curso representado por su composición ID, nombre y código. Si tuviéramos dos objetos representando el mismo curso podríamos usar cualquiera de ellos, ya que nos interesa qué curso es por sus atributos, no por cuál instancia estamos usando. Otros ejemplos de value objects podrían ser String o Integer, ya que no nos importa que ¨C¨ o que ¨3¨ estamos usando. Aunque estos ejemplos son simples los value objects no tienen porque serlo.

Esto conlleva una serie de diferencias a la hora de modelar value objects respecto a las entidades. Los value objects suelen ser modelados como inmutables y son menos complejos de diseñar, ya que podremos usarlos y descartarlos según nos interese, pues no tenemos que preocuparnos por la instancia que estemos utilizando (siempre y cuando sus atributos sean los correctos).

### Services
Los servicios representan operaciones, acciones o actividades que no pertenecen conceptualmente a ningún objeto de dominio concreto. Los servicios no tienen ni estado propio ni un significado más allá que la acción que los definen.

Al contrario que las entidades y los value objects, los servicios son definidos en términos de lo que pueden hacer por un cliente, y por tanto tienden a ser nombrados como verbos. Los verbos utilizados para nombrar a los servicios deben pertenecer al ubiquitous language, o ser introducidos en el en caso de que aún no lo sean. A la hora de implementarlos tanto sus parámetros como resultados deben ser objetos pertenecientes al dominio.

Un servicio debe de cumplir tres características principales:

* La operación que lo define está relacionada con un concepto de dominio, pero no es natural modelarlo como una entidad o un value object.
* Su interfaz se especifica usando otros elementos del modelo de dominio.
* La operación no tiene estado, por lo que cualquier cliente podría usar cualquier instancia del servicio sin tener en cuenta las operaciones que se han realizado con anterioridad en esa instancia.

Podemos dividir los servicios en tres tipos diferentes según su relación con el núcleo del dominio.

> Domain services
Son responsables del comportamiento más específico del dominio, es decir, realizan acciones que no dependen de la aplicación concreta que estemos desarrollando, sino que pertenecen a la parte más interna del dominio y que podrían tener sentido en otras aplicaciones pertenecientes al mismo dominio.Por ejemplo, crear un nuevo curso.

> Application services
Son responsables del flujo principal de la aplicación, es decir, son los casos de uso de nuestra aplicación. Son la parte visible al exterior del dominio de nuestro sistema, por lo que son el punto de entrada-salida para interactuar con la funcionalidad interna del dominio. Su función es coordinar entidades, value objects, domain services e infrastructure services para llevar a cabo una acción.Por ejemplo, añadir un nuevo alumno al registro para tomarle la asitencia.

> Infrastructure services
Declaran comportamiento que no pertenece realmente al dominio de la aplicación pero que debemos ser capaces de realizar como parte de este. Por ejemplo, enviar el registro de asistencia de cada alumno a su correo. 
