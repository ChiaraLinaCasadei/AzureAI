Si se requiere un servicio que procese los datos orientados a lo mismo, pero de diferentes fuentes, se recomienda *Serverless Computing*. Con ella, el proveedor cloud administra aprovisionamiento y mantenimiento de la infraestructura, permitiéndote enfocarte completamente en la lógica de la app.
**Azure Functions** es un componente clave de *serverless computing* que ofrece Azure, te permite correr fragmentos de código o funciones, en la nube.

## Serverless Compute
Es como una Función como Servicio (FaaS), o un microservicio hosteado en la nube. Tu lógica de negocio corre como funciones y no hay que proveer o escalar la infraestructura manualmente. El proveedor cloud administra la infraestrutura. Tu aplicación es automaticamente escalada (hacia arriba o abajo) dependiendo de la carga. Azure tiene diferentes formar de construir este tipo de arquitectura:
* Azure Logic Apps
* Azure Functions

## Azure Functions
Es una plataforma de aplicación serverless. Le permite a los desarrolladores hostear lógica de negocio que puede ser ejecutada sin proveer la infraestructura. Las funciones tienen escalabilidad intrínseca, y solo se cobran los recursos utilizados. Se puede escribir el código de tu función en el lenguaje a tu elección dentro de los soportados. También está incluido el soporte para administradores de paquetes, como NuGet y NPM, así se puede tener acceso a librerías populares en tu lógica de negocio.

## Beneficios de una solución tipo Serverless
Es una gran opción para hostear lógica de negocio en la nube. Con, por ejemplo, Azure Functions, se puede escribir lógica de negocio en el lenguaje a elecciónn, que se escala automáticamente y no hay servidores que administrar, se cobra solo lo que se está utilizando, no por tiempo reservado.
Características Extras:

### Evita la sobre-asignación de infraestructura
Suponiendo que se te asignan servidores de una máquina virtual (VM) configurados con suficientes recursos para manejar los horarios de mayor carga. Cuando la carga es baja, estás pagando por infraestructura que no se usa. Serverless soluciona esto con escalando hacia arriba o hacia abajo la infraestructura de forma automática, y solo se te cobra cuando tu función está procesando trabajo.

### Stateless logic
Las funciones sin estado son buenas candidatas para serverless compute. Las instancias son creadas y destruidas a demandas. Si el estado es requerido, se pueden almacenar en un servicio de almacenamiento asociado.

### Orientado a Eventos
Las funciones solo corren en respuesta a un evento ("trigger"), como recibir una solicitud HTTP, o un mensaje agregado a la cola "queue". El trigger se configura como parte de la definición de la función. Este enfoque simplifica el código permitiéndote declarar desde donde viene la información (trigger/input binding) y hacia donde va (output binding). No se necesita escribir código para vigilar queues, blobs, hubs y así. Uno puede enfocarse completamente en la lógica de negocio.

### Las funciones pueden utilizarse en otros entornos fuera de serverless
Aunque sean un componente clave en serverless computing, son también útiles como plataforma para cualquier tipo de código. Si las necesidades de tu aplicación cambian, tu proyecto puede lanzarse en un ambiente no-serverless, lo que te da la flexibilidad de administrar el escalado, correr en redes virtuales, e incluso aislar completamente tus funciones.

## Inconvenientes de una Solución Serverless
No siempre va a ser la más apropiada para tu lógica de negocio...

### Tiempo de Ejecución
Por default, las funciones tienen un tiempo límite de 5min. Este puede configurarse a un máximo de 10min, Si tu función requiere más de este tiempo para ejecutarse, puede alojarse en una máquina virtual (VM).
Adicionalmente, si tu servicio se inicia mediante una solicitud HTTP y esperas ese valor como una respuesta HTTTP, el tiempo límite queda restringido a 2.5min. Finalmente, está la opción llamada **Durable Functions** , que te permite gestionar la ejecución de múltiples funciones sin ningún timeout.

### Frecuencia de Ejecución
Si esperas que tu función sea ejecutada continuamente por múltiples clientes, sería prudente estimar el uso y calcular los costos. Quizá sea más económico alojar el servicio en una VM.

