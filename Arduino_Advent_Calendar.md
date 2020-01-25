# Arduino Advent Calendar

In preparation of the holiday season I made an automated advent calendar together with my girlfriend. This calendar consists of 24x small drawers, one of which will open each day of December leading up to Christmas to reveal a small present or candy. Luckily, we could source the enclosure and drawers and did not need to start from scratch, however a lot of work was needed to make it presentable and automated.

The idea to use a single micro-servo for each drawer seemed the simplest, and with these servo's costing only around €1 each it would not break the bank. To keep track of time an I2C RTC module was chosen. To control it all we used an Arduino Nano. At first the idea was to control the servo's by switching their power inputs using a grid of 5x5 transistors to switch both V+ and GND connections in a way to pick one servo that would be powered, then send the signal to all servo's resulting in only one moving. However, after some experimentation the voltage drop of the 5V source across the transistors resulted in too low of a voltage for the servo's to operate. As a solution we purchased 2x PWM I2C servo drivers, these can drive up to 16x servos and can be controlled over I2C.

The challenge while programming was mainly to open the drawers smoothly while keeping power consumption to a minimum, as we only used the 5V rail from the Arduino board. It took a while to figure out how to disable the idle holding current of the servo's, but eventually digging into the control library gave the answer. Features for programming the RTC and a manual testing were added too. The basic principle of operation is the main loop, which periodically checks the RTC time and opens a drawer for each day in December, e.g. drawer 1 for December 1st.

In the end, the most challenging part was making the hand made wooden drawers not bind in the hand made wooden frame. If they did the servo would stall and draw enough current to reset the Arduino. With some manual testing and fine adjustment most drawers ended up opening smoothly.

## Parts used
Hardware:
* SG90 Micro-Servo (24x)
* PCA9685 16 Channel I2C PWM Servo Driver (2x)
* TinyRTC I2C
* Arduino Nano (clone)
* USB power supply
(* Potentiometer for testing)

Software:
* [Arduino IDE](https://www.arduino.cc/en/main/software)
* [ArduinoAdvent sketch](https://github.com/JJ-White/ArduinoAdvent)
* [Adafruit PWM Servo Driver Library](https://github.com/adafruit/Adafruit-PWM-Servo-Driver-Library)
* [RTClib](https://github.com/adafruit/RTClib)
