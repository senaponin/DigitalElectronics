#Hw - 4
### Create a version of arduinoMinimProcessing which reads from two sensors

This goes on your Arduino Program.
Open this link as reference: https://www.arduino.cc/en/Tutorial/Graph

```Javascript
/*
  Graph

 A simple example of communication from the Arduino board to the computer:
 the value of analog input 0 is sent out the serial port.  We call this "serial"
 communication because the connection appears to both the Arduino and the
 computer as a serial port, even though it may actually use
 a USB cable. Bytes are sent one after another (serially) from the Arduino
 to the computer.

 You can use the Arduino serial monitor to view the sent data, or it can
 be read by Processing, PD, Max/MSP, or any other program capable of reading
 data from a serial port.  The Processing code below graphs the data received
 so you can see the value of the analog input changing over time.

 The circuit:
 Any analog input sensor is attached to analog in pin 0.

 created 2006
 by David A. Mellis
 modified 9 Apr 2012
 by Tom Igoe and Scott Fitzgerald

 This example code is in the public domain.

 http://www.arduino.cc/en/Tutorial/Graph
 */

void setup() {
  // initialize the serial communication:
  Serial.begin(9600);
}

void loop() {
  // send the value of analog input 0:
  Serial.println(analogRead(A0));
  // wait a bit for the analog-to-digital converter
  // to stabilize after the last reading:
  delay(2);
}
```
This goes onto your Processing Sketch
``` Javascript
import processing.serial.*;
import ddf.minim.signals.*;

Serial myPort;        // The serial port
float inByte = 0;
float freq;

import ddf.minim.*;
import ddf.minim.ugens.*;

Minim       minim;
AudioOutput out;
SineWave sine;   // a function to generate the values of a sine wave

void setup () {
  // Open whatever port is the one you're using.
  myPort = new Serial(this, Serial.list()[2], 9600); // 2 for the number of ports available on my computer. CHANGE THIS!

  // don't generate a serialEvent() unless you get a newline character:
  myPort.bufferUntil('\n');

  minim = new Minim(this);

  // Connect the output of Minim to the audio output 
  // of my laptop
  out = minim.getLineOut(Minim.STEREO);  

  // create a sine wave oscillator
  // Frequency = 440 Hz
  // Amplitude = 0.5
  // Use the same sample rate as my output  
  sine = new SineWave(440, 0.5, out.sampleRate()); 

  //connect the sine wave generator to my audio output
  out.addSignal(sine);
}
void draw () {
  sine.setFreq( freq );
}

void serialEvent (Serial myPort) {
  // get the ASCII string:
  String inString = myPort.readStringUntil('\n');

  if (inString != null) {
    println(inString);
    // trim off any whitespace:
    inString = trim(inString);
    // convert to an int and map to the screen height:
    inByte = float(inString);
    freq = map(inByte, 0, 1023, 110, 880);
  }
}
```
