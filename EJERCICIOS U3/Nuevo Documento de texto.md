## Ejercicio 1
- ¿Cómo se ve un protocolo binario?
  Un protocolo binario es un conjunto de reglas y formatos que define cómo se comunican dos sistemas informáticos mediante datos binarios,
  es decir, 0s y 1s. En contraste con los protocolos de texto, que se basan en caracteres legibles por humanos, los protocolos binarios están
  optimizados para la eficiencia en el uso de ancho de banda y el procesamiento de datos por parte de las computadoras.

- ¿Puedes describir las partes de un mensaje?
Las partes de un mensaje en un protocolo binario típicamente incluyen:

Encabezado: Es la parte inicial del mensaje que contiene información crucial para interpretar el resto del mensaje,
como la identificación del tipo de mensaje, la versión del protocolo, la longitud del mensaje, y otros metadatos relevantes.

Cuerpo del mensaje: Es la parte principal del mensaje que contiene los datos o la información que se está transmitiendo. 
Puede ser estructurado de diversas maneras dependiendo del protocolo, y puede incluir cualquier tipo de información,
desde texto simple hasta datos complejos como imágenes, archivos binarios o estructuras de datos.

Checksum (suma de comprobación): Esta es una parte opcional pero común en los mensajes binarios. 
Se trata de un valor numérico calculado a partir del contenido del mensaje, utilizado para verificar la integridad de los datos durante la transmisión. 
Al recibir un mensaje, el receptor puede calcular su propia suma de comprobación y compararla con la suministrada en el mensaje. 
Si coinciden, significa que el mensaje no ha sido alterado durante la transmisión.

- ¿Para qué sirve cada parte del mensaje?
Cada parte del mensaje sirve a un propósito específico:

Encabezado: Proporciona información crucial para interpretar y procesar el mensaje correctamente. 
Esto incluye información sobre el tipo de mensaje, la longitud del mensaje y otros metadatos necesarios.

Cuerpo del mensaje: Contiene los datos o la información que se está transmitiendo entre los sistemas. 
Esta parte del mensaje es la que lleva la carga útil de la comunicación y puede variar ampliamente dependiendo del propósito del mensaje y del protocolo utilizado.

Checksum: Sirve para verificar la integridad de los datos durante la transmisión.
Ayuda a garantizar que el mensaje recibido sea idéntico al mensaje enviado, sin ningún tipo de alteración o corrupción durante la transmisión. 
Esto es crucial para asegurar la fiabilidad de la comunicación en entornos donde la integridad de los datos es crítica.
