unity: using UnityEngine;
using System.IO.Ports;
using TMPro;
using UnityEngine.UI;
public class Serial : MonoBehaviour

{
private SerialPort _serialPort =new SerialPort();
private byte[] buffer =new byte[100];

public TextMeshProUGUI VelocidadConsumoOxigeno;
public TextMeshProUGUI Oxigeno;
public TextMeshProUGUI OxigenoInGame;
public TextMeshProUGUI VelocidadConsumoRefrigeracion;
public TextMeshProUGUI Refrigeracion;
public TextMeshProUGUI RefrigeracionInGame;

public TextMeshProUGUI VelocidadConsumoTemperatura;
public TextMeshProUGUI Temperatura;
public TextMeshProUGUI TemperaturaInGame;


private int oxygenConsumptionRate;
private int oxygenRate;

private int oxygenRateInGame;
private int RefrigerationRateInGame;
private int TemperatureRateInGame;


private int RefrigerationConsumptionRate;
private int RefrigerationRate;

private int TemperatureConsumptionRate;
private int TemperatureRate;

public InputField inputField;

public GameObject PantallaJuego;
public GameObject PantallaDeDerrota;
public GameObject PantallaDeVictoria;


void Start()
    {
        _serialPort.PortName = "COM4";
        _serialPort.BaudRate = 115200;
        _serialPort.DtrEnable =true;
        _serialPort.Open();
        Debug.Log("Open Serial Port");
    }





