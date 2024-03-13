## EJERCICIO 1
¿Quién debe comenzar primero la comunicación, el computador o el controlador? ¿Por qué?
En este caso, la comunicación a través del puerto serial generalmente sigue un flujo donde el computador (maestro) inicia la comunicación.
En Unity, se usa la clase "SerialPort" para establecer la comunicación a través del puerto serial.
Es esencial establecer un protocolo de comunicación claro entre el computador y el controlador para asegurar una transferencia de datos adecuada.
Es importante efinir qué datos se enviarán y en qué formato.

¿Por qué es importante considerar las propiedades *PortName* y *BaudRate*?
Estas propiedades determinan la configuración del puerto serial y son críticas para el establecimiento de una comunicación exitosa entre el computador y el controlador

* PortName:
Esta propiedad especifica el nombre del puerto al que está conectado el controlador.
Por ejemplo, en Windows, los puertos seriales suelen tener nombres como "COM3" o "COM4".
En sistemas operativos basados en Unix, los nombres pueden ser algo como "/dev/ttyUSB0" o "/dev/ttyS1".
Es crucial establecer correctamente PortName para que Unity (o cualquier aplicación) pueda dirigirse al
puerto serial correcto y comunicarse con el controlador.

* BaudRate:
Esta propiedad determina la velocidad de transmisión de datos entre el computador y el controlador. 
Ambos extremos de la comunicación (computador y controlador) deben estar configurados con la misma velocidad de baudios para entenderse correctamente.
El baud rate se mide en bits por segundo (bps). Comúnmente, valores estándar son 9600, 19200, 38400, 57600, o 115200 bps.
La elección del baud rate es crucial porque si el computador y el controlador están configurados con velocidades diferentes, 
la comunicación puede fallar y los datos pueden interpretarse incorrectamente.

 ¿Qué relación tienen las propiedades anteriores con el controlador?
* El PortName es vital para identificar el puerto físico al que está conectado el controlador en el computador.
Si no se especifica correctamente, la comunicación no podrá establecerse.
* El BaudRate es crucial porque determina la velocidad con la que se transfieren los datos entre el computador y el controlador. 
Ambos extremos deben configurarse con el mismo BaudRate para que la comunicación sea efectiva.
