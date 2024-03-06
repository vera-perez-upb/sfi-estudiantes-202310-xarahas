void task1()
{
    enum class Task1States    {
        CONFIG,
        WAIT_DATA
    };
    static Task1States task1State = Task1States::CONFIG;
    static uint8_t rxData[5];
    static uint8_t TiempoParaAbrir = 5;

    switch (task1State)
    {
    case Task1States::CONFIG:
    {
        char TeclaRecibida;
        
        Serial.begin(115200);
        Serial.print("CONFIG");
        Serial.print("Tiempo para abrir la boveda: " + TiempoParaAbrir);
        Serial.print("Presiona S para aumentar la cantidad de segundos y A para disminuirla")
        
        
        if (Serial.available() > 0)
        {
          TeclaRecibida = Serial.read();
                  
          if (Serial.available() == S)
          {
            TiempoParaAbrir++;
            Serial.print("Tiempo para abrir la boveda: " + TiempoParaAbrir);
          }
          if (Serial.available() == A)
          {
            TiempoParaAbrir--;
            Serial.print("Tiempo para abrir la boveda: " + TiempoParaAbrir);
            
          }

          
        }
