
Speech to text convierte audio a texto en tiempo real, utilizando Inteligencia Artificial. Se puede usar en múltiples plataformas con diferentes lenguajes de programación para transcribir de diferentes fuentes de audio.
Tus aplicaciones, herramientas, o dispositivos pueden entonces utilizar este texto de diferentes formas, mostrar y tomar acción sobre este texto como si fuera input de comando.

## Speech-to-text services

### Standard Transcription
La transcripcion standard provee transcripcion en tiempo real de audio a texto. Se puede obtener los datos de un micrófono, archivos de audio o almacenamiento en blob. Este es el servicio mas óptimo para escenarios de dictado o conversación.

### Batch Transcription
Te permite transcribir grandes cantidades de audio en almacenamiento. Se pueden transcribir uno o más archivos de audio, o procesar un contenedor completo de ellos, recibiendo resultados al cabo de un tiempo (non-real-time)

### Customer Speech
Te deja evaluar y mejorar la exactitud speech-to-text para tus aplicaciones y productos, entrenando y testeando un modelo, usando tus propios archivos de audio. Además ayuda a superar obstáculos como terminología específica de una industria, o ruido de fondo, para proveer exactitud en la transcripción del audio.

Se puede conectar automáticamente a una cuente de Microsoft 365 Enterprise para generar un modelo de reconocimiento de voz personalizado en para tu organización.

### Conversation Transcription
Combina reconocimiento de vez, identificación de quien habla, y reconocimiento de atributos en oraciones, capacidades de transcribir e identificar quién dijo cada cosa en esa transcripción. Este servicio puede hacerse en tiempo real y de forma asíncrona. 

### Automatic Language Detection
Sirve para detectar que lenguaje es el que puede estar hablándose en el audio que brindaste, basado en una lista de lenguajes. Detección de lenguajes puede descubrir hasta 4 lenguajes por detección, permitiendo a speech-to-text una transcripción más acertada. Este servicio está disponible en más de 30 lenguajes.

### Pronunciation Assessment
Los que aprenden un lenguaje nuevo pueden practicar el habla y obtener feedback de la AI sobre qué necesitan mejorar.

### Continuous Recognition
Se usa en casos donde se quiere controlar cuando detener speech-to-text para que deje de transcribir. Es útil en modo Dictation, para cuando se quieren dictar cartas o reportes, este modo además interpretará signos de puntuación, interrogación, etc.

### Profanity Filters
Disponibles para transcripción por Batch, se usan para enmascarar la mala palabra con asteriscos, borrarla, o etiquetarla como "Profanity". La configuración por defecto es enmascararla.


## Funcionamiento de Speech-to-text

Se requiere una cuenta Azure y una suscripción al servicio Speech. 
Speech-to-text puede correr en un container, esto quiere decir que tu información no debe ser enviada a un servidor. Se puede correr la información de forma local, ahorrando ancho de banda, a la vez que también incrementando seguridad.
Se puede acceder al servicio a través del kit de desarrollo de software (Speech SDK), REST API, y Speech CLI. 
El Speech SDK provee una aplia suite de funcionalidades y es ideal para casos que sean o no en tiempo real, usando dispositivos locales, archivos, almacenamiento en blob de Azure o streams de entrada/salida.
Cuando no puede conseguirse esto con Speech SDK, optar por REST API.

## Speech-to-text en tiempo real
El servicio por default usa el lenguaje universal, se entrenó con datos de Microsoft y se deployea en la nube.
Speech-to-text se puede lograr incluso con diferentes entradas de audio.
Para seleccionar un micrófono o dispositivo de audio se utiliza REST API, se obtiene el ID del dispositivo, que entonces puede ser utilizado por el Speech SDK.
El uso de micrófono no está disponible para JavaScript con Node.js.
Speech-to-text puede usarse para archivos de audio y para almacenamiento in-memory.

Hasta 60seg de audio y si Speech SDK no sirve --> Speech-to-text REST API

(por ejemplo si usamos modelos que emplean múltiples data-sets)
El formato por default es .WAV, pero también se soportan MP3, OGG, FLAC, ALAW, y MULAW.

Pronunciation Assessment tambien funciona en tiempo real. Utiliza un texto de referencia, y se pueden configurar parámetros, como el sistema de puntuación.
Además, se puede configurar si el asesoramiento es apartir de las sílabas, texto completo, o solo una palabra.


## Batch Speech-to-text
Esta transcripción es un set de operaciones REST API que te permite transcribir una gran cantidad de audio en almacenamiento. Se pueden referenciar archivos de audio utilizando un URL o Shared Access Signature (SAS) URL para recibir la transcripción en tiempo no real. Con la version v3.0 API, se puede transcribir uno o más archivos de audio, o precesar todo un container de almacenamiento.

La transcripción por batch soporta formatos WAV, MP3 y OGG. La API es capaz de muchas funciones mencionadas para transcripción en tiempo real. Adicionalmente, puede borrarlas automáticamente luego de completarlas, y lanzar speech models personalizados.

```
{
    "contentContainerUrl": "<SAS URL to the Azure blob container to transcribe>",
    "properties": {
        "wordLevelTimestampsEnabled": true
    },
    "locale": "en-US",
    "displayName": "Transcription of container using default model for en-US"
}

```

### Custom Speech-to-text

Los modelos personalizados se usan para dar mas precisión a la transcripción. Se pueden crear utilizando Speech Studio, un portal de personalización para el servicio de Speech de Azure.
Junto con este portal, vienen instrucciones para deployear estos modelos de speech recognition utilizando audio con transcripciones aprobadas por personas, textos relacionados y asesoría de pronunciación (todo esto provisto por ti).
Esto puede ayudarte a superar problemas como acentos, ruido de fondo, conjuntos de palabras específicos.

### Usar datos de 365
Para organizaciones que se encuentren utilizando Microsoft Enterprise, los Tennant Models son una opción de servicio. Estos automáticamente generan reconocimiento personalizado de voz desde los datos de tu organización, como emails públicos y documentos. Es óptimo para términos técnicos, jergas, y nombre de Personas. Todo de forma segura y competente.
Para usar estos modelos, el administrador debe inscribirse en los servicios de Azure Speech y permitir el *organization-wide language models*, junto con la creación del recurso de Speech y la clave de suscripción.
Para crear y lanzar el Tenant Model, se necesita entrar en Speech Studio, y marcar los settings de deploy Tenant Model.

### Phrase Lists
Se provee una lista de palabras o frases con altas probabilidades de aparecer en el discurso y ser confundidas. Como el nombre de una persona, o terminología específica de una industria. Al proveer esta lista, se aumenta la probabilidad de que Speech-to-text las reconozca y dé una transcripción acertada, incluso en el medio de las oraciones. 


## Cuando usar Speech-to-text

* Campo médico : transcribir conversaciones con pacientes y dictar notas.
* Accesibilidad: al sacar del medio el tipeo, personas con alguna dificultad para tipear pueden comunicarse mas facilmente con pares o aplicaciones. También subtitulos en tiempo real.
* Compliance Standards: Companías y organizaciones gubernamentales pueden tener transcriptas las llamadas, sin perder tanto tiempo.
* Comunicación en Organizaciones: Transcribir meetings de trabajo, se puede usar a la par con un traductor. Si se quiere ir mas lejos, se puede traducir, transcribir e identificar quién es el que habla, para crear un entorno de trabajo mas inclusivo y eliminar barreras de comunicación entre miembros del equipo. Esto ahorra la toma de notas, de esta forma todos están más atentos y presentes. Se puede encontrar una copia de lo que se dijo una vez finalizada la reunión.
* 





