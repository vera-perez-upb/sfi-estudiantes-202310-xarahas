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
