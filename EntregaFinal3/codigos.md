CODIGO ARDUINO  
```
unsigned long previousMillis = 0;
const long interval = 1000; // Intervalo de 1 segundo (en milisegundos)

   float variable1 = 20.0;
   float variable2 = 10.0;
   float deltaChangue = 2.0;


void setup()
{
  Serial.begin(115200);
    adc_init();
  adc_set_temp_sensor_enabled(true);
  
}

void loop()
{


   uint8_t arr[4] = {0};
   static bool variable1Habilitada = true;
   static bool variable2Habilitada = true;
   static bool temperaturaHabilitada = true;
   float variable1HabilitadaNumero;
   float variable2HabilitadaNumero;
  float temperaturaHabilitadaNumero;
   float temp = 0.0;

     adc_select_input(4);
      uint16_t result = adc_read();
        float voltage = result * (3.3f /4096);
        if (temperaturaHabilitada == true)
        {
         temp = 27 - (voltage - 0.706) / 0.001721;
        }
        else
        {
          
        }


   //-------------------     
    if(variable1Habilitada == true)
    {
      variable1HabilitadaNumero = 1.0;
    }
    else
    {
       variable1HabilitadaNumero = 0.0;

    }
    //--------------
    
     if(variable2Habilitada == true)
    {
      variable2HabilitadaNumero = 1.0;
    }
    else
    {
       variable2HabilitadaNumero = 0.0;

    }
    //--------------------
         if(temperaturaHabilitada == true)
    {
      temperaturaHabilitadaNumero = 1.0;
    }
    else
    {
       temperaturaHabilitadaNumero = 0.0;

    }
        
 unsigned long currentMillis = millis();
    if (currentMillis - previousMillis >= interval) {
        previousMillis = currentMillis;
        if (variable1Habilitada == true)
        {
         variable1 -= 2.0;
        }

            if (variable2Habilitada == true)
        {
         variable2 -= 2.0;
        }
   
        
        
    }

    
    //logica para el envio de datos
    char TeclaRecibida;
        
        if (Serial.available() > 0)
        {
            TeclaRecibida = Serial.read();

           if (TeclaRecibida == '1')
            {
                memcpy(arr,(uint8_t *)&variable1,4);
                for(int8_t i = 0; i < 4; i++)
                {
                  Serial.write(arr[i]);
                }

                memcpy(arr,(uint8_t *)&variable2,4);
                for(int8_t i = 0; i < 4; i++)
                  Serial.write(arr[i]);


                memcpy(arr,(uint8_t *)&temp,4);
                for(int8_t i = 0; i < 4; i++)
                {
                  Serial.write(arr[i]);
                }
                
                memcpy(arr,(uint8_t *)&variable1HabilitadaNumero,4);
                for(int8_t i = 0; i < 4; i++)
                {
                  Serial.write(arr[i]);
                }

                       memcpy(arr,(uint8_t *)&variable2HabilitadaNumero,4);
                for(int8_t i = 0; i < 4; i++)
                {
                  Serial.write(arr[i]);
                }
                       memcpy(arr,(uint8_t *)&temperaturaHabilitadaNumero,4);
                for(int8_t i = 0; i < 4; i++)
                {
                  Serial.write(arr[i]);
                }

                                       memcpy(arr,(uint8_t *)&deltaChangue,4);
                for(int8_t i = 0; i < 4; i++)
                {
                  Serial.write(arr[i]);
                }

 
  
          
            }

            if (TeclaRecibida == '2')
            {
              variable1Habilitada = true;
            }
            
            if (TeclaRecibida == '3')
            {
              variable2Habilitada = true;
            }
            
            if (TeclaRecibida == '4')
            {
              temperaturaHabilitada = true;                            
            }

            if (TeclaRecibida == '5')
            {
              variable1Habilitada = false;
            }
            
            if (TeclaRecibida == '6')
            {
              variable2Habilitada = false;
            }
            
            if (TeclaRecibida == '7')
            {
              temperaturaHabilitada = false;                            
            }
  
       }
}

```



CODIGO UNITY
```

using UnityEngine;
using System.IO.Ports;
using TMPro;
using UnityEngine.UI;
using System;


public class UTPTPA : MonoBehaviour
{
    private SerialPort _serialPort = new SerialPort();

    public TextMeshProUGUI variable1Texto;
    public TextMeshProUGUI variable2Texto;
    public TextMeshProUGUI temperaturaTexto;

    public TextMeshProUGUI habilitadovariable1Texto;
    public TextMeshProUGUI habilitadovariable2Texto;
    public TextMeshProUGUI habilitadotemperaturaTexto;
    public TextMeshProUGUI deltaChangueTexto;

    public float habilitadovariable1;
    public float habilitadovariable2;
    public float habilitadotemperatura;
    public float deltaChangue;
    void Start()
    {
        _serialPort.PortName = "COM4";
        _serialPort.BaudRate = 115200;

        _serialPort.DtrEnable = true;
        _serialPort.Open();
        Debug.Log("Puerto serial listo");
    }

    
    void Update()
    {
        if (Input.GetKeyDown(KeyCode.S))
        {
            byte[] data = { 0x31 };
            _serialPort.Write(data, 0, 1);
            byte[] buffer = new byte[28];
            Debug.Log("S");
            if (_serialPort.BytesToRead >= 4)
            {
                _serialPort.Read(buffer, 0, 28);

            variable1Texto.SetText("");
            variable2Texto.SetText("");
            temperaturaTexto.SetText("");
            
           float variable1 = BitConverter.ToSingle(buffer, 0);
            float variable2 = BitConverter.ToSingle(buffer, 4);
            float temperatura = BitConverter.ToSingle(buffer, 8);

            variable1Texto.text = variable1.ToString();
            variable2Texto.text = variable2.ToString();
            temperaturaTexto.text = temperatura.ToString();

            habilitadovariable1Texto.SetText("");
            habilitadovariable2Texto.SetText("");
            habilitadotemperaturaTexto.SetText("");
            deltaChangueTexto.SetText("");
            
             habilitadovariable1 = BitConverter.ToSingle(buffer, 12);
             habilitadovariable2 = BitConverter.ToSingle(buffer, 16);
             habilitadotemperatura = BitConverter.ToSingle(buffer, 20);
             deltaChangue = BitConverter.ToSingle(buffer, 24);

            habilitadovariable1Texto.text = habilitadovariable1.ToString();
            habilitadovariable2Texto.text = habilitadovariable2.ToString();
            habilitadotemperaturaTexto.text = habilitadotemperatura.ToString();
            deltaChangueTexto.text = deltaChangue.ToString();
            }
            
        }

        

        if (Input.GetKeyDown(KeyCode.Q))
        {
            byte[] data = { 0x32 };
            _serialPort.Write(data, 0, 1);
        }

        
        if (Input.GetKeyDown(KeyCode.W))
        {
            byte[] data = { 0x33 };
            _serialPort.Write(data, 0, 1);
        }

        
        if (Input.GetKeyDown(KeyCode.E))
        {
            byte[] data = { 0x34 };
            _serialPort.Write(data, 0, 1);
        }

        if (Input.GetKeyDown(KeyCode.R))
        {
            byte[] data = { 0x35 };
            _serialPort.Write(data, 0, 1);
        }

        if (Input.GetKeyDown(KeyCode.T))
        {
            byte[] data = { 0x36 };
            _serialPort.Write(data, 0, 1);
        }

        if (Input.GetKeyDown(KeyCode.Y))
        {
            byte[] data = { 0x37 };
            _serialPort.Write(data, 0, 1);
        }
    }
}
```
