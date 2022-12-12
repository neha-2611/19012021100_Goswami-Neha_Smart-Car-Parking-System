
# Smart Car Prking System



## -Introduction:

The project entitled smart parking system is to manage all the parking facilities to a user. 
The recent growth in economy and due to the availability of low-price cars in the market, 
every average middle-class individual can afford a car, which is good thing, however the 
consequences of heavy traffic jams, pollution, less availability of roads and spot to drive 
the motor car. One of the important concerns, which is to be taken in accounting, is the 
problem of parking those vehicles. Though, if there is space for parking the vehicle but so 
much time is squandered in finding that exact parking slot resulting in more fuel intake and 
not also environment friendly. It will be a great deal if in some way we find out that the 
parking itself can provide the precise vacant position of a parking slot then it'll be helpful 
not limited to the drivers also for the environment. Initially when the user is about to enter 
the location the number of empty and filled spots and when the user is with its vehicle near 
to the parking detect sensor, he/she would be thrown with a notification on their mobile 
app of the parking slot number, where they should park their vehicle.
 
A smart car parking system gives a visual output indicating an available parking space 
rather than driving aimlessly. The driver looks up to the row of LED lights and their color 
to deduct a result of determining the parking space availability. The two main colors used 
are red and yellow stating occupied and free respectively. These lights are placed at the 
ceiling of each parking space and the driver looks up and follows the set of LEDs and 
searches for a Yellow one. These lights are controlled automatically with sensors and the 
feedback is provided through the color of the LED when a vehicle is detected. This system 
not only makes the accessibility easy but also manages the congestion of vehicles avoiding 
long search and wait times. 

## -Source Code:

Arduino code:

#include <SoftwareSerial.h>
#include <Servo.h>

Servo myservo1;  // create servo object to control a servo
Servo myservo2;

SoftwareSerial nodemcu(0,1);

int parking1_slot1_ir_s = 4; // parking slot1 infrared sensor connected with pin number 4 of arduino
int parking1_slot2_ir_s = 5;
int parking1_slot3_ir_s = 6;

int parking2_slot1_ir_s = 7;
int parking2_slot2_ir_s = 8;
int parking2_slot3_ir_s = 9;

int entrance_gate = 11;
int exit_gate = 12;

int pos1 = 90;    // variable to store the servo position(entrance gate)
int pos2 = 90;    // exit gate

String sensor1; 
String sensor2; 
String sensor3; 
String sensor4; 
String sensor5; 
String sensor6; 


String cdata =""; // complete data, consisting of sensors values

void setup()
{
Serial.begin(9600); 
nodemcu.begin(9600);

pinMode(parking1_slot1_ir_s, INPUT);
pinMode(parking1_slot2_ir_s, INPUT);
pinMode(parking1_slot3_ir_s, INPUT);

pinMode(parking2_slot1_ir_s, INPUT);
pinMode(parking2_slot2_ir_s, INPUT);
pinMode(parking2_slot3_ir_s, INPUT);

pinMode(entrance_gate, INPUT);
pinMode(exit_gate, INPUT);

myservo1.attach(13);  // attaches the servo on pin 9 to the servo object
myservo2.attach(3);


}

void loop()
{

p1slot1(); 
p1slot2();
p1slot3(); 

p2slot1();
p2slot2();
p2slot3();

gates();
//conditions();

  
  
   cdata = cdata + sensor1 +"," + sensor2 + ","+ sensor3 +","+ sensor4 + "," + sensor5 + "," + sensor6 +","; // comma will be used a delimeter
   Serial.println(cdata); 
   nodemcu.println(cdata);
   delay(6000); // 100 milli seconds
   cdata = ""; 
digitalWrite(parking1_slot1_ir_s, HIGH); 
digitalWrite(parking1_slot2_ir_s, HIGH); 
digitalWrite(parking1_slot3_ir_s, HIGH);

digitalWrite(parking2_slot1_ir_s, HIGH);
digitalWrite(parking2_slot2_ir_s, HIGH);
digitalWrite(parking2_slot3_ir_s, HIGH);

digitalWrite(entrance_gate, HIGH);
digitalWrite(exit_gate, HIGH);
}


void p1slot1() // parkng 1 slot1
{
  if( digitalRead(parking1_slot1_ir_s) == LOW) 
  {
  sensor1 = "255";
 delay(200); 
  } 
if( digitalRead(parking1_slot1_ir_s) == HIGH)
{
  sensor1 = "0";  
 delay(200);  
}

}

void p1slot2() // parking 1 slot2
{
  if( digitalRead(parking1_slot2_ir_s) == LOW) 
  {
  sensor2 = "255"; 
  delay(200); 
  }
if( digitalRead(parking1_slot2_ir_s) == HIGH)  
  {
  sensor2 = "0";  
 delay(200);
  } 
}


