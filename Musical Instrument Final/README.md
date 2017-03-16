# Final Submission of Musical Instrument 

This is my final submission for the Digital Electronics class of a musical instrument. The problem I had before was how to 
differentiate between button switches when Arduino & Processing was communicating. After a few trial and errors, the key part was
changing the 0 & 1 that was being written in Arduino into different numbers by means of an "if" statement.

Open Link as Reference: http://teachmetomake.com/wordpress/arduino-audio 

Debounce Tutorial: https://www.arduino.cc/en/Tutorial/Debounce

Schematic: (Made by me using Fritzing)

Arduino Code:
```Javascript
/*
  DigitalReadSerial
  Reads a digital input on pin 2, prints the result to the serial monitor

  This example code is in the public domain.
*/

// digital pin 2 has a pushbutton attached to it. Give it a name:
int pushButton1 = 2;
int pushButton2 = 3;
int pushButton3 = 4;
int pushButton4 = 5;

int buttonnum = 0;

// the setup routine runs once when you press reset:
void setup() {
  // initialize serial communication at 9600 bits per second:
  Serial.begin(9600);
  // make the pushbutton's pin an input:
  pinMode(pushButton1, INPUT);
  pinMode(pushButton2, INPUT);
  pinMode(pushButton3, INPUT);
  pinMode(pushButton4, INPUT);
  //  pinMode(pushButton5, INPUT);
}

// the loop routine runs over and over again forever:
void loop() {
  // read the input pin:
  int buttonState1 = digitalRead(pushButton1);
  int buttonState2 = digitalRead(pushButton2);
  int buttonState3 = digitalRead(pushButton3);
  int buttonState4 = digitalRead(pushButton4);
  //  int buttonState5 = digitalRead(pushButton5);

  if (buttonState1 == 1) {
    buttonnum = 1;

  } else if (buttonState2 == 1) {
    buttonnum = 2;

  } else if (buttonState3 == 1) {
    buttonnum = 3;

  } else if (buttonState4 == 1) {
    buttonnum = 4;

  } else {
    buttonnum = 0;
  }

  Serial.println(buttonnum);
  delay(1);
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
AudioPlayer player1, player2, player3, player4; 
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

  player1 = minim.loadFile("G.wav");
  player2 = minim.loadFile("E.wav");
  player3 = minim.loadFile("D.wav");
  player4 = minim.loadFile("C.wav");
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

    if ( inByte == 1 )
    {
      // Rewind the file file in case it's not at the begining
      // play the file from start to finish. 
      // To play again, first call player.rewind();
      // To play for a shorter time, use play(millis).
      player1.play();
      //player1.rewind();
      player1.rewind();
    } else if (inByte==2) {
      player2.play();
      player2.rewind();
    } else if (inByte==3) {
      player3.play();
      player3.rewind();
    } else if (inByte==4) {
      player4.play();
      player4.rewind();
    }
  }
}
```
### Future Itirations:
The next step is to figure out the code so that when pressed, the music file waits until a certain time so that it doesn't keep looping while the switch is pressed. Another iteration is to figure out how to make the sound keep looping until the switch is pressed again.

Questions? Email me at npanes@cca.edu