Mientras se escala, solo una instancia de una función de la app puede ser creada cada 10segs, hasta 200 instancias en total. Teniendo en cuenta que cada instancia puede ofrecer múltiples ejecuciones concurrentes (esto quiere decir que se van intercalando las tareas que componen cada ejecución, no se hacen al mismo tiempo (paralelismo) ni una despues de otra (estructurada)), así que no hay un límite específico respecto a cuánto tráfico puede tolerar una sola instancia. Diferentes tipos de triggers tienen diferentes requerimientos en cuanto a escalabilidad, así que es conveniente investigar tu elección de trigger, junto con sus límites.


## Function App
Es un contexto de ejecución donde se alojan las funciones. Las *Function Apps* se definen para agrupar y estructurar lógicamente tus funciones y recursos de cómputo en Azure.
Hay algunas decisiones que deben tomarse para crear la function app:
* service plan
* compatible storage account

### Service Plan
  **Consumption Plan**
  
   Provee escalabilidad automática y te cobra solo cuando las funciones están corriendo. Viene con un tiempo límite configurable para cada función que se ejecuta. Por default es de 5min, pero puede configurarse hasta 10min.
  
  **Azure App Service Plan**
  Te permite evadir períodos de tiempo límite al tener tu función corriendo en una VM que definas. Al usar este plan, es tu responsabilidad gestionar los recursos de la app en la que corre tu función, así que técnicamente no es Serverless.
  Igualmente, puede ser una mejor elección si tus funciones son utilizadas continuamente, o si requieren mas poder de procesamiente, o mayor tiempo de ejecución que el que puede proveer el Consumption Plan.
  
### Storage Account Requirements
Al crear  una Function App, debe vinculársela a una Storage Account. Se puede elegir una existente, o crear una nueva.
La function app usa la storage account para operaciones internas, como cargar ejecuciones de funciones y administrando triggers de ejecución. En el Consumption Plan, aquí es donde se almacena también el código de la función y el archivo de configuración.


## Triggers

Un trigger es un objeto que define como se llama a una Azure Function.
Cada funcion debe tener un solo trigger asociado a ella. Si se quiere ejecutar una pieza de lógica en múltiples condiciones, entonces hay que crear múltiples funciones que compartan el mismo código principal.

| Type	| Purpose |
| ---------- | -------- |
| Timer	 | Execute a function at a set interval |
| HTTP |	Execute a function when an HTTP request is received |
| Blob |	Execute a function when a file is uploaded or updated in Azure Blob storage |
| Queue |	Execute a function when a message is added to an Azure Storage queue |
| Azure Cosmos DB	| Execute a function when a document changes in a collection |
| Event Hub	| Execute a function when an event hub receives a new event |

## Binding

Es una conección entre datos y la función. Son opcionales y pueden ser de input o de output, o ambos.
Un Input Binding es la información que tu función recibe.
Un Output Binding es la información que tu función envía.

A diferencia de los triggers, una función puede tener múltiples bindings de input/output

### Timer Trigger
Un trigger que ejecuta una función en un intervalo consistente de tiempo. Para crear uno debemos brindar 2 tipos de información:
1) Un *timestamp parameter name* , que es el nombre con el que idenfificamos el trigger y accedemos a él dentro del código.
2) Un *Schedule*, que es una expresion CRON que setea un intervalo para el timer.

#### Cron Expression
Es un string que consiste de 6 campos que representan un set de horario.
La sintaxis para Azure es: 
```
 {second} {minute} {hour} {day} {month} {day of the week}
```
Si queremos, por ejemplo, crear una expresión para un trigger que debe ejecutarse cada 5 minutos:
``` 0*/5**** ```

Sintaxis:

| Special character	| Meaning	| Example |
| ----- | ------- | ------ |
| *	| Selects every value in a field	| An asterisk "*" in the day of the week field means every day |
| ,	| Separates items in a list	| A comma "1,3" in the day of the week field means just Mondays (day 1) and Wednesdays (day 3) |
| -	| Specifies a range	| A hyphen "10-12" in the hour field means a range that includes the hours 10, 11, and 12 |
| /	| Specifies an increment | A slash "*/10" in the minutes field means an increment of every 10 minutes |


