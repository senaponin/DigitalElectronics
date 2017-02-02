# DigitalElectronics

HW 1

02.02.17
Nino Panes
Week 2

Coke Piano and Launchpad:

The Coke musical instrument project connects Arduino to multiple coke cans that when touched, produces a different sound or clip. It almost acts like a Launchpad device which is an equipment most DJs use to create mashups, remixes, and other things. They even have a black coke can that is representative of sound pack being changed.

https://www.youtube.com/watch?time_continue=6&v=Ttm62RBdOuo
http://www.makeuseof.com/tag/musical-projects-arduino-beginners/

3 Sensors:

Tilt Sensor: Tilt sensors allow you to detect orientation or inclination. Inside them is a free moving conductive mass that will orient itself relative to the gravity of the Earth. Hence, up and down.

PIR Sensor: PIR sensors allow you to sense motion.They are pyroelectric sensors that can detect IR spectrum. They are cut into two halves which are made to cancel each other out. This is the basic reason they can detect motion.

Force Sensitive Sensor: FSRs are basically a resistor that changes its resistive value (in ohms Ω) depending on how much it is pressed.


Code 4: Making Sounds with Buttons:

int button1=6; // button1 is connected to pin6 void loop()
int button2=7; // button2 is connected to pin7
int buttonstatus1=0; //variable to save the status of button1
int buttonstatus2=0; //variable to save the status of button2

void setup()
{
pinMode(button1, INPUT); //button1 is an input
pinMode(button2, INPUT); //button2 is an input
}

void loop()     << It worked, this just needed a paranthesis. 
{
buttonstatus1 = digitalRead(button1); //get status from button1 (HIGH or LOW)
buttonstatus2 = digitalRead(button2); //get status from button2 (HIGH or LOW)

if (buttonstatus1 == HIGH) //If button1 gets pushed..
{
tone(8, 100); //...output of a tone with a pitch of 100...
delay (1000); //...one second long...
noTone(8); //...than turn it off
}

if (buttonstatus2 == HIGH) //If button2 gets pushed..
{
tone(8, 200); //...output of a tone with a pitch of 200...
delay (1000); //...one second long...
noTone(8); //...than turn it off
}
}
