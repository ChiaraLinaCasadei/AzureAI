# Cognitive Services

Azure Cognitive Services le permite a los devs agregar capacidades AI en sus aplicaciones.

Son servicios cloud-based que encapsulan capacidades AI. Azure Cognitive Services es un set de servicios individuales que se pueden usar como piezas para construir aplicaciones inteligentes y sofisticadas.

Tipos de servicios:

## Language
Se utilizan para analizar texto y obtener el significado de él.
Ej: Text Analytics, Translator, QnA Maker, Language Uderstanding.

## Speech
Activar procesamiento de diálogo o discurso en apps donde lo hablado puede convertise a texto y viceversa. Mientras que tambien se traduce el texto a otros lenguajes e identifica speakers.
Ej: Speech to Text, Text to Speech, Speech Translation, Speaker Recognition.

## Vision
Puede analizar contenido visual como imágenes, GIFs, y videos para identificar objetos en ellos. Estos servicios pueden permitir a las aplicaciones detectar y agrupar rostros u objetos, basado en distintas características.
Ej: Computer Vision, Custom Vision, Face API.

## Decision
Estas APIs analizan data y patrones para funcionar de forma rápida, precisa y mas eficiente en la toma de decisiones.
Ej: Anomaly Detector, Content Moderator, Personalizer.

Se puede utilizar Cognitive Services para buildear tus propias soluciones AI, tambien incluyen *Azure Applied AI Services*, que proveen soluciones innovadoras para escenarios típicos. 

### Azure Form Recognizer
Es un OCR (Optical Character Recognition) que puede extraer significado semántico de formularios, como facturas, recibos y otros.

### Azure Metrics Advisor
Integrado en el servicio de Anomaly Detector que simplifica monitoreo en tiempo real y respuesta a métricas críticas.

### Azure Video Analyzer for Media
Una solución de analistas de video integrada en el servicio cognitivo Video Indexer.

### Azure Inmersive Reader
Una solución de lectura que trabaja con gente de todas las edades y habilidades.

### Azure Bot Service
Servicio en la nube para enviar soluciones de AI conversacional, o bots.

### Azure Cognitive Search
Una solución de búsqueda a escala cloud que usa cognitive services para extraer conocimiento de información y documentos.

## Proveer un recurso de Cognitive Services
Para usar cualquier capacidad AI incluida en cognitive services, se necesitan crear los recursos apropiados en una suscripción Azure para definir un endpoint donde el servicio puede ser consumido, proveer claves de acceso para acceso autenticado, y manejar el cobro por el uso que hace tu aplicación del servicio.

## Tipos de recursos Azure

1) Recurso Multiservicio: Este enfoque te permite manejar un solo set de credenciales para consumir múltiples servicios en un solo endpoint, con un solo punto de cobro por el uso de todos ellos.
2) Recurso de un solo servicio: Este enfoque permite utilizar diferentes endpoints por cada servicio (por ejemplo para proveerlos en diferentes regiones geográficas) y gestionar diferentes credenciales de acceso por cada servicio, de forma independiente. Además me permite manejar el cobro de cada servicio por separado. Los recursos de un solo servicio generalmente ofrecen un Tier gratuito (con restricciones de uso), volviéndolos una buena opcion para probar un servicio antes de utilizarlo en una aplicación de producción.

## Recursos para entrenamiento y predicción
Algunos cognitives services ofrecen, o requieren, recursos separados para entrenamiento y predicción de modelos. Esto permite gestionar el cobro por entrenar modelos personalizados de forma separada del consumo de modelos por aplicaciones, y en la mayoría de los casos permite usar un recurso dedicado a un servicio específico para entrenar un modelo, pero un recurso genérico de Cognitive Services para hacer el modelo accesible para aplicaciones para inferencia (conclusión).

## Identificar Endpoints y Claves

Al proveer un cognitive service en una suscrioción Azure, se está definiendo un endpoint mediante el cual la aplicación puede consumir el servicio.
Para poder consumir el servicio mediante el endpoint, las aplicaciones requieren la siguiente información:

### Endpoint URL
Es la dirección HTTP en la cual la interfaz REST para el servicio puede ser accedida. La mayoría de los kits de cognitive services para desarrollo de software (SDKs) usan la URL del endpoint para iniciar una conexión con dicho endpoint.

### Subscription Key
El acceso al endpoint está restringido según una clave de suscripción. Las aplicaciones que lo consuman deben proveer una clave válida para hacerlo.
Al proveer un recurso de cognitive service, se crean 2 claves, al consumirse pueden utilizarse cualquiera de las dos. Además se puede regenerar las claves como sea requerido para controlar el acceso al recurso.

### Resource Location
Al proveer un recurso en Azure, generalmente se asigna a una dirección, la cual determina el datacenter de Azure donde está definido el recurso. 
Mientras la mayoría de los SDKs utilizan la URL del endpoint para conectarse al servicio, algunas requieren la dirección.

## Utilizar REST API
Los cognitive services proveen interfaces de programación del tipo REST (APIs) que aplicaciones cliente pueden usar para consumir servicios. En la mayoría de los casos, las funciones de estos servicios se pueden llamar enviando información en formato JSON dentro de un HTTP Request (que puede ser POST, PUT o GET). Los resultados de la función se deuelven al cliente como una HTTP Response, generalmente con contenido JSON que encapsula la información de salida desde la función.

El uso de interfaces tipo REST, con un endpoint HTTP implica que cualquier lenguaje de programación, o herramiente, capaz de enviar y recibir JSONs mediante HTTP se puede usar para consumir cognitive services.
Se pueden usar lenguajes de programación como C#, Python y Javascript, así como utilidades como Postman y cURL, que pueden ser de gran ayuda para testear.

## Utilizar un SDK
Se puede desarollar una aplicación que utilice cognitive services mediante interfaces REST, pero es más facil el armar soluciones mas complejas si utilizamos librerías nativas del lenguaje en el cual estamos desarrollando la aplicación.

Los kits de desarrollo de software (SDKs) para lenguajes de programación abstraen las interfaces REST para la mayoría de los cognitives services.
La disponibilidad de los SDK varía según cognitive services individuales, pero para la mayoría de los servicios hay un SDK para lenguajes como: C# (.NET Core), Python, JavaScript(Node.js), Go y Java.

Cada SDK incluye paquedes que pueden instalarse para usar librerías de servicios específicos en el código, y documentación online para ayudarte a determinar las clases, métodos y parámetros apropiados utilizados para trabajar con el servicio.


