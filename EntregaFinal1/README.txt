#TRABAJO FINAL U1
#Diagrama de estados 

https://web.whatsapp.com/765d77f4-8c5e-4136-a30d-19d8d6bf36bd 

```
enum class Task1States {
    MENSAJEINICIAL,
    CONFIG,
    PROCESO,
    FINAL
};

static Task1States task1State = Task1States::MENSAJEINICIAL;
static uint8_t TiempoParaAbrir = 5;
static uint32_t lastTime;

void task1()
{
    switch (task1State)
    {
      case Task1States::MENSAJEINICIAL:
    {

              Serial.print("CONFIG - Tiempo para abrir la boveda: ");
              Serial.println(TiempoParaAbrir);
              Serial.println("Oye soldado, los Disidentes del tiempo han tomado control de la central nuclear y planean activar la radiacion que destruira a toda la ciudad, despues de que varios agentes de nuestra resistencia murieron, te hemos asignado esta mision para que desactives la camara");
                            Serial.println("debes desactivar la camara de radiacion que se abrira dentro de poco, para ello deberas ingresar una clave secreta compuesta por el formato C0000, si no logras decifrar la clave el mundo se destruira para siempre");
                                            delay(1000);
                              task1State = Task1States::CONFIG;

             
    
    
    }
    case Task1States::CONFIG:
    {
  
      char TeclaRecibida;





        
        if (Serial.available() > 0)
        {
    
            TeclaRecibida = Serial.read();

            if (TeclaRecibida != 'L')
            {
                if (TeclaRecibida == 'S' & TiempoParaAbrir<40 )
                {
                    TiempoParaAbrir++;
                    Serial.println("Tiempo para abrir la boveda: " + String(TiempoParaAbrir));
                }
                else if (TeclaRecibida == 'B' & TiempoParaAbrir>1 )
                {
                    TiempoParaAbrir--;
                    Serial.println("Tiempo para abrir la boveda: " + String(TiempoParaAbrir));
                }
            }
            else
            {
                Serial.println("Listo");
                task1State = Task1States::PROCESO;
            }
        }

        break;
    }

   case /O:
{
    lastTime = TiempoParaAbrir;

    while (lastTime > 0)
    {
        Serial.println(lastTime);

        // Verificar si hay datos disponibles en el puerto serial
        if (Serial.available() > 0)
        {
            char inputChar = Serial.read();

            // Verificar si el primer carácter es 'C'
            if (inputChar == 'C')
            {
                // Leer los siguientes cuatro caracteres como números
                char code[4];
                for (int i = 0; i < 4; ++i)
                {
                    if (Serial.available() > 0)
                    {
                        code[i] = Serial.read();
                    }
                    else
                    {

                    }
                }


                int enteredCode = atoi(code);

                if (enteredCode == 1234)
                {
                    Serial.println("¡Salvaste al mundo!");

                    task1State = Task1States::FINAL;
                    break;  
                }
                else
                {
                    Serial.println("Contraseña incorrecta");
                }
            }
        }

        lastTime--;
        delay(1000);
    }

    // Verificar si la cuenta regresiva ha terminado
    if (lastTime <= 0)
    {
        Serial.println("Se acabó el tiempo, RADIACION NUCLEAR ACTIVA");
        task1State = Task1States::FINAL;
    }

    break;
}

        case Task1States::FINAL:
    {
   
       

        TiempoParaAbrir = 5;
        lastTime = 0;
        task1State = Task1States::MENSAJEINICIAL;

        break;
    }
    

    

    default:
    {
        break;
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
    // Aquí puedes agregar otras tareas que se ejecuten en el bucle principal si es necesario.
}
```
