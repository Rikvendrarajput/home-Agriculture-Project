#include <dht.h>
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd(0x27, 20, 4);

#define dht_pin A0  // Analog Pin A0 of Arduino is connected to DHT11 out pin
#define moist A2      // Analog Pin A2 of Arduino is connected to moisture out pin
#define LDR A1        // Analog Pin A1 of Arduino is connected to LDR out pin

dht DHT;

void setup()
{
  pinMode(2, OUTPUT);
  pinMode(4, OUTPUT);
  pinMode(8, OUTPUT);
  pinMode(9, OUTPUT);
  pinMode(A1, INPUT);
  pinMode(A2, INPUT);

  lcd.begin();
  lcd.backlight();
  lcd.clear();
}

void loop()
{
  // LED Code Start
  digitalWrite(2, HIGH);
  digitalWrite(4, HIGH);

  // Fan Code Start
  DHT.read11(dht_pin);
  if (DHT.temperature > 24.0)
  {
    digitalWrite(8, HIGH); // Turn on the fan
    digitalWrite(9, LOW);
  }
  else
  {
    digitalWrite(8, LOW); // Turn off the fan
    digitalWrite(9, HIGH);
  }

  lcd.print("Humidity = ");
  lcd.print(DHT.humidity);
  lcd.print("%");
  lcd.setCursor(0, 0);
  delay(500);

  lcd.print("Temperature = ");
  lcd.print(DHT.temperature);
  lcd.setCursor(0, 1);
  delay(500);

  int val = analogRead(A1); // Read the analog value from the sensor
  lcd.print("Moisture = ");
  lcd.print(val);
  lcd.setCursor(0, 2);

  int LDRReading = analogRead(A2);
  lcd.print("LDR = ");
  lcd.print(LDRReading);
  lcd.setCursor(0, 3);
  delay(500);
}
