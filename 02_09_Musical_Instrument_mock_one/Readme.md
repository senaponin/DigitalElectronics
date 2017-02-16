# Build a mock-up of a simplified version of your musical instrument

Open Link as Reference: http://teachmetomake.com/wordpress/arduino-audio 

Debounce Tutorial: https://www.arduino.cc/en/Tutorial/Debounce

Schematic: (Found under this folder named: DrawnSchematic

Arduino Code:
```Javascript
/*
 DigitalReadSerial
 Reads a digital input on pin 2, prints the result to the serial monitor 
 
 This example code is in the public domain.
 */

// digital pin 2 has a pushbutton attached to it. Give it a name:
int pushButton = 2;

// the setup routine runs once when you press reset:
void setup() {
 // initialize serial communication at 9600 bits per second:
 Serial.begin(9600);
 // make the pushbutton's pin an input:
 pinMode(pushButton, INPUT);
}

// the loop routine runs over and over again forever:
void loop() {
 // read the input pin:
 int buttonState = digitalRead(pushButton);
 // print out the state of the button:
 Serial.println(buttonState);
 delay(1); // delay in between reads for stability
}
```
Processing Code:

```Javascript
/**
 * This sketch demonstrates how to play a file using the Minim  
 * AudioPlayer object.
 * For more information about Minim and additional features, 
 * visit http://code.compartmental.net/minim/
 */

import ddf.minim.*;
import processing.serial.*;

Minim minim;
AudioPlayer player; 
Serial myPort; // The serial port

void setup()
{
 // List all the available serial ports
 println(Serial.list());
 // I know that the first port in the serial list on my mac
 // is always my Arduino, so I open Serial.list()[0].
 // Open whatever port is the one you're using.
 myPort = new Serial(this, Serial.list()[2], 9600);
 // don't generate a serialEvent() unless you get a newline character:
 myPort.bufferUntil('\n');
 
 // we pass this to Minim so that it can load files from the data 
 // directory
 minim = new Minim(this);
 
 // loadFile will look in all the same places as loadImage does.
 // this means you can find files that are in the data folder and the 
 // sketch folder. you can also pass an absolute path, or a URL e.g. 
 // https://www.freesound.org/people/groovy_turnokk/sounds/147504/

 player = minim.loadFile("feels.mp3");
}

void draw () {
 // everything happens in the serialEvent()
}

void serialEvent (Serial myPort) {
 // get the ASCII string:
 String inString = myPort.readStringUntil('\n');
 
 if (inString != null) {
 // trim off any whitespace:
 inString = trim(inString);

 // convert to an int and map to the screen height:
 float inByte = float(inString); 
 
 if( inByte == 1 )
 {
 // Rewind the file file in case it's not at the begining
 // play the file from start to finish. 
 // To play again, first call player.rewind();
 // To play for a shorter time, use play(millis).
 player.rewind();
 player.play();
 }
 }
}
```
