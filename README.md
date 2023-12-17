# paraLife-support-kit
![Screenshot (37)](https://github.com/shreyansh2118/paraLife-support-kit/assets/111567940/cf3faf37-14b1-40aa-8194-df83cad56159)

# code
#include<LiquidCrystal.h>

const int rs = 12, en = 11, d4 = 5, d5 = 4, d6 = 3, d7 = 2;
LiquidCrystal lcd1(rs, en, d4, d5, d6, d7); 
float celsius;
int temp = A1;
int sensorReading = 0;
// alarm
int h=0,m=0,s=0;
int alarmH = 0, alarmMin = 1, alarmSec = 1;
LiquidCrystal lcd(12, 11, 5, 4, 3, 2);
const int buzzer = 10;

//code for button and buzzer
int buttonPin = 7; //inform ide button pada pin 2
int buzPin = 8; //inform ide buzzer pada pin 8
int buttonState = 0;


void setup(){
pinMode(temp,INPUT);
// for flex sensor and buzzer
pinMode(A0, INPUT);
Serial.begin(9600);
pinMode(9, OUTPUT);
//button and buzzer
pinMode(buzPin , OUTPUT); // buzzer pin 8 OUTPUT
pinMode (buttonPin , INPUT); // Button pin 7 INPUT
//alarm
lcd.begin(16, 2);
pinMode(buzzer, OUTPUT);
}


void loop(){
celsius = analogRead(temp)*0.004882814;
celsius = (celsius - 0.5) * 100.0;
lcd.setCursor(0,1);
lcd.print("Temp: ");
lcd.print(celsius);
lcd.print(" C");
delay(1000);
lcd.clear();
// read the sensor for flex sensor
sensorReading = analogRead(A0);


// map the sensor reading to a range for the
// flex speaker
  if(sensorReading<139){
  tone(9, 440 * pow(2.0, (constrain(int(map(sensorReading, 0, 1023, 36, 84)), 35, 127) - 57) / 12.0), 1000);
  delay(10); // Delay a little bit to improve simulation performance
  }

// call button buzzer 
    buttonState = digitalRead (buttonPin); 
  if ( buttonState ==HIGH ){ //when button = HIGH
  digitalWrite(buzPin , HIGH); //OUTPUT = HIGH
   tone(buzPin,500);
    delay(100);//buzzer delay
    digitalWrite(buzPin , LOW);
    noTone(buzPin);
    //delay(100);
  }
  else {
    digitalWrite (buzPin , LOW ); //button= LOW
  }

  delay(500);
  
  // alarm
   s=s+1;
  if(s==60){
    m=m+1;
    s=0;
  }
  
  if(m==60){
    m=0;
    h=h+1;
  }
  
  lcd.print("HOURS=");
  lcd.print(h);
  lcd.setCursor(10,0);
  lcd.print("MIN=");
  lcd.print(m);
  lcd.setCursor(0,1);
  lcd.print("SEC=");
  lcd.print(s);
  delay(1000);
  lcd.clear();
  
  //(h == alarmH) &&  for hour
  if((m == alarmMin) && (alarmSec == s)){
    tone(buzzer, 500); 
    delay(1000);       
    noTone(buzzer);     
     
  }
}
