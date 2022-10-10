# Air-Pollution---Soil-Moisture-Detecting-Device
#include <LiquidCrystal.h>
LiquidCrystal lcd(2, 3, 4, 5, 6, 7);

int smokeA0 = A2;
int Moisture_signal = A1;
int buzzer = 13;
int G_led = 8;
int R_led = 9;

float gasValue;
float Moisture;

void setup() {
  pinMode(smokeA0, INPUT);
  pinMode(Moisture_signal, INPUT);
  pinMode(buzzer, OUTPUT);
  pinMode(R_led, OUTPUT);
  pinMode(G_led, OUTPUT);

  lcd.begin(16, 2);
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("   Soil Detector  ");
  lcd.setCursor(0, 1);
  lcd.print("   Gas Detector  ");
  delay(2000);
  lcd.clear();

  Serial.begin(9600);
  
  delay(1000);
  noTone(buzzer);

}

void loop() {
  gasValue = analogRead(smokeA0);
  Moisture = analogRead(Moisture_signal);

  delay(1000);

  Serial.print("Gas Sensor Value: ");
  Serial.println(gasValue);

  Serial.print("Soil Moisture Level: ");
  Serial.println(Moisture);


  if (gasValue > 300)
  {
    lcd.setCursor(0, 0);
    lcd.print("Alert!impurities");
    lcd.setCursor(0,1);
    lcd.print(gasValue);
    Serial.print("!Smoke detected!");
    digitalWrite(R_led, HIGH);
    digitalWrite(G_led, LOW);
    tone(buzzer, 2000, 200);
  }
  else
  {
    lcd.setCursor(0, 0);
    lcd.print("Gas Content-Normal");
    lcd.setCursor(0, 1);
    lcd.print(gasValue);
    digitalWrite(R_led, LOW);
    digitalWrite(G_led, HIGH);
    noTone(buzzer);
  }

  Serial.println("");
  delay(2000);
  
  lcd.setCursor(0, 0);
  lcd.print("Moisture Content:");
  lcd.setCursor(0,1);
  lcd.print(Moisture);
  delay(1000); 

  Serial.println("");
  delay(2000);
}
