# Live_covid-19_data_on_16x2_LCD
**Live Covid-19 Data Display on 16x2 LCD

<pre>
<code>

#include <ESP8266WiFi.h>
#include <ESP8266HTTPClient.h>
#include <Wire.h>
#include <LiquidCrystal_I2C.h>
#include <StringSplitter.h>

LiquidCrystal_I2C lcd(0x27, 2, 1, 0, 4, 5, 6, 7, 3, POSITIVE); //Set the LCD address to 0x27 for 16 characters and 2 line Display
const char* ssid = "Honor 9 Lite";
const char* password = "abhishek11";
const char* host = "api.thingspeak.com";
const int httpPort = 80;
const char* url = "/apps/thinghttp/send_request?api_key=<Enter your API Key>";
HTTPClient http;
String cases, death, recover;
void setup() {
  // put your setup code here, to run once:
  lcd.begin(16,2);
  lcd.backlight();
  Serial.begin(9600);
  WiFi.begin(ssid,password);
  while(WiFi.status() != WL_CONNECTED)
  {
    Serial.print("..");
    lcd.clear();
    lcd.setCursor(0,0);
    lcd.print("Connecting.....");
    delay(2000);
  }
  Serial.println("");
  Serial.println("Node MCU is Connected to:");
  Serial.println(WiFi.localIP());
  lcd.clear();
  lcd.setCursor(0,0);
  lcd.print("Node MCU is Connected to:");
  lcd.setCursor(0,1);
  lcd.print(WiFi.localIP());
}

void loop() {
  // put your main code here, to run repeatedly:
  http.begin(host,httpPort,url);
  int httpCode = http.GET();
  String data = http.getString();
  Serial.println(httpCode);
  Serial.println("******************  Data Read  *******************");
  Serial.println(data);

  StringSplitter *splitter = new StringSplitter(data, '/', 3); //new StringSplitter(string_to_split, delimiter, item);
  int itemCount = splitter->getItemCount();
  Serial.println("****************  Data Splited  ***************");
  Serial.println("Item count: " + String(itemCount));

  for(int i =0; i < itemCount; i++)
  {
     String item = splitter->getItemAtIndex(i);
     Serial.println("Item @ index " + String(i) + ": " + String(item));
     if(i==0)
     {
      cases = item;                                                                                          
     }
     if(i==1)
     {
      death = item;                                                                                          
     }
     if(i==2)
     {
      recover = item;                                                                                          
     }
  }
  cases.remove(0,26);
  cases.remove(cases.length()-2,cases.length());
  death.remove(0,13);
  death.remove(death.length()-1,death.length());
  recover.remove(0,13);
  recover.remove(recover.length()-8,recover.length());

  Serial.println("************ Data Filtered  ************");
  Serial.println(cases);
  Serial.println(death);
  Serial.println(recover);
  lcd.clear();
  lcd.setCursor(0,0);
  lcd.print("Covid-19 India");
  lcd.setCursor(0,1);
  lcd.print("Cases:" + (String) cases);
  delay(10000);
  lcd.clear();
  lcd.setCursor(0,0);
  lcd.print("Deaths");
  lcd.setCursor(0,1);
  lcd.print((String) death);
  delay(10000);
  lcd.clear();
  lcd.setCursor(0,0);
  lcd.print("Recover:");
  lcd.setCursor(0,1);
  lcd.print((String) recover);
  delay(10000);
  lcd.clear();
}
</code>
</pre>

**Schematic**

<img src = "https://github.com/abhisheksharma1310/Live_covid-19_data_on_16x2_LCD/blob/main/MCU_LCD-I2C.jpg">


**Important Screen Shots**

<img src = "https://github.com/abhisheksharma1310/Live_covid-19_data_on_16x2_LCD/blob/main/Thingspeak1.jpg">

<img src = "https://github.com/abhisheksharma1310/Live_covid-19_data_on_16x2_LCD/blob/main/Thingspeak2.jpg">

<img src = "https://github.com/abhisheksharma1310/Live_covid-19_data_on_16x2_LCD/blob/main/Worldometer.jpg">

<img src = "https://github.com/abhisheksharma1310/Live_covid-19_data_on_16x2_LCD/blob/main/Worldometer1.jpg">

