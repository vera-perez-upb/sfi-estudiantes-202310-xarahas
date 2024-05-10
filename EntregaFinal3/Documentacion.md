Proyecto de Comunicación PC-Microcontrolador
Guía detallada sobre cómo realizar un proyecto de comunicación entre una aplicación de PC y un microcontrolador utilizando un protocolo binario. El proyecto incluye la lectura y modificación de variables en el microcontrolador, la implementación de un checksum para verificar la integridad de los datos y la simulación de un caso de error. Además, se aborda la tarea de mantener un LED funcionando a una frecuencia específica como indicador de que la aplicación en el microcontrolador no se bloquea.

Objetivo del Proyecto
El objetivo de este proyecto es establecer una comunicación confiable entre una aplicación de PC y un microcontrolador. La aplicación de PC podrá leer y modificar variables en el microcontrolador utilizando un protocolo binario y verificar la integridad de los datos utilizando un checksum. Además, se debe garantizar que el microcontrolador esté siempre operativo y no se bloquee.

Requisitos
Para llevar a cabo este proyecto, necesitarás lo siguiente:

Un microcontrolador compatible (por ejemplo, Arduino).
Un entorno de desarrollo para programar el microcontrolador (por ejemplo, el IDE de Arduino).
Una computadora con una aplicación de PC que pueda comunicarse a través de un puerto serie con el microcontrolador.
Conocimientos en programación de microcontroladores y en desarrollo de aplicaciones de PC.
Implementación
A continuación, se describen los pasos clave para implementar este proyecto:

1. Comunicación Binaria
Establecer una comunicación binaria entre la aplicación de PC y el microcontrolador. Decide el formato de los mensajes y los bytes que se utilizarán para la transmisión de datos. Asegúrate de que ambos extremos estén configurados para utilizar el mismo protocolo.
2. Lectura de Variables
Desde la aplicación de PC, envía un byte al microcontrolador para solicitar la información de TODAS las variables. El microcontrolador debe responder con la información de todas las variables, que incluye el valor actual, si está habilitada, el intervalo de cambio y el delta de cambio de cada variable.
3. Checksum
Implementa un checksum en el mensaje que envía el microcontrolador al PC. El checksum se calcula a partir de los datos del mensaje y se incluye al final del mensaje. El checksum permite al receptor verificar la integridad de los datos recibidos. Puedes encontrar información en línea sobre cómo calcular el checksum.
4. Modificación de Variables
Desde la aplicación de PC, envía un mensaje al microcontrolador que contiene toda la información de las variables, incluyendo los nuevos valores y deltas. Asegúrate de que el mensaje cumpla con el formato establecido en la comunicación binaria.
5. Simulación de Error
Para simular un caso de error, cambia intencionalmente el valor del checksum en el mensaje enviado desde el microcontrolador a la aplicación de PC. Deberás visualizar el resultado de esta simulación de manera creativa, por ejemplo, mostrando un mensaje de error en la aplicación de PC o activando un indicador visual en el microcontrolador.
6. LED de Verificación
Implementa un LED en el microcontrolador que funcione a una frecuencia de 0.5 Hz. Este LED servirá como un indicador visual de que la aplicación en el microcontrolador está en funcionamiento y no se bloquea. Asegúrate de que el LED parpadee a la frecuencia deseada.
Código de Unity
Este código está diseñado para funcionar con un dispositivo Arduino conectado a Unity a través de un puerto serie. El proyecto parece involucrar el seguimiento y monitoreo de datos de temperatura, presión y altura, además de proporcionar alguna interacción con el usuario a través de la interfaz de Unity

Estructura del Código
El código está estructurado en un script C# para Unity e incluye varias funciones y variables utilizadas para controlar e interactuar con el dispositivo Arduino.

Enumeración TaskState2
TaskState2 es una enumeración que representa los diferentes estados de la aplicación. Estos estados incluyen INIT, WAIT_START, WAIT_EVENTS, CONTADOR_TEMP y GANAR. Cada estado representa una fase diferente de la aplicación.