Analizando la expresion vista antes:
``` 0 */5 * * * * ```
El primer campo representa los segundos, y comprende valores entre 0-59. Así que si tiene un cero, estamos hablando del primer segundo.

El segundo campo representa minutos. El valor */5 tiene 2 caracteres especiales. El primero es *, que significa que aplica a cada valor de este campo. La barra / representa un incremento "cada x", entonces combinando todo junto con el 5 al final. Tenemos algo así: "de todos los minutos que tenga este campo, va a sonar cada 5. Va saltando de 5 en 5" 
Los otros campos que quedan tienen todos *, lo que quiere decir que este "cada 5min" es válido para todos los días, de todas las semanas, de todos los meses, de todos los años.

### HTTP Trigger
Se activa al recibir una solicitud HTTP.
Incluyen:
* Acceso autorizado por keys
* Se puede restringir cuales verbos HTTP acepta
* Devuelven datos al que activó/llamó el trigger
* Reciben datos mediante parámetros del string de la solicitud (get), o desde el body (post)
* Aceptan route templates para modificar el URL de la función

Al crear uno de estos triggers, se necesita seleccionar el lenguaje de programación, nombre del trigger y nivel de Authorization.

#### Authorization Level
* Function
* Admin
* Anonymous

Los primeros dos requieren una *key* para poder enviar una solicitud. 
Existen dos tipos de keys: Function y Host.
La diferencia entre ellas es el rango. Las de Function aplican solamente a una función específica.
Las de Host aplican a todas las funciones dentro de la Function App

Se puede acceder a una funcion con nivel de autorización Function con clave tanto Function como Host.
Pero a una con nivel Admin solo con una clave tipo Host.

La Anonymous no necesita ningun tipo de key.

## Blob Trigger
Ejecuta la función cuando un archivo es subido o actualizado en en Azure Blob Storage. Para crear uno, debe crearse una cuenta de Azure Storage y proveer una dirección para que el trigger monitoree.
### Azure Storage
Es una solución de Microsoft Cloud que es compatible con todos los tipos de información, incluyendo: blolbs, queues y NoSQL. El objetivo es proveer un almacenamiento de datos que es:
* Altamente disponible
* Seguro
* Escalable
* Administrado

### Azure Blob Storage
Es una solución de almacenamiento de objetos diseñada para almacenar grandes cantidades de información sin estructura

Muy buena opcion para:
* guardar archivos
* entrega de archivos
* transmisión de video y audio
* registro de datos

Hay tres tipos de blobs: block blobs, append blobs y page blobs.

* **Block**: El tipo mas común, te sirven para giardar texto o archivos binarios eficientemente.
* **Append**: Son como los Block, pero están diseñados para operaciones de agregar datos, como crear un log que se actualiza constantemente.
* **Page**: Hechos de páginas diseñadas para operaciones de lectura y escritura de acceso aleatorio.


### Creando un Blob Trigger - Path
El Path le dice al trigger donde monitorear. Por default su valor es:

``` samples-workitems/{name} ```

*samples-workitems* representa el blob container que va a monitorear el trigger-

*{name}* quiere decir que cualquier tipo de archivo va a causar que el trigger invoque la función. 

La función se va a llamar siempre porque no hay filtro. Si quisiéramos uno, podríamos invocar la función solo cuando se agrega un archivo .png.

``` samples-workitems/{name}.png ```

"name" representa el nombre del parámetro en la Azure Function que recibe el nombre del archivo añadido.

## Types of Supported Bindings
El tipo de binding define desde donde estamos leyendo o enviando información. Hay un binding para responder a solicitudes web, y una gran sección de bindings para interactuar directamente con varios servicios Azure, como también de terceros.
Estos bindings se pueden usar como input, output o ambos. Por ejemplo, una función puede escribir a un Blob Storage Output Binding, pero esa escritura produce un "update" en el Blob, que puede ser trigger de otra función.

Tipos comunes de binding:

* Blob Storage
* Azure Service Bus Queues
* Azure Cosmos DB
* Azure Event Hubs
* External files
* External tables
* HTTP endpoints

### Bindings Properties

