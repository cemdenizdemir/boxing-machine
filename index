#include <LiquidCrystal.h>
#include "HX711.h"

HX711 scale1, scale2, scale3;

LiquidCrystal lcd(12, 11, 10, 9, 8, 7);

String header = "Box Makinesi";

int lampPin = 0;
int speedPin = A0;
int bellPin = 15;

int speed = 0;
int weight = 0; 
int speedPinValue = 0;

int record = 800;

long int lampPrevMillis = 0;
long int lampInterval = 500;

unsigned long musicPrevMillis = 0;
int sequenceState = 0;
const int intervals[] = { 0, 1000, 400, 400, 1000, 400, 400, 1000, 400, 400, 1000, 1000 };
const int numberOfIntervals = sizeof(intervals) / sizeof(intervals[0]);


void setup() {
  lcd.begin(20, 4);
  pinMode(lampPin, OUTPUT);
  
  scale1.begin(A1, A2);
  scale2.begin(51, 53);
  scale3.begin(20, 21);
}

void loop() {
  speedPinValue = analogRead(speedPin);
  speed = map(speedPinValue, 0, 1023, 0, 100);
  weight = getWeight();

  printScreen(speed, weight);
  setLamp();
  playMusic();
}

int getWeight() {
  long w1 = scale1.read();
  long w2 = scale2.read();
  long w3 = scale3.read();
  return (w1 + w2 + w3) / 420;
}

int setScore() {
  int score = speed * weight / 3 - 1;
  if(score < 0 ) { score = 0; }
  if(score > 999 ) { score = 999; }
  if(score > record) {record = score; }
  return score;
}

void setLamp() {
  long int currentMillis = millis();

  if (currentMillis - lampPrevMillis > lampInterval) {
    lampPrevMillis = currentMillis;
    digitalWrite(lampPin, !digitalRead(lampPin)); 
  }
}

void printScreen(int speed, int weight) { 
  // lcd.clear();
  lcd.setCursor(0,0); 
  lcd.print(header);

  lcd.setCursor(0, 1);
  lcd.print("Hiz:  ");
  lcd.print(speed);
  lcd.print(" km/h    ");

  lcd.setCursor(0, 2);
  lcd.print("Kilo: ");
  lcd.print(weight);
  lcd.print(" kg    ");
  

  if(digitalRead(14) == LOW) {
    lcd.setCursor(0, 3);
    lcd.print("Puan: ");
    lcd.print(setScore());
  }

  lcd.setCursor(10, 3);
  lcd.print("Rekor: ");
  lcd.print(record);
}

void playMusic() {
  //https://forum.arduino.cc/t/piezo-buzzer-eye-of-the-tiger-intro/210011

  unsigned long currentMillis = millis();
  if (currentMillis - musicPrevMillis >= intervals[sequenceState]) {
    musicPrevMillis = currentMillis;

    switch (sequenceState) {
     case 0:
      tone(bellPin, 988, 250);
        break;
      case 1:
        tone(bellPin, 988, 250);
        break;
      case 2:
        tone(bellPin, 880, 250);
        break;
      case 3:
        tone(bellPin, 988, 250);
        break;
      case 4:
        tone(bellPin, 988, 250);
        break;
      case 5:
        tone(bellPin, 880, 250);
        break;
      case 6:
        tone(bellPin, 988, 250);
        break;
      case 7:
        tone(bellPin, 988, 250);
        break;
      case 8:
        tone(bellPin, 880, 250);
        break;
      case 9:
        tone(bellPin, 784, 1000);
        break;
      case 10:
        tone(bellPin, 880, 1000);
        break;           
    }

    sequenceState++;
      if (sequenceState == numberOfIntervals) {
        sequenceState = 0; 
      }                                 
  }  
}