    // Método que se llama cuando se presiona un botón para verificar el texto del InputField
    public void VerificarTexto()
    {
        // Accede al texto del InputField usando su propiedad text
        string textoIngresado = inputField.text;

        // Comprueba si el texto ingresado es igual a "C1234"
        if (textoIngresado == "C1234")
        {
            Debug.Log("Correcto");
            PantallaDeVictoria.SetActive(true);
        }
        else
        {
            Debug.Log("Incorrecto");
        }
    }


void Update()
{

    if (_serialPort.BytesToRead > 0)
    {
        int bytesAvailable = _serialPort.BytesToRead;
        int numData = _serialPort.Read(buffer, 0, bytesAvailable);
        string receivedData = System.Text.Encoding.ASCII.GetString(buffer, 0, numData);

        // Separa los datos según el carácter especial que los precede
        string[] dataSplit = receivedData.Split('\n');

        foreach (string data in dataSplit)
        {
            if (data.Length > 0)
            {
                // Identifica el tipo de dato por el primer carácter
                char dataType = data[0];
                // Obtiene el valor del dato eliminando el primer carácter
                if (int.TryParse(data.Substring(1), out int value))
                {
                    // Actualiza el valor correspondiente según el tipo de dato
                    switch (dataType)
                    {
                        case 'B':
                            oxygenRateInGame = value;
                            Debug.Log("Oxygen Rate: " + oxygenRateInGame);
                            break;

                        case 'N':
                            RefrigerationRateInGame = value;
                            Debug.Log("Refrigeration Rate: " + RefrigerationRateInGame);
                            break;

                        case 'M':
                            TemperatureRateInGame = value;
                            Debug.Log("Temperature Rate: " + TemperatureRateInGame);
                            break;

                        default:
                            Debug.Log("Invalid data type: " + dataType);
                            break;
                    }
                }
                else
                {
                    Debug.Log("Failed to parse data value as integer: " + data);
                }
            }
        }

              if (oxygenRateInGame <= 0)
    {

    }
        if (TemperatureRateInGame <= 0)
    {

    }
        if (RefrigerationRateInGame <= 0)
    {


    }

    if (oxygenRateInGame <= 0 && TemperatureRateInGame <= 0 && RefrigerationRateInGame <= 0)
{
    
                if (PantallaJuego.activeSelf)
                {
                    if (!PantallaDeVictoria.activeSelf)
                        PantallaDeDerrota.SetActive(true);
                }

}
    }

    // Actualiza los textos en Unity solo cuando se reciben nuevos datos
    OxigenoInGame.text = oxygenRateInGame.ToString();
    TemperaturaInGame.text = TemperatureRateInGame.ToString();
    RefrigeracionInGame.text = RefrigerationRateInGame.ToString();
}



// ------------------INICIO MANEJO OXIGENO-------------------------
public void AumentarVelocidadConsumoOxigeno()
{

    

        byte[] data = { 0x44 }; // Envía el carácter 'D'
        _serialPort.Write(data, 0, 1);
        Debug.Log("Send Data");
         if (_serialPort.BytesToRead > 0)
        {
            int bytesAvailable = _serialPort.BytesToRead;
            
            // Lee los bytes disponibles en el búfer
            int numData = _serialPort.Read(buffer, 0, bytesAvailable);
            
            // Convierte los bytes leídos en una cadena y muestra en el log
            string receivedData = System.Text.Encoding.ASCII.GetString(buffer, 0, numData);
            Debug.Log("Received Data: " + receivedData);
            
            // Parsea la cadena recibida para obtener el valor numérico

            if (int.TryParse(receivedData, out oxygenConsumptionRate))
            {
                // Ahora 'oxygenConsumptionRate' contiene el valor numérico de la velocidad de consumo de oxígeno
                // Puedes usar este valor para manejar la barra de vida u otra funcionalidad en Unity
                Debug.Log("Oxygen Consumption Rate: " + oxygenConsumptionRate);
            }
            else
            {
                Debug.Log("Failed to parse received data as integer.");
            }
            
            // Opcional: puedes mostrar el número de bytes recibidos en el log
            Debug.Log("Bytes received: " + numData.ToString());
        }
        else
        {
            Debug.Log("No data available to read.");
        }

        VelocidadConsumoOxigeno.text = oxygenConsumptionRate.ToString();
    
    
}

public void DisminuirVelocidadConsumoOxigeno()
{

    

        byte[] data = { 0x46}; 
        _serialPort.Write(data, 0, 1);
        Debug.Log("Send Data");
    


        if (_serialPort.BytesToRead > 0)
        {
            int bytesAvailable = _serialPort.BytesToRead;

            int numData = _serialPort.Read(buffer, 0, bytesAvailable);
            
            string receivedData = System.Text.Encoding.ASCII.GetString(buffer, 0, numData);
            Debug.Log("Received Data: " + receivedData);

            if (int.TryParse(receivedData, out oxygenConsumptionRate))
            {
                Debug.Log("Oxygen Consumption Rate: " + oxygenConsumptionRate);
            }
            else
            {
                Debug.Log("Failed to parse received data as integer.");
            }

            Debug.Log("Bytes received: " + numData.ToString());
        }
        else
        {
            Debug.Log("No data available to read.");
        }

        VelocidadConsumoOxigeno.text = oxygenConsumptionRate.ToString();
    
    
}

public void AumentarOxigeno()
{

    

        byte[] data = { 0x47}; 
        _serialPort.Write(data, 0, 1);
        Debug.Log("Send Data");
    


        if (_serialPort.BytesToRead > 0)
        {

            int bytesAvailable = _serialPort.BytesToRead;
            
            int numData = _serialPort.Read(buffer, 0, bytesAvailable);

            string receivedData = System.Text.Encoding.ASCII.GetString(buffer, 0, numData);
            Debug.Log("Received Data: " + receivedData);
            


            if (int.TryParse(receivedData, out oxygenRate))
            {

                Debug.Log("Oxygen  Rate: " + oxygenRate);
            }
            else
            {
                Debug.Log("Failed to parse received data as integer.");
            }
            

            Debug.Log("Bytes received: " + numData.ToString());
        }
        else
        {
            Debug.Log("No data available to read.");
        }

        Oxigeno.text = oxygenRate.ToString();
    
    
}

public void DisminuirOxigeno()
{

    

        byte[] data = { 0x48}; 
        _serialPort.Write(data, 0, 1);
        Debug.Log("Send Data");
    


        if (_serialPort.BytesToRead > 0)
        {
 
            int bytesAvailable = _serialPort.BytesToRead;
            

            int numData = _serialPort.Read(buffer, 0, bytesAvailable);
            

            string receivedData = System.Text.Encoding.ASCII.GetString(buffer, 0, numData);
            Debug.Log("Received Data: " + receivedData);
            


            if (int.TryParse(receivedData, out oxygenRate))
            {

                Debug.Log("Oxygen  Rate: " + oxygenRate);
            }
            else
            {
                Debug.Log("Failed to parse received data as integer.");
            }
            

            Debug.Log("Bytes received: " + numData.ToString());
        }
        else
        {
            Debug.Log("No data available to read.");
        }

        Oxigeno.text = oxygenRate.ToString();
    
    
}

//------------------------- FIN MANEJO OXIGENO ------------------------------------

//--------------------------INICIO MANEJO REFRIGERACION--------------------------------

public void AumentarVelocidadConsumoRefrigeracion()
{

    

        byte[] data = { 0x51 }; 
        _serialPort.Write(data, 0, 1);
        Debug.Log("Send Data");
         if (_serialPort.BytesToRead > 0)
        {
            int bytesAvailable = _serialPort.BytesToRead;

            int numData = _serialPort.Read(buffer, 0, bytesAvailable);

            string receivedData = System.Text.Encoding.ASCII.GetString(buffer, 0, numData);
            Debug.Log("Received Data: " + receivedData);
            

            if (int.TryParse(receivedData, out RefrigerationConsumptionRate))
            {

                Debug.Log("Refrigeration Consumption Rate: " + RefrigerationConsumptionRate);
            }
            else
            {
                Debug.Log("Failed to parse received data as integer.");
            }
            

            Debug.Log("Bytes received: " + numData.ToString());
        }
        else
        {
            Debug.Log("No data available to read.");
        }

        VelocidadConsumoRefrigeracion.text = RefrigerationConsumptionRate.ToString();
    
    
}

public void DisminuirVelocidadConsumoRefrigeracion()
{

    

        byte[] data = { 0x57}; 
        _serialPort.Write(data, 0, 1);
        Debug.Log("Send Data");
    


        if (_serialPort.BytesToRead > 0)
        {
            int bytesAvailable = _serialPort.BytesToRead;

            int numData = _serialPort.Read(buffer, 0, bytesAvailable);
            
            string receivedData = System.Text.Encoding.ASCII.GetString(buffer, 0, numData);
            Debug.Log("Received Data: " + receivedData);

            if (int.TryParse(receivedData, out RefrigerationConsumptionRate))
            {
                Debug.Log("Refrigeration Consumption Rate: " + RefrigerationConsumptionRate);
            }
            else
            {
                Debug.Log("Failed to parse received data as integer.");
            }

            Debug.Log("Bytes received: " + numData.ToString());
        }
        else
        {
            Debug.Log("No data available to read.");
        }

        VelocidadConsumoRefrigeracion.text = RefrigerationConsumptionRate.ToString();
    
    
}

public void AumentarRefrigeracion()
{

    

        byte[] data = { 0x45}; 
        _serialPort.Write(data, 0, 1);
        Debug.Log("Send Data");
    


        if (_serialPort.BytesToRead > 0)
        {

            int bytesAvailable = _serialPort.BytesToRead;
            
            int numData = _serialPort.Read(buffer, 0, bytesAvailable);

            string receivedData = System.Text.Encoding.ASCII.GetString(buffer, 0, numData);
            Debug.Log("Received Data: " + receivedData);
            


            if (int.TryParse(receivedData, out RefrigerationRate))
            {

                Debug.Log("Refrigeration Rate: " + RefrigerationRate);
            }
            else
            {
                Debug.Log("Failed to parse received data as integer.");
            }
            

            Debug.Log("Bytes received: " + numData.ToString());
        }
        else
        {
            Debug.Log("No data available to read.");
        }

        Refrigeracion.text = RefrigerationRate.ToString();
    
    
}

public void DisminuirRefrigeracion()
{

    

        byte[] data = { 0x52}; 
        _serialPort.Write(data, 0, 1);
        Debug.Log("Send Data");
    


        if (_serialPort.BytesToRead > 0)
        {
 
            int bytesAvailable = _serialPort.BytesToRead;
            

            int numData = _serialPort.Read(buffer, 0, bytesAvailable);
            

            string receivedData = System.Text.Encoding.ASCII.GetString(buffer, 0, numData);
            Debug.Log("Received Data: " + receivedData);
            


            if (int.TryParse(receivedData, out RefrigerationRate))
            {

                Debug.Log("Refrigeration Rate: " + RefrigerationRate);
            }
            else
            {
                Debug.Log("Failed to parse received data as integer.");
            }
            

            Debug.Log("Bytes received: " + numData.ToString());
        }
        else
        {
            Debug.Log("No data available to read.");
        }

        Refrigeracion.text = RefrigerationRate.ToString();
    
    
}
//--------------------------FIN MANEJO REFRIGERACION----------------------------------

//----------------------------INICIO MANEJO TEMPERATURA---------------------------------

public void AumentarVelocidadConsumoTemperatura()
{

    

        byte[] data = { 0x54 }; 
        _serialPort.Write(data, 0, 1);
        Debug.Log("Send Data");
         if (_serialPort.BytesToRead > 0)
        {
            int bytesAvailable = _serialPort.BytesToRead;

            int numData = _serialPort.Read(buffer, 0, bytesAvailable);

            string receivedData = System.Text.Encoding.ASCII.GetString(buffer, 0, numData);
            Debug.Log("Received Data: " + receivedData);
            

            if (int.TryParse(receivedData, out TemperatureConsumptionRate))
            {

                Debug.Log("Temperature Consumption Rate: " + TemperatureConsumptionRate);
            }
            else
            {
                Debug.Log("Failed to parse received data as integer.");
            }
            

            Debug.Log("Bytes received: " + numData.ToString());
        }
        else
        {
            Debug.Log("No data available to read.");
        }

        VelocidadConsumoTemperatura.text = TemperatureConsumptionRate.ToString();
    
    
}

public void DisminuirVelocidadConsumoTemperatura()
{

    

        byte[] data = { 0x59}; 
        _serialPort.Write(data, 0, 1);
        Debug.Log("Send Data");
    


        if (_serialPort.BytesToRead > 0)
        {
            int bytesAvailable = _serialPort.BytesToRead;

            int numData = _serialPort.Read(buffer, 0, bytesAvailable);
            
            string receivedData = System.Text.Encoding.ASCII.GetString(buffer, 0, numData);
            Debug.Log("Received Data: " + receivedData);

            if (int.TryParse(receivedData, out TemperatureConsumptionRate))
            {
                Debug.Log("Temperature Consumption Rate: " + TemperatureConsumptionRate);
            }
            else
            {
                Debug.Log("Failed to parse received data as integer.");
            }

            Debug.Log("Bytes received: " + numData.ToString());
        }
        else
        {
            Debug.Log("No data available to read.");
        }

        VelocidadConsumoTemperatura.text = TemperatureConsumptionRate.ToString();
    
    
}

public void AumentarTemperatura()
{

    

        byte[] data = { 0x55}; 
        _serialPort.Write(data, 0, 1);
        Debug.Log("Send Data");
    


        if (_serialPort.BytesToRead > 0)
        {

            int bytesAvailable = _serialPort.BytesToRead;
            
            int numData = _serialPort.Read(buffer, 0, bytesAvailable);

            string receivedData = System.Text.Encoding.ASCII.GetString(buffer, 0, numData);
            Debug.Log("Received Data: " + receivedData);
            


            if (int.TryParse(receivedData, out TemperatureRate))
            {

                Debug.Log("Temperature Rate: " + TemperatureRate);
            }
            else
            {
                Debug.Log("Failed to parse received data as integer.");
            }
            

            Debug.Log("Bytes received: " + numData.ToString());
        }
        else
        {
            Debug.Log("No data available to read.");
        }

        Temperatura.text = TemperatureRate.ToString();
    
    
}

public void DisminuirTemperatura()
{

    

        byte[] data = { 0x49}; 
        _serialPort.Write(data, 0, 1);
        Debug.Log("Send Data");
    


        if (_serialPort.BytesToRead > 0)
        {
 
            int bytesAvailable = _serialPort.BytesToRead;
            

            int numData = _serialPort.Read(buffer, 0, bytesAvailable);
            

            string receivedData = System.Text.Encoding.ASCII.GetString(buffer, 0, numData);
            Debug.Log("Received Data: " + receivedData);
            


            if (int.TryParse(receivedData, out TemperatureRate))
            {

                Debug.Log("Temperature Consumption Rate: " + TemperatureRate);
            }
            else
            {
                Debug.Log("Failed to parse received data as integer.");
            }
            

            Debug.Log("Bytes received: " + numData.ToString());
        }
        else
        {
            Debug.Log("No data available to read.");
        }

        Temperatura.text = TemperatureRate.ToString();
    
    
}
//----------------------------FIN MANEJO TEMPERATURA---------------------------------

//--------------------------INICIO MANEJO HABILITAR/DESHABILITAR VARIABLES-------------

public void OnOffOxigeno()
{

    

        byte[] data = { 0x4F}; 
        _serialPort.Write(data, 0, 1);
        Debug.Log("Send Data");
    


        if (_serialPort.BytesToRead > 0)
        {
 
            int bytesAvailable = _serialPort.BytesToRead;
            

            int numData = _serialPort.Read(buffer, 0, bytesAvailable);
            

            string receivedData = System.Text.Encoding.ASCII.GetString(buffer, 0, numData);
            Debug.Log("Received Data: " + receivedData);
            
        }
        else
        {
            Debug.Log("No data available to read.");
        }   
}

public void OnOffRefrigeracion()
{

    

        byte[] data = { 0x50}; 
        _serialPort.Write(data, 0, 1);
        Debug.Log("Send Data");
    


        if (_serialPort.BytesToRead > 0)
        {
 
            int bytesAvailable = _serialPort.BytesToRead;
            

            int numData = _serialPort.Read(buffer, 0, bytesAvailable);
            

            string receivedData = System.Text.Encoding.ASCII.GetString(buffer, 0, numData);
            Debug.Log("Received Data: " + receivedData);
            
        }
        else
        {
            Debug.Log("No data available to read.");
        }   
}

public void OnOffTemperatura()
{

    

        byte[] data = { 0x41}; 
        _serialPort.Write(data, 0, 1);
        Debug.Log("Send Data");
    


        if (_serialPort.BytesToRead > 0)
        {
 
            int bytesAvailable = _serialPort.BytesToRead;
            

            int numData = _serialPort.Read(buffer, 0, bytesAvailable);
            

            string receivedData = System.Text.Encoding.ASCII.GetString(buffer, 0, numData);
            Debug.Log("Received Data: " + receivedData);
            
        }
        else
        {
            Debug.Log("No data available to read.");
        }   
}

//--------------------------FIN MANEJO HABILITAR/DESHABILITAR VARIABLES-------------

public void IniciarPartida()
{

    

        byte[] data = { 0x4A}; 
        _serialPort.Write(data, 0, 1);
        Debug.Log("Send Data");
    


        if (_serialPort.BytesToRead > 0)
        {
 
            int bytesAvailable = _serialPort.BytesToRead;
            

            int numData = _serialPort.Read(buffer, 0, bytesAvailable);
            

            string receivedData = System.Text.Encoding.ASCII.GetString(buffer, 0, numData);
            Debug.Log("Received Data: " + receivedData);
            
        }
        else
        {
            Debug.Log("No data available to read.");
        }   
}

}