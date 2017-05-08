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
![alt tag](http://i.imgur.com/Ojxa6LU.png)

## Code
Color Sensor:
```javascript
/*
  BM017_Arduino_color_sensing:  This program interfaces to the AMS TCS34725 color light
  to digital converter IC.  It uses the Arduino I2C interface.

  Schematics associated with the BM017 module may be used for hardware wiring information.
  See the user datasheet at www.solutions-cubed.com for additional information.

  This code is edited by Nino Panes from the school of California College of the Arts (npanes@cca.edu)

*/

#include "Adafruit_TCS34725.h"

//include Neopixel n.
//include Avr/power.h n.
//end include Avr/power.h n.
//define PIN 6 n.
#include <Adafruit_NeoPixel.h>
#include <avr/power.h>

//For neopixels to register. Make sure the middle is connected to this pin. n.
#define PIN 6

// How many NeoPixels are attached to the Arduino?
#define NUMPIXELS      3

//end include Neopixel n.
//end include Avr/power.h n.
//define PIN 6 n.

// When we setup the NeoPixel library, we tell it how many pixels, and which pin to use to send signals.
// Note that for older NeoPixel strips you might need to change the third parameter--see the strandtest
// example for more information on possible values.
//Adding Strip code. n.
Adafruit_NeoPixel pixels = Adafruit_NeoPixel(NUMPIXELS, PIN, NEO_GRB + NEO_KHZ800);
//Adafruit_NeoPixel strip = Adafruit_NeoPixel(NUMPIXELS, PIN, NEO_GRB + NEO_KHZ800);
int delayval = 20; // delay for half a second

//#include <Wire.h>


byte i2cWriteBuffer[10];
byte i2cReadBuffer[10];

#define SensorAddressWrite 0x29 //
#define SensorAddressRead 0x29 // 
#define EnableAddress 0xa0 // register address + command bits
#define ATimeAddress 0xa1 // register address + command bits
#define WTimeAddress 0xa3 // register address + command bits
#define ConfigAddress 0xad // register address + command bits
#define ControlAddress 0xaf // register address + command bits
#define IDAddress 0xb2 // register address + command bits
#define ColorAddress 0xb4 // register address + command bits


/* Initialise with default values (int time = 2.4ms, gain = 1x) */
// Adafruit_TCS34725 tcs = Adafruit_TCS34725();

/* Initialise with specific int time and gain values */
Adafruit_TCS34725 tcs = Adafruit_TCS34725(TCS34725_INTEGRATIONTIME_700MS, TCS34725_GAIN_1X);

void Writei2cRegisters(byte numberbytes, byte command)
{
  byte i = 0;

  Wire.beginTransmission(SensorAddressWrite);   // Send address with Write bit set
  Wire.write(command);                          // Send command, normally the register address
  for (i = 0; i < numberbytes; i++)                 // Send data
    Wire.write(i2cWriteBuffer[i]);
  Wire.endTransmission();

  delayMicroseconds(0);      // allow some time for delay
}

/*
  Send register address to this function and it returns byte value
*/
byte Readi2cRegisters(int numberbytes, byte command)
{
  byte i = 0;

  Wire.beginTransmission(SensorAddressWrite);   // Write address of read to sensor
  Wire.write(command);
  Wire.endTransmission();

  delayMicroseconds(0);      // allow some time for delay

  Wire.requestFrom(SensorAddressRead, numberbytes);  // read data
  for (i = 0; i < numberbytes; i++)
    i2cReadBuffer[i] = Wire.read();
  Wire.endTransmission();

  delayMicroseconds(0);      // allow some time for delay
}

void init_TCS34725(void)
{
  i2cWriteBuffer[0] = 0x10;
  Writei2cRegisters(1, ATimeAddress);   // RGBC timing is 256 - contents x 2.4mS =
  i2cWriteBuffer[0] = 0x00;
  Writei2cRegisters(1, ConfigAddress);  // Can be used to change the wait time
  i2cWriteBuffer[0] = 0x00;
  Writei2cRegisters(1, ControlAddress); // RGBC gain control
  i2cWriteBuffer[0] = 0x03;
  Writei2cRegisters(1, EnableAddress);   // enable ADs and oscillator for sensor
}

//Check if color sensor is responding
void get_TCS34725ID(void)
{
  Readi2cRegisters(1, IDAddress);
  if (i2cReadBuffer[0] = 0x44)
    Serial.println("TCS34725 is present");
  else
    Serial.println("TCS34725 not responding");
}

/*
  Reads the register values for clear, red, green, and blue.
*/
void get_Colors(void)
{
  unsigned int clear_color = 0;
  unsigned int red_color = 0;
  unsigned int green_color = 0;
  unsigned int blue_color = 0;

  //yellow n.
  unsigned int yellow_color = 0;
  //end yellow n.

  
//To register color from sensor
  Readi2cRegisters(8, ColorAddress);
  clear_color = (unsigned int)(i2cReadBuffer[1] << 8) + (unsigned int)i2cReadBuffer[0];
  red_color = (unsigned int)(i2cReadBuffer[3] << 8) + (unsigned int)i2cReadBuffer[2];
  green_color = (unsigned int)(i2cReadBuffer[5] << 8) + (unsigned int)i2cReadBuffer[4];
  blue_color = (unsigned int)(i2cReadBuffer[7] << 8) + (unsigned int)i2cReadBuffer[6];

  //Readi2cRegisters Yellow n.
  yellow_color = 05 * (red_color + green_color);
  //End Readi2cRegisters Yellow n.


  // Basic RGB color differentiation can be accomplished by comparing the values and the largest reading will be
  // the prominent color

  if ((red_color > blue_color) && (red_color > green_color))
    Serial.println("detecting red");
  else if ((green_color > blue_color) && (green_color > red_color))
    Serial.println("detecting green");
  else if ((blue_color > red_color) && (blue_color > green_color))
    Serial.println("detecting blue");

//  //Detect Yellow. This is left here for future tweaking to get the sensor to respond to the color Yellow n.
//  else if ((red_color) && (green_color) * 05)
//    Serial.println("detecting yellow");
//  //End Detect Yellow n.

  else
    Serial.println("color not detectable");

}


void setup() {


  Serial.begin(9600);

  if (tcs.begin()) {
    Serial.println("Found sensor");
  } else {
    Serial.println("No TCS34725 found ... check your connections");
    while (1);
  }

  // This initializes the NeoPixel library. This was added by Nino to this spot
  //to enable this code to activate neopixels. n.
  pixels.begin();
  pixels.show(); // This sends the updated pixel color to the hardware.
  //  strip.begin();
  //strip.show(); // Initialize all pixels to 'off' //Change this back to pixels later n.
}

void loop()

{

  //  analogWrite (colorMotor, 55);
  unsigned int clear_color = 0;
  unsigned int red_color = 0;
  unsigned int green_color = 0;
  unsigned int blue_color = 0;

  //yellow n.
  unsigned int yellow_color = 0;
  //end yellow n.


  //Adafruit Color Library n.
  Readi2cRegisters(8, ColorAddress);
  clear_color = (unsigned int)(i2cReadBuffer[1] << 8) + (unsigned int)i2cReadBuffer[0];
  red_color = (unsigned int)(i2cReadBuffer[3] << 8) + (unsigned int)i2cReadBuffer[2];
  green_color = (unsigned int)(i2cReadBuffer[5] << 8) + (unsigned int)i2cReadBuffer[4];
  blue_color = (unsigned int)(i2cReadBuffer[7] << 8) + (unsigned int)i2cReadBuffer[6];
  uint16_t r, g, b, c, colorTemp, lux;

  //Calculate Value
  tcs.getRawData(&r, &g, &b, &c);
  colorTemp = tcs.calculateColorTemperature(r, g, b);
  lux = tcs.calculateLux(r, g, b);



  //Adafruit Color Library n.

  Serial.print("Color Temp: "); Serial.print(colorTemp, DEC); Serial.print(" K - ");
  Serial.print("Lux: "); Serial.print(lux, DEC); Serial.print(" - ");
  Serial.print("R: "); Serial.print(r, DEC); Serial.print(" ");
  Serial.print("G: "); Serial.print(g, DEC); Serial.print(" ");
  Serial.print("B: "); Serial.print(b, DEC); Serial.print(" ");
  Serial.print("C: "); Serial.print(c, DEC); Serial.print(" ");
  Serial.println(" ");

  //Statements to call which color is being detected by the color sensor. n.

  if ((red_color > blue_color) && (red_color > green_color)) {
    pixels.setPixelColor(0, pixels.Color(100, 0, 0)); // Moderately bright Red color.
    pixels.setPixelColor(1, pixels.Color(0, 0, 0)); // No Green color.
    pixels.setPixelColor(2, pixels.Color(0, 0, 0)); // No Blue color.
    pixels.show(); // This sends the updated pixel color to the hardware.
    delay(delayval); // Delay for a period of time (in milliseconds).
    Serial.println("detecting red");
    //Serial.println("1"); //n.
  }
  else if ((green_color > blue_color) && (green_color > red_color)) {
    pixels.setPixelColor(0, pixels.Color(0, 0, 0)); // Moderately bright Red color.
    pixels.setPixelColor(1, pixels.Color(0, 100, 0)); // No Green color.
    pixels.setPixelColor(2, pixels.Color(0, 0, 0)); // No Blue color.
    pixels.show(); // This sends the updated pixel color to the hardware.
    delay(delayval); // Delay for a period of time (in milliseconds).
    Serial.println("detecting green");
    //Serial.println("2"); //n.
  }
  else if ((blue_color > red_color) && (blue_color > green_color)) {
    pixels.setPixelColor(0, pixels.Color(0, 0, 0)); // Moderately bright Red color.
    pixels.setPixelColor(1, pixels.Color(0, 0, 0)); // No Green color.
    pixels.setPixelColor(2, pixels.Color(0, 0, 100)); // No Blue color.
    pixels.show(); // This sends the updated pixel color to the hardware.
    delay(delayval); // Delay for a period of time (in milliseconds).
    Serial.println("detecting blue");
    //Serial.println("3"); //n.
  }
  else {
    pixels.setPixelColor(0, pixels.Color(100, 0, 0)); // Moderately bright Red color.
    pixels.setPixelColor(1, pixels.Color(0, 100, 0)); // No Green color.
    pixels.setPixelColor(2, pixels.Color(0, 0, 100)); // No Blue color.
    pixels.show(); // This sends the updated pixel color to the hardware.
    delay(delayval); // Delay for a period of time (in milliseconds).
    Serial.println("color not detectable");
    //Serial.println("4"); //n.

  }
}

```