void p1slot3() // parking 1 slot3
{
  if( digitalRead(parking1_slot3_ir_s) == LOW) 
  {
  sensor3 = "255"; 
  delay(200); 
  }
if( digitalRead(parking1_slot3_ir_s) == HIGH)  
  {
  sensor3 = "0";  
 delay(200);
  } 
}


// now for parking 2

void p2slot1() // parking 1 slot3
{
  if( digitalRead(parking2_slot1_ir_s) == LOW) 
  {
  sensor4 = "255"; 
  delay(200); 
  }
if( digitalRead(parking2_slot1_ir_s) == HIGH)  
  {
  sensor4 = "0";  
 delay(200);
  } 
}


void p2slot2() // parking 1 slot3
{
  if( digitalRead(parking2_slot2_ir_s) == LOW) 
  {
  sensor5 = "255"; 
  delay(200); 
  }
if( digitalRead(parking2_slot2_ir_s) == HIGH)  
  {
  sensor5 = "0";  
 delay(200);
  } 
}


void p2slot3() // parking 1 slot3
{
  if( digitalRead(parking2_slot3_ir_s) == LOW) 
  {
  sensor6 = "255"; 
  delay(200); 
  }
if( digitalRead(parking2_slot3_ir_s) == HIGH)  
  {
  sensor6 = "0";  
 delay(200);
  } 
}

// for the gates

void gates()
{
  if (digitalRead(exit_gate) == LOW)
      {
        for (pos2 = 90; pos2 <= 180 ; pos2 += 1) { // goes from 0 degrees to 180 degrees
          // in steps of 1 degree
          myservo2.write(pos2);              // tell servo to go to position in variable 'pos'
          delay(15);                       // waits 15ms for the servo to reach the position
        }
          delay(1000);
        for (pos2 = 180; pos2 >= 90; pos2 -= 1) { // goes from 180 degrees to 0 degrees
          myservo2.write(pos2);              // tell servo to go to position in variable 'pos'
          delay(15);                       // waits 15ms for the servo to reach the position
        }
      }
  
  if (((digitalRead(entrance_gate) == LOW)) && (( digitalRead(parking1_slot1_ir_s) == HIGH) || ( digitalRead(parking1_slot2_ir_s) == HIGH) || ( digitalRead(parking1_slot3_ir_s) == HIGH) || ( digitalRead(parking2_slot1_ir_s) == HIGH) || ( digitalRead(parking2_slot2_ir_s) == HIGH)|| ( digitalRead(parking2_slot3_ir_s) == HIGH)))
      {
        for (pos1 = 0; pos1 <= 90 ; pos1 += 1) { // goes from 0 degrees to 180 degrees
          // in steps of 1 degree
          myservo1.write(pos1);              // tell servo to go to position in variable 'pos'
          delay(15);                       // waits 15ms for the servo to reach the position
        }
          delay(1000);
        for (pos1 = 90; pos1 >= 0; pos1 -= 1) { // goes from 180 degrees to 0 degrees
          myservo1.write(pos1);              // tell servo to go to position in variable 'pos'
          delay(15);                       // waits 15ms for the servo to reach the position
        }
      }


}



-ESP8266 code:

#define BLYNK_PRINT Serial
#define BLYNK_TEMPLATE_ID "TMPLfjRxF8TP"
#define BLYNK_DEVICE_NAME "Daksh Raval"
#define BLYNK_AUTH_TOKEN "zs6ygzpK60L5amTBovGDBJHgOpkgU8kC"
#include <SoftwareSerial.h>
#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>
//#include <SimpleTimer.h>


SoftwareSerial arduinoUno(0,1); //( RX, TX )

char auth[] = BLYNK_AUTH_TOKEN;

// Your WiFi credentials.
char ssid[] = "enter wifi ssid";
char pass[] = "enter wifi password";

BlynkTimer timer;
//SimpleTimer timer;


String myString; // complete message from arduino, which consists of sensors data
char rdata; // received characters

int firstVal, secondVal,thirdVal; // sensors 
int led1,led2,led3,led4,led5,led6;
// This function sends Arduino's up time every second to Virtual Pin (1).
// In the app, Widget's reading frequency should be set to PUSH. This means
// that you define how often to send data to Blynk App.
void myTimerEvent()
{
  // You can send any value at any time.
  // Please don't send more that 10 values per second.
  Blynk.virtualWrite(V1, millis() / 1000);
  
}

void setup()
{
  // Debug console
  Serial.begin(9600);

  Blynk.begin(auth, ssid, pass);

    timer.setInterval(1000L,sensorvalue1); 
    timer.setInterval(1000L,sensorvalue2); 
    timer.setInterval(1000L,sensorvalue3);
    timer.setInterval(1000L,sensorvalue4);
    timer.setInterval(1000L,sensorvalue5);
    timer.setInterval(1000L,sensorvalue6);
  

}

