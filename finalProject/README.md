**Table of Contents**  

- [Final Project](#)
	- [Proposal](#)
		- [How to determine the exact color?](#)
		- [How to give visual cues that the right color is being detected?](#)
	- [Research](#)

# Final Project 
This final project will be done by Nino Panes for the Spring '17 class of "Digital Electronics" at California College of the Arts.
For further inquiries on the project, please email me at npanes@cca.edu.

## Proposal
For this project, I wish to continue my work from last semester's Studio 1 class. In that class we created a mock up of a ring that helped a colorblind person see the shades of blue, green, yellow and red (etc.)that she/he would have not otherwise. This would be done using a color sensor that relays information to a vibrator that would act as a haptic feedback to notify the user that the color they wish to choose. 

Conclusion: However for this project, the end result is a feedback using neopixel lights to determine what color is being detected based on position.

I used Adafruit main board to create a wearable device that is easy to translate into either haptic feedback or light. Arduino's board was
too bulky to be able to give the user the experience I wanted to convey.


## Materials Used
-FLORA main board 
-3 FLORA RGB NeoPixels 
-Flora color sensor 
-3xAAA battery pack or 150mAh LiPo battery


The perceived factors that make this project hard are as follows: 
### How to determine the exact color?
	Color sensors are hard to control with the input they are receiving. How do we calibrate for 
	the appropriate color?
### How to give visual cues that the right color is being detected? How does the user choose color?
	In my team's original idea, we used an app that connected to the ring via bluetooth so that 
	the user could see and choose. I would need to reiterate this interaction into a different model.

## Research

My preliminary research involved seeing if the sensor is available and if there are different kinds.My search led me to sites like 
Adafruit or similar. (This section will be updated when I determine which sensor to use moving forward).

The next part involved looking at tutorials that would help me in formulating a plan of attack in tackling this endeavor. The list below is as follows:

* http://howtomechatronics.com/tutorials/arduino/arduino-color-sensing-tutorial-tcs230-tcs3200-color-sensor/.
		
		(A simple calibration technique for basic color sensing using using Arduino and the 
		TCS230 / TCS3200 Color Sensor)
		
* https://circuitdigest.com/microcontroller-projects/arduino-color-sensor-tcs3200.
	
		(In this project they are going to interface TCS3200 color sensor with Arduino UNO. 
		TCS3200 is a color sensor which can detect any number of colors with right programming. 
		TCS3200 contains RGB (Red Green Blue) arrays. They also use a screen that shows the exact 
		RGB values.
		
* http://www.instructables.com/id/Introduction-29/.
	
		(Color sensor project using LEDs for feedback)

The above list is not exhaustive and will change as this project progresses and I dive deeper into modifying the device to fit the specific criteria for the purpose of my project.


## Minimal Viable Project
At the very list some sort of device(wearable or portable) that can be carried around by the person who has color blindness that will enable them to discern colors when confronted with the task of navigating through the wolrd and having to choose certain objects that are identical in color to them. Attached is some sort of sensor or notification mechanism that allows them to know which color is being read.

## Optional Add-ons
Some sort of digital display that allows the user to choose the color or input the desired color that can customize to their needs.

## Flow Chart
![alt tag](http://i63.tinypic.com/6ozlfn.png)

# Circuit Diagram
![alt tag](http://imgur.com/a/vZGqT)

## Code
Color Sensor:
```javascript
int OutPut= 10;//naming pin10 of uno as output
unsigned int frequency = 0;
 
#include <LiquidCrystal.h>
// initialize the library with the numbers of the interface pins
LiquidCrystal lcd(8, 9, 7, 11, 12, 13);//RS,EN,D4,D5,D6,D7
 
void setup()
{
                // set up the LCD's number of columns and rows
                lcd.begin(16, 2);
 
                pinMode(2, OUTPUT);
                pinMode(3, OUTPUT);//PINS 2, 3,4,5 as OUTPUT
                pinMode(4, OUTPUT);
                pinMode(5, OUTPUT);
                pinMode(10, INPUT);//PIN 10 as input
 
                digitalWrite(2,HIGH);
                digitalWrite(3,LOW);//setting frequency selection to 20%
}
void loop()
{
                lcd.print("R=");//printing name
                digitalWrite(4,LOW);
                digitalWrite(5,LOW);//setting for RED color sensor
                frequency = pulseIn(OutPut, LOW);//reading frequency
                lcd.print(frequency);//printing RED color frequency
                lcd.print("  ");
                lcd.setCursor(7, 0);//moving courser to position 7
                delay(500);
               
               lcd.print("B=");// printing name
                digitalWrite(4,LOW);
                digitalWrite(5,HIGH);// setting for BLUE color sensor
                frequency = pulseIn(OutPut, LOW);// reading frequency
                lcd.print(frequency);// printing BLUE color frequency
                lcd.print("  ");
                lcd.setCursor(0, 1);
                delay(500);
               
               lcd.print("G=");// printing name
                digitalWrite(4,HIGH);
                digitalWrite(5,HIGH);// setting for GREEN color sensor
                frequency = pulseIn(OutPut, LOW);// reading frequency
                lcd.print(frequency);// printing GREEN color frequency
                lcd.print("    ");
                lcd.setCursor(0, 0);
                delay(500);        
}            
```



