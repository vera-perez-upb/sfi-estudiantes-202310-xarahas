# SOLUCION AL TRAYECTO DE ACTIVIDADES
## EJERCICIO 3
NOMBRE DEL GRUPO: Armony 
INTEGRANTES: 
Samuel Gaviria Agudelo Id:471727
Gustavo Lora Id:501672 (LIDER) 
Sahara Álvarez Romero Id: 448890

Al cambiarse los valores de 100 a 500 la velocidad del led disminuyo

## EJERCICIO 5
Una Raspberry Pi es un pequeño ordenador de placa única (SBC, por sus siglas en inglés) desarrollado por la Fundación Raspberry Pi, aunque parezca una placa arduino, la Raspberry Pi es se considera mas más potente y versátil, ya que funciona como un microordenador y requiere un sistema operativo para ejecutar una variedad de proyectos complejos.

CARACTERISTICAS:
- Microcontrolador RP2040: La Raspberry Pi Pico está impulsada por el microcontrolador RP2040, un componente dual-core ARM Cortex-M0+ diseñado por Raspberry Pi. Este microcontrolador se enfoca en aplicaciones embebidas y de bajo consumo.
-Memoria RAM: Con 264 KB de RAM, la Pico puede almacenar datos temporales y ejecutar programas de manera eficiente.
Pines GPIO (Entrada/Salida de Propósito General): Con 26 pines GPIO, la Pico facilita la conexión a diversos dispositivos y sensores para proyectos electrónicos.
-Entradas/Salidas Analógicas: Ofrece 3 entradas/salidas analógicas (pines ADC), que permite la lectura de señales analógicas útiles en aplicaciones como sensores de luz o temperatura.
-Conectividad: no cuenta con conectividad incorporada, como Wi-Fi o Bluetooth, pero se pueden agregar módulos externos para habilitar estas funciones.
-Programación: Compatible con diversos lenguajes, incluyendo MicroPython y C/C++, a través del SDK de Raspberry Pi Pico. Además, es compatible con herramientas populares como Thonny y entornos de desarrollo integrado (IDE) de C/C++.
-Relación Calidad-Precio: es Conocida por su bajo costo en comparación con otras placas de desarrollo similares, la Raspberry Pi Pico es atractiva para proyectos que requieren un microcontrolador potente y barato.
-Soporte de Comunidad: Al igual que otros productos de Raspberry Pi, la Pico se beneficia de una comunidad activa en línea, que comparte recursos como tutoriales, proyectos y foros de discusión.
Otras Características Relevantes:
-Adaptador USB incorporado: La Pico cuenta con un adaptador USB incorporado, simplificando la conexión, la programación y la alimentación.

## LINK EJERCICIO 4

