# Home-automation-using-Arduino

#include <dht.h>

dht DHT;

#define DHT11_PIN 5
int inputPin = 3;               // choose the input pin (for PIR sensor)
int pirState = LOW;             // we start, assuming no motion detected
int val = 0;                    // variable for reading the pin status

const int RELAY_PIN = 8;
const int RELAY_PIN_2 = 9;
const int RELAY_PIN_3 = 10;
 
void setup() {
  pinMode(inputPin, INPUT);     // declare sensor as input
  pinMode(RELAY_PIN, OUTPUT);
  digitalWrite(RELAY_PIN, LOW);
  Serial.begin(9600);
}
 
void loop(){
  int chk = DHT.read11(DHT11_PIN);
  float temp = (float)DHT.temperature;
  val = digitalRead(inputPin);  // read input value
  Serial.print("Temperature = ");
  Serial.println((float)DHT.temperature, 2);
  if (val == HIGH) {            // check if the input is HIGH
    if (pirState == LOW) {
      // we have just turned on
      Serial.println("Motion detected!");
      // We only want to print on the output change, not state
      pirState = HIGH;
      if(temp > 20.00 && temp <= 25.00){
        Serial.println("Level 1");
        digitalWrite(RELAY_PIN, HIGH);
        digitalWrite(RELAY_PIN_2, LOW);
        digitalWrite(RELAY_PIN_3, LOW);
      }else if(temp > 25.00 && temp <= 30.00){
        Serial.println("Level 2");
        digitalWrite(RELAY_PIN, LOW);
        digitalWrite(RELAY_PIN_2, HIGH);
        digitalWrite(RELAY_PIN_3, LOW);
      }else if(temp > 30.00 && temp <= 35.00){
        Serial.println("Level 3");
        digitalWrite(RELAY_PIN, HIGH);
        digitalWrite(RELAY_PIN_2, HIGH);
        digitalWrite(RELAY_PIN_3, LOW);
      }else if(temp > 35.00){
        Serial.println("Level 4");
        digitalWrite(RELAY_PIN, LOW);
        digitalWrite(RELAY_PIN_2, LOW);
        digitalWrite(RELAY_PIN_3, HIGH);
      }else{
        Serial.print("OFF ");
        digitalWrite(RELAY_PIN, LOW);
        digitalWrite(RELAY_PIN_2, LOW);
        digitalWrite(RELAY_PIN_3, LOW);
      }
      }
      }else {
    if (pirState == HIGH){
      // we have just turned of
      Serial.println("Motion ended!");
      digitalWrite(RELAY_PIN, LOW);
        digitalWrite(RELAY_PIN_2, LOW);
        digitalWrite(RELAY_PIN_3, LOW);
      // We only want to print on the output change, not state
      pirState = LOW;
    }
  }
  delay(3000);
    }
    