void loop()
{
   if (Serial.available() == 0 ) 
   {
  Blynk.run();
  timer.run(); // Initiates BlynkTimer
   }
   
  if (Serial.available() > 0 ) 
  {
    rdata = Serial.read(); 
    myString = myString+ rdata; 
    //Serial.print(rdata);
    if( rdata == '\n')
    {
     Serial.println(myString); 
  // Serial.println("fahad");
// new code
String l = getValue(myString, ',', 0);
String m = getValue(myString, ',', 1);
String n = getValue(myString, ',', 2);
String o = getValue(myString, ',', 3);
String p = getValue(myString, ',', 4);
String q = getValue(myString, ',', 5);


// these leds represents the leds used in Blynk application
led1 = l.toInt();
led2 = m.toInt();
led3 = n.toInt();
led4 = o.toInt();
led5 = p.toInt();
led6 = q.toInt();

  myString = "";
// end new code
    }
  }

}

void sensorvalue1()
{
int sdata = led1;
  // You can send any value at any time.
  // Please don't send more that 10 values per second.
  Blynk.virtualWrite(V10, sdata);

}
void sensorvalue2()
{
int sdata = led2;
  // You can send any value at any time.
  // Please don't send more that 10 values per second.
  Blynk.virtualWrite(V11, sdata);

}

void sensorvalue3()
{
int sdata = led3;
  // You can send any value at any time.
  // Please don't send more that 10 values per second.
  Blynk.virtualWrite(V12, sdata);

}

void sensorvalue4()
{
int sdata = led4;
  // You can send any value at any time.
  // Please don't send more that 10 values per second.
  Blynk.virtualWrite(V13, sdata);

}

void sensorvalue5()
{
int sdata = led5;
  // You can send any value at any time.
  // Please don't send more that 10 values per second.
  Blynk.virtualWrite(V14, sdata);

}

void sensorvalue6()
{
int sdata = led6;
  // You can send any value at any time.
  // Please don't send more that 10 values per second.
  Blynk.virtualWrite(V15, sdata);

}


String getValue(String data, char separator, int index)
{
    int found = 0;
    int strIndex[] = { 0, -1 };
    int maxIndex = data.length() - 1;

    for (int i = 0; i <= maxIndex && found <= index; i++) {
        if (data.charAt(i) == separator || i == maxIndex) {
            found++;
            strIndex[0] = strIndex[1] + 1;
            strIndex[1] = (i == maxIndex) ? i+1 : i;
        }
    }
    return found > index ? data.substring(strIndex[0], strIndex[1]) : "";
}




## Build Project:

Step 1: In this Project First, we make a model. After making the model, we made parking 
slots, road for vehicles and made toll gates on both sides of the road. 

Step 2: After making a model, we make a circuit diagram. After making circuit diagram, 
we joined all components with Arduino board. When we are joined all components with 
Arduino board, we compile code for Arduino board, after the compilation we got output 
value in serial monitor. 

We are using Arduino IDE software for this compilation code. In 
this output, we see all sensors are detected and servo motor working.
We are using Arduino board, 8 IR sensors, 2 servo motors (using toll gates), jumper 
wires, bread board and ESP8266 Node MCU. 
 
Step 3: After the compilation of Arduino code, we are using ESP8266 Node MCU. First, 
we are opened the web of Blynk io and register blynk io web. After the registration, open 
the mobile app Blynk Iot and create a device, after creating the device, select thehardware 
and connection type. After this creating all connections, go to developer mode and top 
right side ‘+’ button available. Press the ‘+’ button, open the widget box. After the 
opened the widget box, select the TAB option. When select the TAB option, we are creating two tabs for two parking space (Parking 1 & Parking 2). 
 
In Parking 1 tab, we are going to again widget box and select the 3 times 3 led. The first 
Led1 name is slot1 and select the virtual PIN(V10). Same time we put the led 2(slot 2-
V11) and led 3(slot 3 -V12). 
 
In parking 2 tab, we will select Led in front like parking 1 and select different virtual pin 
number (V13, V14, V15). 

Step 4: Now we are doing connected the ESP8266 Node MCU after the setup work in 
Blink app. The device created in Blynk IO, then we goes to device info and copy the auth 
token. After the auth token copy, go to ESP8266 code and copy the auth token code.we 
put our mobile hotspot device in the code with wifi name and password. 

We compiled the ESP8266 code then ESP8266 connecting the mobile hotspot. After 
connecting open the serial motor and see the slot is empty then output ‘0’ and car go to 
parking then slots value is ‘255’.While opening the Blynk IOT mobile app,we see the parking slots empty and full.  


## Reference:

[1] https://apps.apple.com/in/app/blynk-iot

[2] https://github.com/blynkkk/blynk-library/blob/master/src/Blynk/BlynkTimer.h 

[3] https://docs.blynk.io/en/legacy-platform/legacy-articles/nodemcu


