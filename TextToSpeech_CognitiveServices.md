# Machine Translation

## Literal and Semantic Translation
Una traducción literal es cuando cada palabra se traduce a la correspondiente en el lenguaje objetivo. Este enfoque trae desventajas: puede que la palabra que queremos traducir no exista en el lenguaje objetivo, además no tiene en cuenta el contexto.

Los sistemas de inteligencia artifical deben ser capaces de entender, no solo las palabras, sino el *contexto semántico* en el cual se emplean. De esta manera, el servicio puede generar una traducción mas acertada respecto a la palabra o frase de la que partió.
Es necesario que el vocabulario, tanto formal como informal, y expresiones coloquiales, sean todas consideradas.

## Text and speech translation
La *traducción de textos* se puede usar para pasar documentos de un lenguaje a otro, emails de gobiernos extranjeros, y también páginas web.

La *traducción de habla* se usa para traducir entre lenguajes hablados, a veces directamente (voz a voz), otras pasándolo primero a texto como forma intermedia (voz a texto a voz).

## Lo que Azure provee

* El servicio **Translator Text** , que permite traducción text-to-text.
* El servicio **Speech** , que permite  traducciones speech-to-text y speech-to-speech.

## Recursos de Azure Para Translator Text y Speech

Antes de usar estos dos servicios, se debe proveer recursos adecuados en la suscripción de Azure, que se pueden usar si se quiere gestionar el acceso y cobro para cada servicio de manera individual.

De forma alternativa, se puede crear un recurso de Cognitive Services que provea acceso a ambos servicios, configurando el cobro y permitiendo a aplicaciones acceder a ambos servicios con un solo endpoint y authentication key.

## Traducción de texto con el servicio Translator Text
Este servicio usa un modelo NMT (Neural Machine Translation) para traducir, el cual analiza el contexto semántico y renderiza una traducción mas acertada y completa como respuesta.

### Lenguage Support
Posee mas de 60 lenguajes.
Cuando se utilice el servicio, se debe especificar desde qué lenguaje se traduce y a cuál, utilizando ISO 639-1 como código de lenguaje.
Ex: *en* = English , *fr* = French , *zh* = Chinese. Además, se pueden especificar vvariantes culturales con el código 3166-1 apropiado. Por ejemplo: *en*-US = US English, *en*-GB = British English o *fr*-CA = Frances Canadiense.

Al utilizar el servicio, se puede traducir de un lenguaje a múltiples de forma simultánea.

### Optional Configuration
El servicio tiene algunas configuraciones extra para refinar el resultado de la traducción áun más.

* **Profanity Filtering** : por default el servicio traduce todo, sin importar qué diga, pero se puede configurar para así marcar el texto traducido como inadecuado, o directamente omitiendo su traducción de los resultados.
* **Selective Translation** : Se puede marcar contenido para no ser traducido. Como por ejemplo código, marca de un producto, o una frase o palabra que no tiene sentido.

## Traducción de voz con el servicio Speech
El servicio contiene las siguientes APIs:
* **Speech to text** voz a texto.
* **Text to speech** texto a voz.
* **Speech Translation** para traducir desde un discurso a otro en diferentes lenguajes.

La API de Speech Translation se puede usar para traducir audio hablado desde una fuente de streaming, como un micrófono o archivo de audio, y devolver la traducción como texto o como stream de audio también.

