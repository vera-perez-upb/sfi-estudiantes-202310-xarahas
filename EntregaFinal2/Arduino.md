//Arduino Code
void loop()
{
    task1(); 
}

enum class Task1States {
    CONFIG,
    FINAL,
    OxigenoProceso
};

static Task1States task1State = Task1States::CONFIG;
static uint8_t TiempoParaAbrir = 120;
static uint32_t lastTime;
static uint32_t lastTimeOxigeno;
static uint32_t Temperatura;
static uint32_t VelocidadConsumoTemperatura = 1;
static uint32_t Refrigeracion = 1;
static uint32_t VelocidadConsumoRefrigeracion = 1;
static uint32_t Oxigeno = 1;
static uint32_t VelocidadConsumoOxigeno = 1;
static bool oxigenoHabilitado = true;
static bool refrigeracionHabilitada = true;
static bool temperaturaHabilitada = true;

void task1()
{
    switch (task1State)
    {
    case Task1States::CONFIG:
    {
  
      char TeclaRecibida;
        
        if (Serial.available() > 0)
        {
    
            TeclaRecibida = Serial.read();

                    if (TeclaRecibida == 'O')
        {
            oxigenoHabilitado = !oxigenoHabilitado; // Cambiar el estado de habilitación
            Serial.print("Oxigeno habilitado: ");
            Serial.println(oxigenoHabilitado ? "Si" : "No");
        }
                // Habilitar/deshabilitar refrigeración (tecla A)
        if (TeclaRecibida == 'P')
        {
            refrigeracionHabilitada = !refrigeracionHabilitada; // Cambiar el estado de habilitación
            Serial.print("Refrigeracion habilitada: ");
            Serial.println(refrigeracionHabilitada ? "Si" : "No");
        }
        
        // Habilitar/deshabilitar temperatura (tecla P)
        if (TeclaRecibida == 'A')
        {
            temperaturaHabilitada = !temperaturaHabilitada; // Cambiar el estado de habilitación
            Serial.print("Temperatura habilitada: ");
            Serial.println(temperaturaHabilitada ? "Si" : "No");
        }
        



              // ----------------------------INICIO MANEJO OXIGENO----------------------

               if (oxigenoHabilitado)
                 {
                  if (TeclaRecibida == 'D')
                  {
                      VelocidadConsumoOxigeno++;
                      Serial.println(VelocidadConsumoOxigeno);

                  }
                  
                  if (TeclaRecibida == 'F')
                  {
                    if (VelocidadConsumoOxigeno > 0)
                    {
                      VelocidadConsumoOxigeno--;
                      Serial.println(VelocidadConsumoOxigeno);
                    }
                  }
                  
                  if (TeclaRecibida == 'G')
                  {
                      Oxigeno++;
                      Serial.println(Oxigeno);
                  }
                  
                  if (TeclaRecibida == 'H')
                  {
                    if (Oxigeno > 0)
                    {
                      Oxigeno--;
                      Serial.println(Oxigeno);
                    }
                  }
                 }
                //----------------FIN MANEJO OXIGENO ---------------------

                // ---------------- INICIO MANEJO REFRIGERACION -------------------
         if (refrigeracionHabilitada)
        {
                   if (TeclaRecibida == 'Q')
                  {
                      VelocidadConsumoRefrigeracion++;
                      Serial.println(VelocidadConsumoRefrigeracion);

                  }
                  
                  if (TeclaRecibida == 'W')
                  {
                    if (VelocidadConsumoRefrigeracion > 0)
                    {
                      VelocidadConsumoRefrigeracion--;
                      Serial.println(VelocidadConsumoRefrigeracion);
                    }
                  }
                  
                  if (TeclaRecibida == 'E')
                  {
                      Refrigeracion++;
                      Serial.println(Refrigeracion);
                  }
                  
                  if (TeclaRecibida == 'R')
                  {
                    if (Refrigeracion > 0)
                    {
                      Refrigeracion--;
                      Serial.println(Refrigeracion);
                    }
                  }
        }
                // -----------------FIN MANEJO REFRIGERACION------------------------------------

                //----------------- INICIO MANEJO TEMPERATURA -------------------------------------
        if (temperaturaHabilitada)
        {
                if (TeclaRecibida == 'T')
                  {
                      VelocidadConsumoTemperatura++;
                      Serial.println(VelocidadConsumoTemperatura);

                  }
                  
                  if (TeclaRecibida == 'Y')
                  {
                    if (VelocidadConsumoTemperatura > 0)
                    {
                      VelocidadConsumoTemperatura--;
                      Serial.println(VelocidadConsumoTemperatura);
                    }
                  }
                  
                  if (TeclaRecibida == 'U')
                  {
                      Temperatura++;
                      Serial.println(Temperatura);
                  }
                  
                  if (TeclaRecibida == 'I')
                  {
                    if (Temperatura > 0)
                    {
                      Temperatura--;
                      Serial.println(Temperatura);
                    }
                  }
        }
                //---------------------FIN MANEJO TEMPERATURA -----------------
                
                  if (TeclaRecibida == 'J')
                  {
                  Serial.println("HOLA, PASANDO AL JUEGO");
                  task1State = Task1States::OxigenoProceso;
                  }

        }

        break;
    }


   case Task1States::OxigenoProceso:
{

    static unsigned long lastUpdateTime = 0; // Variable para almacenar el tiempo del último update
    static unsigned long updateInterval = 1000; // Intervalo de actualización en milisegundos (1 segundo)

    // Bucle mientras haya oxígeno, temperatura y refrigeración
    while (Oxigeno > 0 || Temperatura > 0 || Refrigeracion > 0)
    {
        unsigned long currentTime = millis(); // Obtener el tiempo actual

        // Verificar si ha pasado el tiempo suficiente para actualizar
        if (currentTime - lastUpdateTime >= updateInterval)
        {
            lastUpdateTime = currentTime; // Actualizar el tiempo del último update

            // Reducir oxígeno, temperatura y refrigeración según la velocidad de consumo
            if (Oxigeno > 0) {
                Oxigeno -= VelocidadConsumoOxigeno;
            }
            if (Refrigeracion > 0) {
                Refrigeracion -= VelocidadConsumoRefrigeracion;
            }
            if (Temperatura > 0) {
                Temperatura -= VelocidadConsumoTemperatura;
            }

            // Enviar datos a Unity
            Serial.print("B");
            Serial.println(Oxigeno);
            Serial.print("N");
            Serial.println(Refrigeracion);
            Serial.print("M");
            Serial.println(Temperatura);
        }
    }

    // Comprobar si se agotó alguna de las variables
    if (Oxigeno <= 0 && Temperatura <= 0 && Refrigeracion <= 0)
    {
        Serial.println("Se acabó el oxígeno, temperatura o refrigeración, MORISTE");
        task1State = Task1States::FINAL;
    }

    break;
}


   
 

        case Task1States::FINAL:
    {
   
       

        TiempoParaAbrir = 120;
        lastTime = 0;
        lastTimeOxigeno  = 0;
        Temperatura = 0;
        Refrigeracion = 0;
        Oxigeno = 0;
        VelocidadConsumoOxigeno = 0;


        VelocidadConsumoTemperatura = 1;
        
        VelocidadConsumoRefrigeracion = 1;
        
        oxigenoHabilitado = true;
        refrigeracionHabilitada = true;
        temperaturaHabilitada = true;
        task1State = Task1States::CONFIG;

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