[LINK DEL REPOSITORIO INDIVIDUAL](https://github.com/xarahas/Repositorio-Individual-.git)


##EJERCICIO 6
- ¿Cómo se ejecuta este programa?
El programa sigue un patrón de máquina de estados, realizando diversas acciones dependiendo del estado actual, y esto se repite en un bucle principal.
El programa comienza estableciendo la comunicación serial y configurando un estado inicial llamado INIT, entrando en un bucle principal (loop()) donde llama repetidamente a la función task1().
La función task1() realiza acciones diferentes según el estado actual. En el estado INIT, inicia la comunicación serial y luego cambia al estado WAIT_TIMEOUT, en el que verifica si ha pasado un cierto intervalo de tiempo y realiza acciones en consecuencia.
  
- Pudiste ver este mensaje: `Serial.print("Task1States::WAIT_TIMEOUT\n");`. ¿Por qué crees que ocurre esto?
No se pudo ver el mensaje porque la instruccion de imprimir el texto, esta despues de la instruccion del cambio de estado de maquina, por lo que ola maquina cambia de estado despues de imprimir el mensaje, pero agregando un "DELAY" y moviendo la instruccion arriba del cambio de estado permite la microcontrolador procesar e imprimir el mensaje correctamente.
  
- ¿Cuántas veces se ejecuta el código en el case Task1States::INIT?
En el código proporcionado, el código dentro del caso `Task1States::INIT` se ejecuta una vez al inicio del programa,posteriormente, se cambia al estado  `Task1States::WAIT_TIMEOUT`, y se queda en bucle en ese estado.


## EJERCICIO 7
Observa la función millis(); ¿Para qué sirve? Recuerda que puedes buscar en Internet.

La funcion millis(); sirve para devolver el numero de milisegundos transcurridos desde que el programa comienza a ejecutarse, es útil para medir el tiempo entre eventos, realizar acciones periódicas o implementar temporizadores en programas para plataformas Arduino.

## EJERCICIO 8
```
void task1()
{
    // Definición de estados y variable de estado
    enum class Task1States
    {
        INIT,
        WAIT_TIMEOUT1,
        WAIT_TIMEOUT2,
        WAIT_TIMEOUT3
    };
    static Task1States task1State = Task1States::INIT;

    // Definición de variables static (conservan su valor entre llamadas a task1)
    static uint32_t lastTime = 0;

    // Constantes
    constexpr uint32_t INTERVAL1 = 1000;
    constexpr uint32_t INTERVAL2 = 2000;
    constexpr uint32_t INTERVAL3 = 3000;

    // MÁQUINA de ESTADOS
    switch (task1State)
    {
    case Task1States::INIT:
    {
        // Acciones:
        Serial.begin(115200);

        // Garantiza los valores iniciales para el siguiente estado.
        lastTime = millis();
        task1State = Task1States::WAIT_TIMEOUT1;

        break;
    }
    case Task1States::WAIT_TIMEOUT1:
    {
        uint32_t currentTime = millis();

        // Evento
        if ((currentTime - lastTime) >= INTERVAL1)
        {
            // Acciones:
            lastTime = currentTime;
            Serial.print(currentTime);
            Serial.print("hola este es el primer mensaje");
            Serial.print('\n');
            task1State = Task1States::WAIT_TIMEOUT2;
        }
        break;
    }


    case Task1States::WAIT_TIMEOUT2:
    {

      uint32_t currentTime = millis();
  
          // Evento
          if ((currentTime - lastTime) >= INTERVAL2)
          {
              // Acciones:
              lastTime = currentTime;
              Serial.print(currentTime);
              Serial.print("hola este es el segundo mensaje");
              Serial.print('\n');
              task1State = Task1States::WAIT_TIMEOUT3;
          }
          break;
    }
      



    case Task1States::WAIT_TIMEOUT3:
    {

      uint32_t currentTime = millis();
  
          // Evento
          if ((currentTime - lastTime) >= INTERVAL3)
          {
              // Acciones:
              lastTime = currentTime;
              Serial.print(currentTime);
              Serial.print("hola este es el 3 mensaje");
              Serial.print('\n');
              task1State = Task1States::WAIT_TIMEOUT1;
              
              
          }
          break;
      
    }
    
    
    default:
    {
        Serial.println("Error");
    }
    }
}

void setup()
{
    task1();
}

void loop()
{
    task1();
}

```
- ¿Cuáles son los estados del programa?
INIT: Estado inicial donde se configuran las condiciones iniciales y se ejecutan acciones de inicio, como la inicialización de la comunicación serial y la configuración de valores iniciales.

WAIT_TIMEOUT1: Estado en el que se espera que pase un intervalo de tiempo INTERVAL1 antes de realizar ciertas acciones y transicionar al siguiente estado.

WAIT_TIMEOUT2: Similar a WAIT_TIMEOUT1, pero con un intervalo de tiempo INTERVAL2.

WAIT_TIMEOUT3: Similar a WAIT_TIMEOUT1 y WAIT_TIMEOUT2, pero con un intervalo de tiempo INTERVAL3.
  
- ¿Cuáles son los eventos?
Tiempo transcurrido (timeout): El evento que desencadena la transición de un estado a otro es el tiempo transcurrido desde la última vez que se realizó cierta acción (lastTime). Este evento se verifica en cada estado y está basado en comparaciones de tiempo utilizando la función millis().
  
- ¿Cuáles son las acciones?
  INIT:Inicialización de la comunicación serial.
       Establecimiento de valores iniciales (lastTime).
       Transición al estado WAIT_TIMEOUT1.
  WAIT_TIMEOUT1:
       Verificación del evento de tiempo transcurrido (INTERVAL1).
       Impresión de un mensaje en la comunicación serial.
       Actualización del tiempo de referencia (lastTime).
       Transición al estado WAIT_TIMEOUT2.
WAIT_TIMEOUT2:
      Verificación del evento de tiempo transcurrido (INTERVAL2).
      Impresión de otro mensaje en la comunicación serial.
      Actualización del tiempo de referencia (lastTime).
      Transición al estado WAIT_TIMEOUT3.
WAIT_TIMEOUT3:
      Verificación del evento de tiempo transcurrido (INTERVAL3).
      Impresión de un tercer mensaje en la comunicación serial.
      Actualización del tiempo de referencia (lastTime).
      Transición al estado WAIT_TIMEOUT1 para reiniciar el ciclo.
  
