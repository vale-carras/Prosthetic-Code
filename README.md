//# Prosthetic-Project
//BME Lab I Project
#include <Servo.h>
// servo library

// Sets up servo variables
Servo servoindex;
Servo servomiddle;
Servo servoring;
Servo servopinky;

// Variable intialization
const int analogInPin = A0;
int sensorValue = 0;
int outputValue = 0;
int n = 0; // starting 0 to apply to the first if statement. changes to 1 later on to apply to the else if statement
int threshold=2.0; // voltage threshold 
int i=1; 
int start=1; // variable that is used one time at the start of the loop.=

void setup() {

// sets up digital pins to correspond with particular servos
  servoindex.attach(3);
  servomiddle.attach(4);
  servoring.attach(5);
  servopinky.attach(6);
  Serial.begin(9600);
 
  rest();
}

void loop() {            

  // on startup this will set the hand back to open (rest). This is only happens once
if (start==1) {
  delay(2000);
  rest() ;
  start=0;
  delay(500);
}


// The sensor reads analog voltage and coverts it to digital.
  sensorValue = analogRead(analogInPin);
  delay(50);
// The digital value is then mathematically converted to the value of the analog voltage 
  float outputValue = sensorValue*(5.0/1023.0);
  
  // print the results to the serial monitor:
  Serial.print("sensor = ");
  Serial.print(sensorValue);
  Serial.print("\t output = ");
  Serial.println(outputValue);

  
for (int i = 1; i < 101; i++) {
 //if the output value is above threshold and n==0 the active function runs and n changes to prepare to match else if conditions
  if (outputValue >= threshold && n == 0){
    
          Serial.println("case 1");
          active() ;
          n = 1;
          delay(5000);
          sensorValue = analogRead(analogInPin);
          
            outputValue = sensorValue*(5.0/1023.0);
  delay(50);


          
         
  }
  //if the ouput value is above threshold and n==1 the rest function runs and n changes to prepare to match the if conditions
         else if (outputValue >= threshold && n == 1) { 
             Serial.println("case 2");
              rest();
              delay(2000);
              n=0;
              sensorValue = analogRead(analogInPin);
              delay(50);
              outputValue = sensorValue*(5.0/1023.0);     
}
    }
}

// Rest is at 0 degrees from the norm (if deviated it will return on activation of this command)
void rest() {
  
  servoindex.write(0);
 
  servomiddle.write(0);

  servoring.write(0);

  servopinky.write(0);

  

}





//  Active is 80 degrees clockwise from 0, from the norm (if deviated it will move to 80 degrees in a clockwise motion 
void active() {
  servopinky.write(80);
 
  servoring.write(80);

  servomiddle.write(80);

  servoindex.write(80);
 
  n==1;

 


}
