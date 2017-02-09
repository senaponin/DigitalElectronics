#HW - 3 
### 1. Draw a sketch of your instrument
(attached in this folder under Musical Instrument sketch in musical_instrument folder)

### 2. Describe the overall concept

From day one, I've always want to improve the graduate studio space at CCA. I believe that wanting to put as much interactive experience in the vicinity will inspire others to do the same. It will also promote discussion, sharing of ideas and the overall excitement around the area. It is not enough to just go to your graduate studio space and do work; it needs to be a space of inspiration.

With that in mind, I want to create a musical instrument out of the stairs in our studio. There are three modes of entry and they are all staircases - this will force the graduate students to interact with the musical instrument.

It will involve an analog pressure sensor for each step that relays to different sounds when triggered. Depending on how heavy or light the applied pressure will create different sounds. For example, a heavier stomp = lower sounds.

### 3. Create a very rough schematic to the best of your ability
(attached in this folder under Musical Instrument shematics in musical_instrument folder)

### 4. Describe what your program will do, to the best of your ability
The program will build upon this system:

https://gist.github.com/timpulver/5ba4a29cddd543b4a900

I will connect the pressure sensor/arduino into processing to draw forth musical notes and play it back for every time people stepped on the sensor. I have not yet fully researched the process however the reasoning to connect this arduino code to processing makes sense as I am interested in making sounds come out lower depending on pressure; it will be (to my knowledge) something I am familiar in using.

The bottom code is taken from the website and will be the basis of my experimentation into creating my musical steps. This will hopefully make it more fun to walk up to the studio in the days that they are implemented.

```javascript
/*
  AnalogReadSerial
  Reads an analog input on pin 0, prints the result to the serial monitor.
  Attach the center pin of a potentiometer to pin A0, and the outside pins to +5V and ground.

 This example code is in the public domain.
 */

// the setup routine runs once when you press reset:
void setup() {
  // initialize serial communication at 9600 bits per second:
  Serial.begin(9600);
}

// the loop routine runs over and over again forever:
void loop() {
  // read the input on analog pin 0:
  int sensorValue = analogRead(A0);
  // print out the value you read:
  Serial.println(sensorValue);
  if(sensorValue > 200) {
    Serial.println("T"); // send the letter T (for Trigger) once the sensor value is bigger than 200  
  }
  delay(1);        // delay in between reads for stability
}
```
Alternatively: http://www.instructables.com/id/Piano-Stairs-with-Arduino-and-Raspberry-Pi/
