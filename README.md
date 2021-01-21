# arduino_1
Weather Station
#define BLINKER_WIFI 
#include <Blinker.h>
#include <DHT.h>

#define DHTPIN 2                        //set DHT11 to io2
#define DHTTYPE DHT11                   //DHT11

char auth[] = "your phone key";         //your phone key
char ssid[] = "your wifi name";         //your wifi name
char pswd[] = "123456789gt";            //your wifi key


BlinkerNumber HUMI("humity");           //define the name of humity
BlinkerNumber TEMP("temperature");      //define the name of temperature
   
DHT dht(DHTPIN, DHTTYPE); 
 
float humi_read = 0, temp_read = 0;
 
void heartbeat()
{
    HUMI.print(humi_read);             //return temperature data to blinkerapp
    TEMP.print(temp_read);             //return humity data to blinkerapp
}

void setup()
{
    Serial.begin(115200);
    BLINKER_DEBUG.stream(Serial);
    BLINKER_DEBUG.debugAll();
 
    Blinker.begin(auth, ssid, pswd);   //initialize blinker 
    Blinker.attachHeartbeat(heartbeat);//send data to blinker app
    dht.begin();                       //initialize DHT
}

void loop() 
{
    Blinker.run();                     //run Blinker
 
    float h = dht.readHumidity();      //read humidity from DHT11
    float t = dht.readTemperature();   //read temperature from DHT11
    if (isnan(h) || isnan(t))          //determine if the humidity data is read successfully判断是否成功读取到温湿度数据
    {
        BLINKER_LOG("Failed to read from DHT sensor!");
    }
    else
    {   
        BLINKER_LOG("Humidity: ", h, " %");
        BLINKER_LOG("Temperature: ", t, " *C");
        
        humi_read = h;
        temp_read = t;
    }
    Blinker.delay(2000);
}