* **Name** - Define el parámetro de la función mediante el cual se accede a la información (por ejemplo, en un queue input binding, es el nombre del parámetro de la función que recibe el contenido del mensaje de queue).
* **Type** - Identifica el tipo de binding (por ejemplo, el tipo de información o servicio con el que queremos interactuar).
* **Direction** - Indica la dirección hacia la que fluye la información (por ejemplo, si es un binding de input o de output)

Además, muchos tipos de binding necesitan una cuarta propiedad:
* **Connection** - Provee el nombre de una clave en appsettting que contiene la cadena de conección.
Los binding usan connection strings almacenadas en appsettings para mantener información confidencial fuera del código de la función. Esto vuelve el código mas configurable y seguro.

### Crear un Binding

Los Bindings se definen en JSON. Se configuran en el archivo de configuración de tu función, que se llama *fuction.json* y está en la misma carpeta que el código de tu función.

```
...
    {
      "name": "headshotBlob",
      "type": "blob",
      "path": "thumbnail-images/{filename}",
      "connection": "HeadshotStorageConnection",
      "direction": "in"
    },
    ...
```
1) Creamos un binding en el archivo *function.json*.
2) Le damos un valor a la variable *name*, que en este ejemplo va a alojar información Blob.
3) Proveemos el tipo *type* de almacenamiento
4) Le damos el *path*, que especifica el container y el nombre del item que va en él. Ésta se requiere al usar un Blob Trigger, y debe proveerse como se la muestra aquí, con llaves que encuadren la porción del nombre del archivo del path. Esto crea una *binding expression* que te permite referenciar el nombre del blob en otros bindings, y en el código de tu función. En este ejemplo, el parámetro en la función llamado *filename* será llenado con el nombre del archivo del blob que activó la función.
5) Proveer el nombre de la *connection* string bajo la cual está alojada en appsettings. Se usa como clave para encontrar la connection string real a tu storage account.
6) Definir la dirección *direction*. En el ejemplo está puesta como *in*, porque lee info del blob.

Los bindings se usan para connectar información en tu función.

## Binding Input

* **Azure Blob Storage** - Leer info de un Blob 
* **Azure Cosmos DB** - Usar la SQL API para devolver uno o mas Azure Cosmos DB documents, y pasarlos como parámetro input de la función. El ID del documento o parámetros query se pueden determinar en base a un trigger que invoque la función.
* **Mobile Apps** - Carga un endpoint de un registro Mobile Table y se lo pasa a tu función.
* **Azure Table Stirage** - Se puede leer información y trabajar con Azure Table Storage.

Para crear binding como input se debe definir el valor de la variable *direction* como *in*.

### Binding Expressions
* App settings
* Trigger filename
* Trigger metadata
* JSON payloads
* New GUID
* Fecha y hora actuales

La mayoría de las expresiones se identifican por estar entre llaves. Las expresiones de appsetting para binding se encasillan en signos de porcentaje.
Por ejemplo, el path para un binding de output de tipo blob ```%Environment%/newblob.txt```, y el valor Environment en appsetings es Development, se creará un Blob en el container Development.

## Binding Output

* **Azure Blob Storage** - Escribir Blobs 
* **Azure Cosmos DB** - Usar la SQL API para escribir uno o mas documentos en una Azure Cosmos DB.
* **Event Hubs** - Para escribir eventos en un event stream. Se debe enviar permisos al Event Hub para escribirle eventos.
* **HTTP** - Para responder a solicitudes HTTP. Requiere un HTTP trigger y te permite personalizar la respuesta asociada con la soliciud del trigger. Se puede usar tambien para conectarse a webhooks.
* **Microsoft Graph** - Permite escribir archivos en OneDrive, modificar info en Excel y enviar emails mediante Outlook.
* **Mobile Apps** - Escribe un nuevo registro en una table Mobile Apps.
*  **Notification Hubs** - Se pueden enviar notificaciones push.
*  **Queue Storage** - Escribir mensajes en una queue.
*  **Send Grid** - Envia emails
*  **Service Bus** - Para enviar queue o topic messsages.
*  **Table Storage** - Escribir en una table de Azure Storage Account.
*  **Twilio** - Envia mensajes de texto






