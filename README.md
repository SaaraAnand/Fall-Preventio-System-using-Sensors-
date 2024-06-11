# Fall-Preventio-System-using-Sensors-
The aging of population is a worldwide social problem. The health and quality of life of the people will have a significant impact on the country and society. In fact, falls are the leading cause of accidental injury or death in the elderly. This project designs an indoor protection device for elderly patients in the rehabilitation stage. 
CPP CODE:
#include<SoftwareSerial.h>
SoftwareSerial mySerial(2,3);
#include <Wire.h>
#include <Adafruit_Sensor.h>
#include <Adafruit_ADXL345_U.h>

int xval,yval,zval,magnitude,xref,yref;
Adafruit_ADXL345_Unified accel = Adafruit_ADXL345_Unified(12345);
int fall=0;
int dst1=0;
int tr=5;
int ec=6;
int buz=4;
int rly=7;

void setup() {
  Serial.begin(9600); 
mySerial.begin(9600); 
accel.begin();
  pinMode(buz,OUTPUT);
  pinMode(rly,OUTPUT);

   pinMode(tr,OUTPUT);
  pinMode(ec,INPUT);

  digitalWrite(buz,0);
  digitalWrite(rly,1);

for(int i=0;i<5;i++)
{
  sensors_event_t event; 
 accel.getEvent(&event);
 xval=xval+event.acceleration.x; 
 yval=yval+event.acceleration.y;
 
}
  xref=xval/5;
  yref=yval/5;
  delay(2000);

}

void loop() {


 sensors_event_t event; 
 accel.getEvent(&event);
 xval=event.acceleration.x; 
 yval=event.acceleration.y;
 
 fall=0;
  
digitalWrite(tr,1);
delayMicroseconds(10);
digitalWrite(tr,0);
delayMicroseconds(2);
int dst=pulseIn(ec,1)/58.2;

if(abs(xval-xref)>3 || abs(yval-yref)>3)
  {
    fall=1;
  }

Serial.println("D:"+String(dst)+ " F:"+String(fall));
delay(10);

if(dst<20)
{
  fall=0;
}

if(fall==1)
{
  
Serial.println("fall");
mySerial.print("2508157,ZIPEMI5VIN893XK8,0,0,SRC 24G,src@internet,"+String(dst)+","+String(fall)+",\n");
dst1=dst;
digitalWrite(rly,0);
digitalWrite(buz,1);
delay(1000);
digitalWrite(buz,0);
delay(29000);
digitalWrite(rly,1);

}
    
}
