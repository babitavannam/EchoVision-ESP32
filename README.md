#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>
#include <Fonts/FreeSerif9pt7b.h>

#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64

Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, -1);
const int trigPin = 5;
const int echoPin = 18;
const int sw = 4;
bool swval;
int led1=2;
int led2=23;
int dis;
const int potPin = 15;
int potValue=0;
int mapvalue=0;
//define sound speed in cm/uS
#define SOUND_SPEED 0.034
long duration;
float distanceCm;

void setup() {
  pinMode(sw, INPUT_PULLUP);
  Serial.begin(115200); // Starts the serial communication
  pinMode(trigPin, OUTPUT); // Sets the trigPin as an Output
  pinMode(echoPin, INPUT);
  pinMode(led1,OUTPUT); 
  pinMode(led2,OUTPUT); 
  if(!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) { 
    Serial.println("SSD1306 allocation failed");
    for(;;);
  }
  delay(200);
  
}

void loop() {
  swval = digitalRead(sw);
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  // Sets the trigPin on HIGH state for 10 micro seconds
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  
  // Reads the echoPin, returns the sound wave travel time in microseconds
  duration = pulseIn(echoPin, HIGH);
  
  // Calculate the distance
  distanceCm = duration * SOUND_SPEED/2;
  dis=distanceCm;
  // Convert to inches
  //distanceInch = distanceCm * CM_TO_INCH;
  
  // Prints the distance in the Serial Monitor
  Serial.print("Distance (cm): ");
  Serial.println(distanceCm);
  display.setFont(&FreeSerif9pt7b);
  display.clearDisplay();
  display.setTextSize(0.5);             
  display.setTextColor(WHITE);        
  display.setCursor(0,20); 
  display.print("cm: ");
  display.println(distanceCm);
  
  
  potValue = analogRead(potPin);
  mapvalue=map(potValue,0,4095,0,100);
  Serial.println(mapvalue);
  Serial.println(potValue);
  display.print("potValue");
   display.println(mapvalue);
   delay(500);
  
  if (swval == LOW) {   // Button pressed
    display.println("Pressed");
    Serial.println("Pressed");
  }
  else {                // Button not pressed
    display.println("Not Pressed");
    Serial.println("Not Pressed");
  }
  if(distanceCm>80){
    digitalWrite(led1,HIGH);
    digitalWrite(led2,LOW);
  }
  else{
    digitalWrite(led1,LOW);
    digitalWrite(led2,HIGH);
  }

display.display();

  delay(100);
}






























# EchoVision-ESP32

EchoVision-ESP32 is an ESP32 project that measures distance using an HC-SR04 ultrasonic sensor and displays real-time readings on a 128×64 SSD1306 OLED display.

## Features
- 📏 Real-time distance measurement (cm)
- 📺 OLED display with readable interface
- 🎛️ Potentiometer input with mapped values (0–100)
- 🔘 Push-button status detection
- 💡 Dual LED indication based on distance threshold
- 🖥️ Serial Monitor output for debugging

## Hardware Used
- ESP32 Development Board
- HC-SR04 Ultrasonic Sensor
- SSD1306 128×64 I2C OLED Display
- Potentiometer
- Push Button
- 2 LEDs
- Breadboard and Jumper Wires

## Pin Configuration
| Component | ESP32 Pin |
|-----------|-----------|
| HC-SR04 Trigger | GPIO 5 |
| HC-SR04 Echo | GPIO 18 |
| Push Button | GPIO 4 |
| LED 1 | GPIO 2 |
| LED 2 | GPIO 23 |
| Potentiometer | GPIO 15 |
| OLED (I2C) | SDA/SCL (Default ESP32 I2C Pins) |

## Working
- Measures the distance using the HC-SR04 ultrasonic sensor.
- Displays the measured distance on the OLED screen.
- Reads the potentiometer and maps its value from 0–100.
- Detects the push-button state (Pressed/Not Pressed).
- Turns on LED 1 when the distance is greater than 80 cm.
- Turns on LED 2 when the distance is less than or equal to 80 cm.
- Sends all readings to the Serial Monitor for debugging.

## Libraries Used
- Wire
- Adafruit GFX
- Adafruit SSD1306
- FreeSerif Font (Adafruit GFX Fonts)

## Future Improvements
- Add buzzer alerts for close objects.
- Implement adjustable distance thresholds.


# EchoVision-ESP32

## Smart Ultrasonic Vision System Powered by ESP32

EchoVision is an embedded sensing project that brings distance awareness to life using the ESP32 microcontroller. The system uses ultrasonic wave technology to detect object distance and presents real-time information through an OLED visual interface with intelligent LED feedback.

Designed as a compact IoT-ready prototype, EchoVision demonstrates the integration of sensor data acquisition, graphical display handling, analog input processing, and user interaction in an embedded environment.

## ✨ Key Highlights

🚀 **Real-Time Object Detection**
- Uses an HC-SR04 ultrasonic sensor to measure object distance accurately.
- Continuously processes and updates sensor readings.

🖥️ **Interactive OLED Dashboard**
- Displays live distance values and user input data.
- Provides a simple visual monitoring interface.

💡 **Smart Visual Alerts**
- Dual LED indication provides instant distance-based feedback.
- Helps identify safe and close-range object conditions.

🎛️ **Analog Control Interface**
- Potentiometer input enables adjustable parameter handling.
- Demonstrates ADC reading and value mapping on ESP32.

🔘 **User Interaction**
- Push-button input allows manual control and status detection.

## 🛠️ Technologies Used

- ESP32 Microcontroller
- Embedded C/C++ Programming
- Ultrasonic Sensing
- I2C OLED Graphics Interface
- Analog Signal Processing
- Digital Input/Output Control

## ⚙️ System Workflow

1. Ultrasonic sensor sends sound waves and captures the returning echo.
2. ESP32 calculates the distance from the measured travel time.
3. Data is displayed on the OLED screen in real time.
4. Distance thresholds control LED indicators.
5. User inputs are processed through button and potentiometer controls.

## 🌱 Future Development Ideas

- Wireless monitoring using ESP32 Wi-Fi capabilities
- Mobile dashboard integration
- Data logging and analytics
- Adjustable smart warning zones
- AI-based object detection integration

## 📌 Project Vision

EchoVision explores the foundation of smart sensing systems by combining hardware interfacing, real-time visualization, and embedded intelligence into a compact ESP32 platform.
- Add Wi-Fi/IoT monitoring using ESP32.
- Log sensor data to the cloud or SD card.
