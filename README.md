# controlling-6-servo-with-ultrasonic-and-second-arduino
In this project i used an ultrasonic sensor to detect anyone in front of the robot for 3 second if there is one then move the hand and send order for the screen to play video using Bluetooth which is demonstrated here as second arduino

**Tinkercad Link:** https://www.tinkercad.com/things/hOuxHKngnmg-epic-jaiks-sango/editel?tenant=circuits



**Circuit diagram:** 


![image](https://user-images.githubusercontent.com/5675794/125006042-dbddd880-e065-11eb-9295-70710b52872c.png)




**Code for first arduino:**

```ruby
#include <Servo.h>;

const int echoPin = 4;
const int trigPin = 2;

long duration; // variable for the duration of sound wave travel
int distance; // variable for the distance measurement

int ServoR1 = 6;
int ServoR2 = 5;
int ServoR3 = 3;
int ServoL1 = 11;
int ServoL2 = 10;
int ServoL3 = 9;

Servo ServoR1_Object;
Servo ServoR2_Object;
Servo ServoR3_Object;
Servo ServoL1_Object;
Servo ServoL2_Object;
Servo ServoL3_Object;

void setup()
  
{
  Serial.begin(9600);
  pinMode(trigPin, OUTPUT); // Sets the trigPin as an OUTPUT
  pinMode(echoPin, INPUT); // Sets the echoPin as an INPUT
  
  ServoR1_Object.attach(ServoR1);
  ServoR2_Object.attach(ServoR2);
  ServoR3_Object.attach(ServoR3);
  ServoL1_Object.attach(ServoL1);
  ServoL2_Object.attach(ServoL2);
  ServoL3_Object.attach(ServoL3);
  
       ServoR1_Object.write(0); // intial position
       ServoR2_Object.write(0);
       ServoR3_Object.write(0);
       ServoL1_Object.write(0);
       ServoL2_Object.write(0);
       ServoL3_Object.write(0);
}

void loop()
{
  while(getDistance() > 25){  continue; } // if the sensor see someone near start counting

  int startTime = millis(); // start counting to know if he stay or pass by only

  while(getDistance() <25){ // if the new value is less than 25 also enter while loop
 
   int newTime = millis(); //record the current time

  if (newTime - startTime > 3000){ // if the we have 3 sec difference between the first value and current value so customer is standing
    Serial.write('1'); // send message to screen to open
    Serial.println('\n');
    
     Serial.println("Doing Welcome Move...");
  
      ServoR1_Object.write(180);
      ServoR2_Object.write(0);
      ServoR3_Object.write(0);
      delay(850);
      ServoR3_Object.write(45);
      delay(850);
      ServoR3_Object.write(0);
      delay(1500);
      
      initialPosition();
      Serial.println("Done");
    break; } // get out so only send message once
  }
  
  while(getDistance() < 25){ continue; } // if he still standing wait untill sensor know he walked away

}

int getDistance(){ // defining the function to calculate the distance
   
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
 
  duration = pulseIn(echoPin, HIGH);
  distance = duration * 0.034 / 2;  // Calculating the distance

  return distance;
}

void initialPosition(){
  
       ServoR1_Object.write(0);
       ServoR2_Object.write(0);
       ServoR3_Object.write(0);
       ServoL1_Object.write(0);
       ServoL2_Object.write(0);
       ServoL3_Object.write(0);
       delay(1500);
}
```





**Code for second arduino used instead of bluetooth module**

```ruby


void setup()
  
{
  Serial.begin(9600);
  pinMode(12, OUTPUT);
}

void loop()
{
  if(Serial.available() != 0){
    
    if(Serial.read() == '1'){
      int start = millis();
      digitalWrite(12, HIGH);
      int current;
      while ( ((current = millis()) - (start)) < 500 ){continue;}
        digitalWrite(12, LOW);
    }
  }

}
```
