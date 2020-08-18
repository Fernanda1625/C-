# *Proyecto Registro de Asistencias*
## Prop�sito 
El pr�posito principal de este proyecto es usar todas las herramientas aprendidas en el curso de Ingenier�a de Software I, para crear un Registro de Asistencias para la Universidad Nacional de San Agust�n, en el cual los alumnos y docentes podr�n registrarlas y tambi�n los profesores podr�n generar reportes, y as� tener un mejor control de las inasistencias de los estudiantes. 
## Funcionalidades
Las principales funcionalidades son:
* Ingreso de alumnos con correo institucional
* Ingresar cursos al sistema y mostrarlos seg�n el docente que lleva la asignatura
* Registrar asistencias por parte del docente y alumno
* Generar reportes de asistencias 

## Pr�cticas de C�digo Legibles
> Comentar y Documentar
Los comentarios que agregu� en la definici�n de funci�n se pueden observar en una vista previa siempre que use esa funci�n, incluso desde otros archivos.

> Sangrado consistente
Manterner el estilo de sangrado de una manera coherente.

> Evitar Comentarios Obvios
Comentar el c�digo es fant�stico, sin embargo puede estar sobrecomentado o simplemente redundante.

> Agrupaci�n de C�digo
Mantener las tareas dentro de bloques separados de c�digo, con algunos espacios entre ellos.

>Principio DRY
DRY significa Don't Repeat Yourself - No se Repita a s� Mismo. Tambi�n conocido como DIE: Duplication is Evil - Duplicar es Maligno.

> Evitando la Anidaci�n Profunda
Demasiados niveles de anidamiento pueden hacer que el c�digo sea m�s dif�cil de leer y seguir.

> Nombres Temporales Consistentes
Normalmente, las variables deben ser descriptivas y contener una o m�s palabras. Pero esto no necesariamente aplica a variables temporales. Estas pueden ser tan cortas como de un solo car�cter. Es una buena pr�ctica utilizar nombres coherentes para sus variables temporales que tengan el mismo tipo de rol. 

> Capitalizar Palabras Especiales de SQL
La interacci�n de bases de datos es una gran parte de la mayor�a de las aplicaciones web. A pesar de que las palabras especiales y los nombres de funciones de SQL no distinguen entre may�sculas y min�sculas, es una pr�ctica com�n escribirlas en may�sculas para distinguirlas de sus nombres de tabla y columna.

> Sintaxis Alterna Dentro de las Plantillas
Evitar un sinn�mero de llaves. Adem�s, el c�digo se ve y se siente similar a la forma en que HTML est� estructurado y sangrado.

## Estilos de Programaci�n
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
Entre los objetivos de tener en cuenta a la hora de escribir c�digo encontramos:

* Crear un software eficaz: que cumpla con su cometido y que sea robusto y estable.
* Escribir un c�digo limpio y flexible ante los cambios: que se pueda modificar f�cilmente seg�n necesidad, que sea reutilizable y mantenible.
* Permitir escalabilidad: que acepte ser ampliado con nuevas funcionalidades de manera �gil.

En  este sentido la aplicaci�n de los principios SOLID est� muy relacionada con la comprensi�n y el uso de patrones de dise�o, que nos permitir�n mantener una alta cohesi�n y, por tanto, un bajo acoplamiento de software.

### SRP - Single Responsibility Principle 
Seg�n este principio �una clase deber�a tener una, y solo una, raz�n para cambiar�. El principio de Responsabilidad �nica es el m�s importante y fundamental de SOLID, muy sencillo de explicar, pero el m�s dif�cil de seguir en la pr�ctica.

El propio Bob resume c�mo hacerlo: �Gather together the things that change for the same reasons. Separate those things that change for different reasons�, es decir: �Re�ne las cosas que cambian por las mismas razones. Separa aquellas que cambian por razones diferentes�.
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
El segundo principio de SOLID lo formul� Bertrand Meyer en 1988 en su libro �Object Oriented Software Construction� y dice: �Deber�as ser capaz de extender el comportamiento de una clase, sin modificarla�. En otras palabras: las clases que usas deber�an estar abiertas para poder extenderse y cerradas para modificarse.

Es importante tener en cuenta el Open/Closed Principle (OCP) a la hora de desarrollar clases, librer�as o frameworks.

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
El dise�o guiado por el dominio, en ingl�s: domain-driven design (DDD), es un enfoque para el desarrollo de software con necesidades complejas mediante una profunda conexi�n entre la implementaci�n y los conceptos del modelo y n�cleo del negocio.
El DDD no es una tecnolog�a ni una metodolog�a, este provee una estructura de pr�cticas y terminolog�as para tomar decisiones de dise�o que enfoquen y aceleren el manejo de dominios complejos en los proyectos de software.
### Entities
Las entidades son objetos del modelo que se caracterizan por tener identidad en el sistema, los atributos que contienen no son su principal caracter�stica. Representan conceptos con una identidad que se mantienen en el tiempo, y que con frecuencia tambi�n se mantienen bajo distintas representaciones de la entidad. Deben poder ser distinguidas de otros objetos aunque tengan los mismos atributos. Tienen que poder ser consideradas iguales a otros objetos a�n cuando sus atributos difieren.

