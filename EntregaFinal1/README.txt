 enum class Task1States {
    CONFIG,
    PROCESO,
    FINAL
};

static Task1States task1State = Task1States::CONFIG;
static uint8_t TiempoParaAbrir = 5;
static uint32_t lastTime;

void task1()
{
    switch (task1State)
    {
    case Task1States::CONFIG:
    {
        char TeclaRecibida;

    
              /*Serial.print("CONFIG - Tiempo para abrir la boveda: ");
              Serial.println(TiempoParaAbrir);
              Serial.println("Presiona S para aumentar la cantidad de segundos y A para disminuirla, Presiona L para aceptar los cambios");
              */




        
        if (Serial.available() > 0)
        {
    
            TeclaRecibida = Serial.read();

            if (TeclaRecibida != 'L')
            {
                if (TeclaRecibida == 'S')
                {
                    TiempoParaAbrir++;
                    Serial.println("Tiempo para abrir la boveda: " + String(TiempoParaAbrir));
                }
                else if (TeclaRecibida == 'A')
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

   case Task1States::PROCESO:
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
        task1State = Task1States::CONFIG;

        break;
    } 
