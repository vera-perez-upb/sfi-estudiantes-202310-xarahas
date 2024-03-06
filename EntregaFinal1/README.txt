enum class Task1States {
    CONFIG,
    WAIT_DATA
};

static Task1States task1State = Task1States::CONFIG;
static uint8_t TiempoParaAbrir = 5;

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
        Serial.begin(115200);
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
                task1State = Task1States::WAIT_DATA;
            }
        }

        break;
    }

    case Task1States::WAIT_DATA:
    {
        // Evento 1:

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
