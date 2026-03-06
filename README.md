# Iot-smart-agriculture-project-



blynk code
/* Developer contact 
   nk978605@gmail.com
   Blynk password: projectiot2023
*/

/* Fill-in information from Blynk Device Info here */
#define BLYNK_TEMPLATE_ID "TMPL389_46wnF"
#define BLYNK_TEMPLATE_NAME "Agri"
#define BLYNK_AUTH_TOKEN "7lYdNO8b294fp4im_zlrUVHN79ix6jI5"

/* Comment this out to disable prints and save space */
#define BLYNK_PRINT Serial

#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>

// Your WiFi credentials
char ssid[] = "projectiot";
char pass[] = "123456789";

// Virtual pins
#define VPIN_1 V1
#define VPIN_2 V2
#define VPIN_3 V3
#define VPIN_5 V5

BLYNK_CONNECTED()
{
  Blynk.syncVirtual(VPIN_1);
  Blynk.syncVirtual(VPIN_2);
  Blynk.syncVirtual(VPIN_3);
  Blynk.syncVirtual(VPIN_5);
}

void setup()
{
  Serial.begin(9600);
  Blynk.begin(BLYNK_AUTH_TOKEN, ssid, pass);
}

void loop()
{
  Blynk.run();

  if (Serial.available() > 0)
  {
    char a = Serial.read();
    Serial.print(a);

    if (a == 'D')
      Blynk.virtualWrite(VPIN_5, Serial.parseInt());

    if (a == 'M')
      Blynk.virtualWrite(VPIN_1, HIGH);

    if (a == 'm')
      Blynk.virtualWrite(VPIN_1, LOW);

    if (a == 'Y')
      Blynk.virtualWrite(VPIN_2, HIGH);

    if (a == 'y')
      Blynk.virtualWrite(VPIN_2, LOW);

    if (a == 'T')
      Blynk.virtualWrite(VPIN_3, Serial.parseInt());
  }
}



adruino code


#include <LiquidCrystal.h>
#include <Adafruit_Sensor.h>
#include "DHT.h"

LiquidCrystal lcd(13, 12, 11, 10, 9, 8);

#define DHTPIN A2
#define DHTTYPE DHT11

int t;
DHT dht(DHTPIN, DHTTYPE);

const int trigPin = 7;
const int echoPin = 6;

long duration;
int d, distanceInch;
int m;
int R;

void setup()
{
  Serial.begin(9600);
  lcd.begin(16, 2);
  dht.begin();

  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);

  pinMode(A0, INPUT);   // moisture
  pinMode(A3, OUTPUT);  // water motor
  pinMode(A2, INPUT);   // temperature
}

void loop()
{
  dht11();

  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);

  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);

  digitalWrite(trigPin, LOW);

  duration = pulseIn(echoPin, HIGH);
  d = duration * 0.034 / 2;

  Serial.print('D');
  Serial.print(d);

  int m = digitalRead(A0);

  if (m == LOW)
    Serial.print("Y");
  else
    Serial.print("y");

  // retain your full decision logic here
}

void dht11()
{
  int h = dht.readHumidity();
  t = dht.readTemperature();
  float f = dht.readTemperature(true);

  if (isnan(h) || isnan(t) || isnan(f))
    return;

  lcd.setCursor(6, 1);
  lcd.print("T:");

  lcd.setCursor(8, 1);
  lcd.print(t);
  lcd.print(" ");

  Serial.print("T");
  Serial.print(t);

  if (t > 36)
  {
    lcd.setCursor(13, 1);
    lcd.print("T-H");
    Serial.print('A');
  }
  else
  {
    lcd.setCursor(13, 1);
    lcd.print("T-L");
    Serial.print('a');
  }
}