Variables y Componentes
SerialPort _serialPort: Representa la conexión al puerto serie con el dispositivo Arduino.
TextMeshProUGUI myText: Componente de texto para mostrar un contador en la interfaz de Unity.
TextMeshProUGUI stats: Componente de texto para mostrar los estados de varias variables.
GameObject squareStats: Un GameObject para mostrar estadísticas de variables.
GameObject pantallaFinal y pantallaFinal2: Objetos de juego para mostrar pantallas de victoria y derrota.
GameObject botonesInicio: Objeto de juego para mostrar botones iniciales.
Varias variables de tipo float (temperaturaMax, temperaturaActual, velocidadTemp, alturaMax, alturaActual, velocidadHeight, presionMax, presionActual, velocidadPress) que representan diferentes variables relacionadas con la temperatura, la presión y la altura.
string code: Una variable de cadena para almacenar un código ingresado por el usuario.
string[] dificultad: Un array de cadenas que representa diferentes niveles de dificultad.
int randomDificultad: Un entero para almacenar un nivel de dificultad aleatorio.
Funciones de Botones
Existen varias funciones de botones que manejan las interacciones del usuario con la interfaz de Unity, como iniciar y detener contadores, enviar datos a Arduino y mostrar estadísticas.

ButtonStartCounter(): Inicia el contador.
ButtonStopCounter(): Detiene el contador.
ButtonSeeStats(): Muestra u oculta la visualización de estadísticas.
ButtonPressedSendTemp(): Envía datos de temperatura a Arduino.
ButtonPressedSendPress(): Envía datos de presión a Arduino.
ButtonPressedSendHeight(): Envía datos de altura a Arduino.
ButtonPressedVelTemp(): Envía datos de velocidad para temperatura.
ButtonPressedVelPress(): Envía datos de velocidad para presión.
ButtonPressedVelHeight(): Envía datos de velocidad para altura.
ButtonPressedCode(): Verifica un código ingresado por el usuario y muestra una pantalla de victoria o derrota.
Funciones Start() y Update()
Start(): Inicializa la conexión al puerto serie y los buffers.
Update(): Contiene la lógica principal de la aplicación basada en el estado actual.
Cómo Utilizar
Asegúrate de que un dispositivo Arduino esté conectado al puerto serie especificado (COM3) con una velocidad de baudios de 115200.
Asocia el script de Unity a un GameObject adecuado en tu proyecto de Unity.
Configura las diversas funciones y botones para interactuar con tu dispositivo Arduino según tus necesidades.
Ejecuta la aplicación de Unity y interactúa con el Arduino a través de la interfaz de Unity.
Código de Arduino
El código de Arduino se ha diseñado para interactuar con una aplicación Unity a través de un puerto serie y comunicarse con sensores y actuadores. A continuación, se describen los componentes y las funcionalidades clave del código de Arduino:

Estructura del Código de Arduino
El código de Arduino consiste en una serie de funciones y declaraciones que permiten que la placa Arduino se comunique con la aplicación Unity a través del puerto serie. A continuación, se describen los componentes más relevantes:

Variables y Pines
int val: Una variable que almacena un valor analógico leído de un pin.
int counterTemp, counterPress, counterHeight: Contadores que se utilizan para rastrear eventos y datos de temperatura, presión y altura.
int velTemp, velPress, velHeight: Variables para configurar las velocidades de los contadores.
float temperature, pressure, height: Variables para almacenar datos de temperatura, presión y altura.
int estado = 0: Una variable de estado que indica el estado actual del Arduino.
Configuración de Pines y Comunicación Serie
El código inicia la comunicación serie a una velocidad de baudios de 115200. Además, establece las condiciones iniciales de varias variables, incluyendo valores de temperatura, presión y altura, así como las velocidades de los contadores.

Recepción de Datos desde Unity
El código recibe datos desde la aplicación Unity a través del puerto serie y los utiliza para configurar valores de temperatura, presión, altura y velocidades. También recibe comandos que indican cuándo iniciar el conteo regresivo de cada variable.

Generación de Paquetes de Datos
El código genera paquetes de datos que incluyen información sobre las variables, sus velocidades y un valor de comprobación (checksum) para garantizar la integridad de los datos. Los paquetes de datos se envían a la aplicación Unity a través del puerto serie.

Contadores Regresivos
El código implementa contadores regresivos para cada variable (temperatura, presión, altura) que se activan de acuerdo a la velocidad configurada. Cuando se alcanza el tiempo especificado, se envía un valor a Unity para indicar el cambio en el contador.

Uso
Carga el código en una placa Arduino y asegúrate de que esté conectada al puerto serie.
Inicia la aplicación Unity que se comunica con el Arduino a través del puerto serie.
Utiliza la interfaz de Unity para configurar valores de temperatura, presión, altura y velocidades.
Controla el inicio y el final de los contadores desde la aplicación Unity.
Observa cómo el Arduino envía datos y eventos de contador a la aplicación Unity a través del puerto serie.
Este código de Arduino proporciona una base para la comunicación entre una placa Arduino y una aplicación Unity.