Esto permite situaciones como subtitulado en tiempo real o traducción en vivo de conversaciones habladas (como en los People Choice's)

### Language Support
el mismo formato que para Translator Text

# Text-to-Speech
Es un servicio multiplataforma capaz de un rango de voces y lenguajes, incluyendo tonos por regiones y géneros. Las voces pueden personalizarse a tu requerimiento particular, con estilo de voz, velocidad para hablar y tono, creando voces únicas para cada aplicación.
Además, se puede animar un rostro virtual a tu app. Esto agrega aspectos más interactivos a tu aplicación, aparte de agregar mayor accesibilidad con lectura de labios.

## Tipos de voces
* Standard voice -> la más simple y efectiva en cuanto a costo.
* Neural voice -> es una voz sintetizada que es casa indistinguible de grabaciones humanas. Alimentadas por redes neuronales, estas voces suenan mas natural que las standard, produciendo discursos o habla similares a los humanos, como estrés y tono mas fuerte en ciertas palabras. Con esta voz las palabras se articulan de forma mas precisa y el usuario no se cansa tanto de escucharla. 
* Custom Neural voice -> tu propio audio como dato. Tiene el nivel más profundo de personalización, con un discurso realista que puede usarse para identificar marcas, personaliar máquinas, y permitir a los usuarios interactuar con aplicaciones conversacionalmeente.

*Si estamos hablando de textos muy largos, LONG AUDIO API hace el trabajo por nosotros, genera audios largos de mas de 10min*

## Cómo funciona?
Este servicio requiere una cuenta Azure y un recurso creado.
Se puede acceder al servicio mediante REST, Speech CLI, o con un *software development kit* (SDK).

## Eligiendo una voz
Se puede sintetizar un string de texto en audio con un solo método. Text-to-speech devolverá audio en formato .wav, que podrá guardarse como archivo o utilizada como in-memory stream. El lenguaje se especifica eligiendo la voz apropiada y localización al llamar el método.
El servicio puede enviar una lista de voces y localidades a tu programa, así la salida deseada será seleccionada.

La salida de audio puede personalizarse incluyendo:
* tipo de archivo de audio
* sample rate
* bit deph

Se puede elegir entre voces standard o neural cambiando el atributo "voice" del método.
Text-to-speech se puede usar con otros servicios, como traductor o mejora de accesibilidad.

## Granular speech control
Speech Synthesis Markup Language (SSML) permite configurar muchas características del audio de salida.

```
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-AriaNeural">
        This is the text that is spoken by Aria.
    </voice>
</speak>

```
**xml:lang** especifica el lenguaje en el que está escrito el texto que queremos hablado, en este caso, en inglés con localidad en US.
Los standards que utiliza son los de la world wide web consortium (W3C), que deberían usarse por default.

## Ajustando estilos de habla
Con las voces neuronales, se puede cambiar diferentes estilos de habla para expresar emociones como tristeza, calma y seriedad. Se puede especificar tambien tipos de voces, como servicio al cliente, noticiero y asistente virtual, utilizando el elemento **xmlns:express-as**.

```
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xmlns:mstts="https://www.w3.org/2001/mstts" xml:lang="en-US">
  <voice name="en-US-AriaNeural">
    <mstts:express-as style="cheerful">
      This is awesome!
    </mstts:express-as>
  </voice>
</speak>
```
## Custom Voices
Se pueden crear usando Speech Studio, un portal de personalización del servicio de Azure Speech. Ahí se te proveen instrucciones sobre cómo crear el audio y transcripción correspondiente. Al subir los archivos, el servicio crea una voz personalizada y provee con un endpoint personalizado también para que puedas acceder a ella.
Microsoft posee políticas para permitir el uso de voces neuronales, no son accesibles por default, ya que tienen un compromiso con el uso responsable de IA, y está siendo un tema de preocupación las voces emuladas por deepfake.

Para acceder a las voces neuronales personalizadas, se debe aplicar y proveer evidencia.

# Cuando utilizar text-to-speech
## Assistive Technology
La voz neuronal provee una voz clara, natural, con la habilidad de personalizarla mucho. Esto último quiere decir que los individuos pueden encontrar el sonido que mejor se ajuste a su personaje.
Además, al no ser una voz tan mecánica, no produce tanta fatiga auditiva, volviéndola una opción ideal para anuncios repetitivos y uso dentro del auto.

Text-to-speech puede mejorar la accesibilidad de las aplicaciones para gente con impedimentos visuales y otras necesidades.
Para diferencias en aprendizaje, como dislexia, text-to-speech promueve el aprendizaje independiente sintetizando audio en tiempo real. La API de Long Audio puede posibilitar un mundo de literatura que de otra manera sería inaccesible.

## Anuncios de Servicios Públicos
Los anuncios públicos deben ser confiables y entendibles. Text-to-speech puede ir junto a servicios como un traductor para ayudar a las organizaciones a comunicar de forma rápida información al público.
Muy útil en regiones donde se hablen varios idiomas. El usar algo tan uniforme da una imagen mas profesional y confiable de los gobiernos, y el conocimiento de que ningún grupo es excluido de anuncios importantes.

## Aprendizaje Online
## AudioLibros
## Chatbots de Servicio al Cliente
Los chatbots de Conversational AI pueden automatizar algunas operaciones de call-center, con Text-to-speech como proveedor de voces que suenen como personas, se puede personalizar un servicio mas amigable, y tono de conversación. Esto reduce la fricción con interacción entre clientes y una AI.







