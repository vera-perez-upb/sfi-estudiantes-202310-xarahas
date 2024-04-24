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

## Ejercicio 6
``
SerialPort _serialPort =new SerialPort();
_serialPort.PortName = "/dev/ttyUSB0";
_serialPort.BaudRate = 115200;
_serialPort.DtrEnable =true;
_serialPort.Open()
``
este código establece una conexión serial entre el programa y un dispositivo externo, lo que permite la comunicación de datos entre ambos a través del puerto serial especificado.

SerialPort _serialPort = new SerialPort();: Esta línea crea una nueva instancia de la clase SerialPort, que es la clase utilizada para la comunicación serial
_serialPort.PortName = : Aquí se especifica el nombre del puerto serial al que se va a conectar.
_serialPort.BaudRate = 1152 : La velocidad de baudios especifica la tasa de transferencia de datos entre el dispositivo y el ordenador. En este caso, se establece en 115200 bits por segundo.
_serialPort.DtrEnable = true;: Aquí se habilita la señal de control DTR. Esta señal es utilizada para indicar al dispositivo conectado que el terminal está listo para comunicarse.

``
byte[] data = { 0x01, 0x3F, 0x45};
_serialPort.Write(data,0,1);
``
Envía una serie de bytes a través del puerto serial, en la declaración del arreglo de bytes.
- La línea byte[] data = { 0x01, 0x3F, 0x45}; declara un arreglo de bytes llamado data que contiene los valores hexadecimales 0x01, 0x3F, y 0x45. Estos valores representan los datos que se desean enviar a través del puerto serial.
- La línea _serialPort.Write(data,0,1); invoca el método Write del objeto _serialPort para enviar datos a través del puerto serial. Los parámetros que recibe el método Write son:
data: El arreglo de bytes que se va a enviar.
0: La posición inicial en el arreglo de bytes desde donde se comenzará a enviar datos.
1: La cantidad de bytes que se enviarán. En este caso, solo se enviará el primer byte del arreglo.


``
byte[] buffer =new byte[4];
.
.
.

if(_serialPort.BytesToRead >= 4){

    _serialPort.Read(buffer,0,4);
for(int i = 0;i < 4;i++){
        Console.Write(buffer[i].ToString("X2") + " ");
    }
}

``

este código lee 4 bytes del puerto serial (si están disponibles), los almacena en un búfer y luego imprime esos bytes en formato hexadecimal en la consola.
