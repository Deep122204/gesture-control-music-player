//# gesture-control-music-player
//so, this is the gesture control music player code, the gesture senser is apds9960
//code
#include <Wire.h>
#include <SparkFun_APDS9960.h>
 
#define APDS9960_INT    2 // Needs to be an interrupt pin
 
// Global Variables
SparkFun_APDS9960 apds = SparkFun_APDS9960();
int isr_flag = 0;
int relay[]={3,4,5}; 
 
void setup() {
 
  // Initialize Serial port
  Serial.begin(9600);
  Serial.println();
  Serial.println(F("--------------------------------"));
  Serial.println(F("APDS-9960 - GestureTest"));
  Serial.println(F("--------------------------------"));
  pinMode(3, OUTPUT);//define arduino pin D2
  pinMode(4, OUTPUT);
  pinMode(5, OUTPUT);
 
  // Initialize interrupt service routine
  attachInterrupt(0, interruptRoutine, FALLING);
 
  // Initialize APDS-9960 (configure I2C and initial values)
  if ( apds.init() ) {
    Serial.println(F("APDS-9960 initialization complete"));
  } else {
    Serial.println(F("Something went wrong during APDS-9960 init!"));
  }
 
  // Start running the APDS-9960 gesture sensor engine
  if ( apds.enableGestureSensor(true) ) {
    Serial.println(F("Gesture sensor is now running"));
  } else {
    Serial.println(F("Something went wrong during gesture sensor init!"));
  }
}
 
void loop() {
  if( isr_flag == 1 ) {
    detachInterrupt(0);
    handleGesture();
    isr_flag = 0;
    attachInterrupt(0, interruptRoutine, FALLING);
  }
}
 
void interruptRoutine() {
  isr_flag = 1;
}
 
void handleGesture() {
    if ( apds.isGestureAvailable() ) {
    switch ( apds.readGesture() ) {
      case DIR_UP:
        Serial.println("UP");
        digitalWrite(4,HIGH);//relay on
  Serial.println("ON");//print serial monitor ON
  delay(1000);//delay
  digitalWrite(4, LOW);//relay off
  Serial.println("OFF");//print serial monitor OFF
        break;
      case DIR_DOWN:
        Serial.println("DOWN");
        digitalWrite(3,HIGH);//relay on
  Serial.println("ON");//print serial monitor ON
  delay(1000);//delay
  digitalWrite(3, LOW);//relay off
  Serial.println("OFF");//print serial monitor OFF
        break;
      case DIR_LEFT:
        Serial.println("LEFT");
        digitalWrite(3,HIGH);//relay on
  Serial.println("ON");//print serial monitor ON
  delay(100);//delay
  digitalWrite(3, LOW);//relay off
  Serial.println("OFF");//print serial monitor OFF
        break;
      case DIR_RIGHT:
        Serial.println("RIGHT");
        digitalWrite(4,HIGH);//relay on
  Serial.println("ON");//print serial monitor ON
  delay(100);//delay
  digitalWrite(4, LOW);//relay off
  Serial.println("OFF");//print serial monitor OFF
        break;
      case DIR_NEAR:
        Serial.println("play");
        break;
      case DIR_FAR:
        Serial.println("pause");
        digitalWrite(5,HIGH);//relay on
  Serial.println("ON");//print serial monitor ON
  delay(100);//delay
  digitalWrite(5, LOW);//relay off
  Serial.println("OFF");//print serial monitor OFF
        
        break; 
      default:
        Serial.println("NONE");
        
    }
  }
}
