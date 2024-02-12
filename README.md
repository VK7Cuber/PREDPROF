#include <AmperkaServo.h>
AmperkaServo servo;
 
// Задаём имя пина, к которому подключён сервопривод
constexpr uint8_t SERVO_PIN = A9;
#define fr1 A1
#define fr2 A2
#define fr3 A3
#define fr4 A4
#define fr5 A5
#define fr6 A6
#define fr7 A7
#define fr8 A8
 
void setup() {
  pinMode(fr1, INPUT);
  pinMode(fr2, INPUT);
  pinMode(fr3, INPUT);
  pinMode(fr4, INPUT);
  pinMode(fr5, INPUT);
  pinMode(fr6, INPUT);
  pinMode(fr7, INPUT);
  pinMode(fr8, INPUT);
  servo.attach(SERVO_PIN, 544, 2400);
  Serial.begin(9600);
}
 
float adds1, k = 0.05, adds2 = 0, adds3 = 0, adds4 = 0, adds5 = 0, adds6 = 0, adds7 = 0, adds8 = 0;
int ind = 1, pos = 1, min;
void loop() {
   min = 1000000; 
  adds1 += (analogRead(fr1) - adds1) * k;
  adds2 += (analogRead(fr2) - adds2) * k;
  adds3 += (analogRead(fr3) - adds3) * k;
  adds4 += (analogRead(fr4) - adds4) * k;
  adds5 += (analogRead(fr5) - adds5) * k;
  adds6 += (analogRead(fr6) - adds6) * k;
  adds7 += (analogRead(fr7) - adds7) * k;
  adds8 += (analogRead(fr8) - adds8) * k;
  int s[8] = {adds1, adds2, adds3, adds4, adds5, adds6, adds7, adds8};
  for(int i = 1; i<9; i++){
    if (s[i-1] < min && min - s[i-1] > 10){
      min = s[i-1];
      ind = i ;
    }
  }
  if(abs(ind-pos) <= 4){
    if(pos<ind){
      int povr = ind - pos;
      servo.writeSpeed(245);
      delay(178*povr);
      servo.writeSpeed(0);
      delay(50);
    }
    if(pos>ind){
      int povr = pos - ind;
      servo.writeSpeed(-245);
      delay(178*povr);
      servo.writeSpeed(0);
      delay(50);
    }
  }
  if(abs(ind-pos) > 4){
    if(pos < ind){
      int povr = 8 - ind + pos;
      servo.writeSpeed(-245);
      delay(178*povr);
      servo.writeSpeed(0);
      delay(50);
    }
    if(pos>ind){
      int povr = 8 - pos + ind;
      servo.writeSpeed(245);
      delay(178*povr);
      servo.writeSpeed(0);
      delay(50);
    }
  }
  pos = ind;
  ind = 0;
 }