Por ejemplo, imaginemos un objeto Alumno con nombre y apellidos como atributos de una clase en un sistema donde dos objetos que representan a dos alumnos diferentes con los mismos nombres y apellidos deber�an ser considerados diferentes. Como vemos, en este caso no podemos describir el objeto persona primariamente por sus atributos, si no que debemos de asignarle una identidad que se mantenga para cualquier representaci�n de esa persona. Si nuestro sistema trabaja �nicamente con personas de nacionalidad peruana podr�amos considerar como identidad el DNI, en cambio si manejamos personas de cualquier nacionalidad quiz�s debamos de autogenerar este ID dentro de nuestro sistema. Cabe destacar que en nuestro sistema, Alumno es considerado una entidad.

La identidad tiene que ser declarada de tal manera que podemos rastrear la entidad de manera eficaz. Los atributos, responsabilidades y relaciones deben ser definidas en relaci�n a la identidad que representa la entidad m�s que en los atributos que la componen.

Como podemos observar, el hecho de que necesitemos identificar y distinguir distintos objetos a lo largo de su ciclo de vida hace que la complejidad para manejarlos y dise�arlos sea bastante mayor que la de los que no la necesitan. Por este motivo debemos usar entidades �nicamente para objetos que realmente lo requieran, lo cual tiene dos ventajas importantes. Por un lado, no incluiremos complejidad innecesaria en objetos que no requieran ser identificados, por otro lado al reducir el n�mero de entidades en el sistema seremos capaces de identificarlas r�pidamente.

### Value objects
Al contrario que las entidades los value objects representan conceptos que no tienen identidad. Simplemente describen caracter�sticas. Por lo tanto solo nos interesan sus atributos.

Los value object representan elementos del modelo que se describen por el QU� son, y no por QUI�N o CU�L son.

Pongamos por ejemplo, un objeto Curso representado por su composici�n ID, nombre y c�digo. Si tuvi�ramos dos objetos representando el mismo curso podr�amos usar cualquiera de ellos, ya que nos interesa qu� curso es por sus atributos, no por cu�l instancia estamos usando. Otros ejemplos de value objects podr�an ser String o Integer, ya que no nos importa que �C� o que �3� estamos usando. Aunque estos ejemplos son simples los value objects no tienen porque serlo.

Esto conlleva una serie de diferencias a la hora de modelar value objects respecto a las entidades. Los value objects suelen ser modelados como inmutables y son menos complejos de dise�ar, ya que podremos usarlos y descartarlos seg�n nos interese, pues no tenemos que preocuparnos por la instancia que estemos utilizando (siempre y cuando sus atributos sean los correctos).

### Services
Los servicios representan operaciones, acciones o actividades que no pertenecen conceptualmente a ning�n objeto de dominio concreto. Los servicios no tienen ni estado propio ni un significado m�s all� que la acci�n que los definen.

Al contrario que las entidades y los value objects, los servicios son definidos en t�rminos de lo que pueden hacer por un cliente, y por tanto tienden a ser nombrados como verbos. Los verbos utilizados para nombrar a los servicios deben pertenecer al ubiquitous language, o ser introducidos en el en caso de que a�n no lo sean. A la hora de implementarlos tanto sus par�metros como resultados deben ser objetos pertenecientes al dominio.

Un servicio debe de cumplir tres caracter�sticas principales:

* La operaci�n que lo define est� relacionada con un concepto de dominio, pero no es natural modelarlo como una entidad o un value object.
* Su interfaz se especifica usando otros elementos del modelo de dominio.
* La operaci�n no tiene estado, por lo que cualquier cliente podr�a usar cualquier instancia del servicio sin tener en cuenta las operaciones que se han realizado con anterioridad en esa instancia.

Podemos dividir los servicios en tres tipos diferentes seg�n su relaci�n con el n�cleo del dominio.

> Domain services
Son responsables del comportamiento m�s espec�fico del dominio, es decir, realizan acciones que no dependen de la aplicaci�n concreta que estemos desarrollando, sino que pertenecen a la parte m�s interna del dominio y que podr�an tener sentido en otras aplicaciones pertenecientes al mismo dominio.Por ejemplo, crear un nuevo curso.

> Application services
Son responsables del flujo principal de la aplicaci�n, es decir, son los casos de uso de nuestra aplicaci�n. Son la parte visible al exterior del dominio de nuestro sistema, por lo que son el punto de entrada-salida para interactuar con la funcionalidad interna del dominio. Su funci�n es coordinar entidades, value objects, domain services e infrastructure services para llevar a cabo una acci�n.Por ejemplo, a�adir un nuevo alumno al registro para tomarle la asitencia.

> Infrastructure services
Declaran comportamiento que no pertenece realmente al dominio de la aplicaci�n pero que debemos ser capaces de realizar como parte de este. Por ejemplo, enviar el registro de asistencia de cada alumno a su correo. 
