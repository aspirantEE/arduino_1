# arduino_1
Weather Station
#include <dht11.h>      // Including library for dht
#include <LiquidCrystal.h>
LiquidCrystal lcd( 12, 11, 5, 4, 3, 2);
#define dht_dpin 8
dht11 DHT;
byte degree[8] = 
              {
                0b00011,
                0b00011,
                0b00000,
                0b00000,
                0b00000,
                0b00000,
                0b00000,
                0b00000
              };
  int buzzer=10;//set digital IO
void setup()
{
  Serial.begin(9600);
  lcd.begin(16, 2);
  lcd.createChar(1, degree);
  lcd.clear();
  lcd.print("   Humidity   ");
  lcd.setCursor(0,1);
  lcd.print("  Measurement  ");
  delay(2000);
  lcd.setCursor(0,0);
  lcd.print(" &Temperature");
  delay(2000);
  lcd.clear();
  pinMode(buzzer,OUTPUT);//set mode to OUTPUT
}
void loop()
{
  int a=0;
  int b=0;
  int c=0;
  int d=0;
  int i=1;
  DHT.read(dht_dpin);
  lcd.setCursor(0,0);
  lcd.print("Humidity:");
  for(i=1;i<=100;i++){
    a+=DHT.humidity;
    delay(2);
  }
  b=a/100;
  lcd.print(b);   // printing Humidity on LCD
  lcd.print(" %");
  lcd.setCursor(0,1);
  lcd.print("Temp:");
  for(i=1;i<=100;i++){
    c+=DHT.temperature;
    delay(2);
  }
  d=c/100;
  lcd.print(d);   // Printing temperature on LCD
  lcd.write(1);
  lcd.print("C");

  if(b > 75 && d > 30){
     digitalWrite(buzzer,HIGH);//ring
     lcd.setCursor(0,0);
     lcd.print("  danger     ");
     lcd.setCursor(0,1);
     lcd.print("  danger ");
     delay(500);
  }
  else if(b > 75 && d < 30){
     digitalWrite(buzzer,HIGH);//ring
     lcd.setCursor(0,1);
     lcd.print("  danger ");
     delay(500);
  }
  else if(d > 30){
     digitalWrite(buzzer,HIGH);//ring
     lcd.setCursor(0,0);
     lcd.print("  danger     ");
     delay(500);
  }
  else{
    digitalWrite(buzzer,LOW);//don't ring
  }

  Serial.print("Temperature:");                        //print 'Temperature':
  Serial.print(d);                                     //print temperature
  Serial.println(" C");

  Serial.print("Humidity:");                            //print 'Humidity':
  Serial.print(b);                                     //print humidity
  Serial.println("%");                                 

  delay(500);
}
