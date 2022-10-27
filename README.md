# CircuitPython
This repository will actually serve as a aid to help you get started with your own template.  You should copy the raw form of this readme into your own, and use this template to write your own.  If you want to draw inspiration from other classmates, feel free to check [this directory of all students!](https://github.com/chssigma/Class_Accounts).
## Table of Contents
* [Hello_CircuitPython](#Hello_CircuitPython)
* [CircuitPython_Servo](#CircuitPython_Servo)
* [CircuitPython_LCD](#CircuitPython_LCD)
---

## Hello_CircuitPython

### Description & Code
The assignment was to make your NeoPixel blink while running on CircuitPython and make sure that the Metro Express worked.

Here's how you make code look like code:

```python
from time import time
import board
import neopixel
import time

dot = neopixel.NeoPixel(board.NEOPIXEL, 1) #sets the pin for the neopixel
dot.brightness = 0.5 

print("Make it red!")

while True:
dot.fill((255, 0, 0)) #turns neopixel red
time.sleep(0.5) #pause 1/2 second
dot.fill((0,0,0)) #turn neopixel off
time.sleep(0.5)

```


Code Credit: [Grant Gastinger](https://github.com/ggastin30)
### Evidence


https://user-images.githubusercontent.com/91094422/192555526-bd92a678-54f8-4581-986d-d5b9983d4272.mp4



Video Credit: [Grant Gastinger](https://github.com/ggastin30)



### Wiring
There is no wirin needed because the NeoPixel is part of the board

### Reflection
* functions aren't available unless you import them and the files that you use need to be put into the library folder.
* CircuitPython is "while true" = void loop in Arduino.
* Serial monitor may not be as effective.



## CircuitPython_Servo

### Description & Code
Get a 180 degree servo to slowly sweep back and forth between 0 and 180 degrees. Then control the servo with buttons

```python
import time
import board
import pwmio
from adafruit_motor import servo

pwm = pwmio.PWMOut(board.D2, duty_cycle=2 ** 15, frequency=250) # create a PWMOut object on Pin A2.

my_servo = servo.Servo(pwm) # Create a servo object, my_servo.

while True:
    for angle in range(0, 180, 5):  # 0 - 180 degrees, 5 degrees at a time.
        my_servo.angle = angle #slowly changes angle by 5 degrees
        time.sleep(0.01)
    for angle in range(180, 0, -5): # 180 - 0 degrees, 5 degrees at a time.
        my_servo.angle = angle
        time.sleep(0.01)

```

Code Credit: [Grant Gastinger](https://github.com/ggastin30)
### Evidence

https://user-images.githubusercontent.com/91094422/193044111-5d46a9d9-b366-4e6e-9d3c-922e085cacc6.mp4


Video Credit: [Grant Gastinger](github.com/ggastin30)
### Wiring
![EliasWiring](https://user-images.githubusercontent.com/91094422/193046672-c67778a8-b683-4d67-8af1-8846cccd1db1.png)

Image Credit: [Elias Garcia](https://github.com/egarcia28/CircuitPython)

### Reflection
* A pins don't work, use D pins
* [this is the source of my code](https://learn.adafruit.com/circuitpython-essentials/circuitpython-servo)




## CircuitPython_DistanceSensor

### Description & Code
Use the HC-SR04 to measure the distance to an object and print that out to the serial monitor or LCD in cm.
Next, get will get the neopixel to turn red when the object is less than 5cm, blue when between 5 and 20cm, and green when farther than 20cm.
Make a GIF of the NeoPixel changing colors.

```python
import time
import board
import adafruit_hcsr04
import neopixel
import simpleio

sonar = adafruit_hcsr04.HCSR04(trigger_pin=board.D3, echo_pin=board.D2)
Mason = neopixel.NeoPixel(board.NEOPIXEL, 1)#connecting the neopixel on the board to the code, Mason = the neopixel
Mason.brightness = .5  #setting the brightness of the light, from 0-1 brightness
red = 0
blue = 0
green = 0

while True: #void loop
    try:
        cm = sonar.distance #measure sonar distance in cm
        print((sonar.distance))
        time.sleep(0.01)
        if cm < 5:
            Mason.fill((255, 0, 0))#red
        if cm > 5 and cm < 12.5: #fade red to blue at this interval
            red = simpleio.map_range(cm,5,12.5,255,0)
            blue = simpleio.map_range(cm,5,12.5,0,255)
            Mason.fill((red, 0, blue))
        if cm > 12.5 and cm < 20: #Blue to green at this interval
            blue = simpleio.map_range(cm,12.5,20,255,0)
            green = simpleio.map_range(cm,12.5,20,0,255)
            Mason.fill((0, green, blue))
        if cm > 20:
            Mason.fill((0, 255, 0))#green
    except RuntimeError: #If it doesn't work, try again
        print("Retrying!")
    time.sleep(0.1)
```
Code Credit: [Grant Gastinger](github.com/ggastin30)

### Evidence
https://user-images.githubusercontent.com/91094422/193284908-be352a88-619a-47df-9850-289edfa8f144.mp4

Video Credit: [Grant Gastinger](github.com/ggastin30)

### Wiring
![eLIAS3](https://user-images.githubusercontent.com/91094422/193285420-b9590fd5-7f85-4ac5-8543-de44b9a417bd.png)

Image Credit: [Elias Garcia](https://github.com/egarcia28/CircuitPython)

### Reflection
When dealing with if statements, instead of using brackets like in arduino, you need to make sure the code is indented correctly. If a line of code is not indented enough, it will not be run in the loop. The syntax for the map function is "variable = simpleio.map_range(measured variable, input min, input max, output min, output max). "try" is a loop that checks a bunch of if statements and if none of them work, it runs the "except RuntimeError:" loop. 

## CircuitPython_LCD

### Code

``` python
#Grant Gastinger
#lcdAssignment
#Uses an LCD to display the amount of times a button is clicked. Reverses if switch is flipped.
#Based off code from Engineering 3 canvas

import board
from lcd.lcd import LCD
from lcd.i2c_pcf8574_interface import I2CPCF8574Interface
import time
from digitalio import DigitalInOut, Direction, Pull

# get and i2c object
i2c = board.I2C()
btn = DigitalInOut(board.D2)
btn.direction = Direction.INPUT
btn.pull = Pull.UP
clickCount = 0

switch = DigitalInOut(board.D7)
switch.direction = Direction.INPUT
switch.pull = Pull.UP
# some LCDs are 0x3f... some are 0x27...
lcd = LCD(I2CPCF8574Interface(i2c, 0x3f), num_rows=2, num_cols=16)

lcd.print("Grant")

while True:
    if not switch.value: #If switch is not flipped...
        if not btn.value:
            lcd.clear() #clear lcd
            lcd.set_cursor_pos(0, 0) #move cursor to top left
            lcd.print("Click Count:")
            lcd.set_cursor_pos(0,13)
            clickCount = clickCount + 1 #add 1 to click count
            lcd.print(str(clickCount)) #str needed to print variables
        else:
            pass #end the loop
    else: #if switch is flipped...
        if not btn.value:
            lcd.clear()
            lcd.set_cursor_pos(0, 0)
            lcd.print("Click Count:")
            lcd.set_cursor_pos(0,13)
            clickCount = clickCount - 1
            lcd.print(str(clickCount))
        else:
            pass
    time.sleep(0.1) # sleep for debounce

```
Code Credit: [Grant Gastinger](github.com/ggastin30)
### Evidence

https://user-images.githubusercontent.com/91094422/193284908-be352a88-619a-47df-9850-289edfa8f144.mp4

Video Credit: [Grant Gastinger](github.com/ggastin30)

### Wiring
![eLIAS3](https://user-images.githubusercontent.com/91094422/193285420-b9590fd5-7f85-4ac5-8543-de44b9a417bd.png)

Image Credit: [Elias Garcia](https://github.com/egarcia28/CircuitPython)

### Reflection
* If statements won't work using brackets, you have to indent the code for it to work. 
* Map function = "variable = simpleio.map_range(measured variable, input min, input max, output min, output max"
* "try" just checks your the statements you write beside your code





## CircuitPython_LCD

### Description & Code
Use your fancy new LCD screen for output and make two inputs (buttons, switches, capacitive touch??)  

```python
import board
from lcd.lcd import LCD
from lcd.i2c_pcf8574_interface import I2CPCF8574Interface
import time
from digitalio import DigitalInOut, Direction, Pull

# get and i2c object
i2c = board.I2C()
btn = DigitalInOut(board.D2)
btn.direction = Direction.INPUT
btn.pull = Pull.UP
clickCount = 0

switch = DigitalInOut(board.D7)
switch.direction = Direction.INPUT
switch.pull = Pull.UP
# some LCDs are 0x3f... some are 0x27...
lcd = LCD(I2CPCF8574Interface(i2c, 0x3f), num_rows=2, num_cols=16)

lcd.print("Grant")

while True:
    if not switch.value: #If switch is not flipped...
        if not btn.value:
            lcd.clear() #clear lcd
            lcd.set_cursor_pos(0, 0) #move cursor to top left
            lcd.print("Click Count:")
            lcd.set_cursor_pos(0,13)
            clickCount = clickCount + 1 #add 1 to click count
            lcd.print(str(clickCount)) #str needed to print variables
        else:
            pass #end the loop
    else: #if switch is flipped...
        if not btn.value:
            lcd.clear()
            lcd.set_cursor_pos(0, 0)
            lcd.print("Click Count:")
            lcd.set_cursor_pos(0,13)
            clickCount = clickCount - 1
            lcd.print(str(clickCount))
        else:
            pass
    time.sleep(0.1) # sleep for debounce

```
Code Credit: [Grant Gastinger](github.com/ggastin30)

### Evidence
![image](https://user-images.githubusercontent.com/91094422/197212627-0e835803-2ad7-4bbc-863a-95a5b803c1dd.png)

Image Credit: [Elias Garcia](https://github.com/egarcia28/CircuitPython)

### Wiring
![image](https://user-images.githubusercontent.com/91094422/197210798-31bc93db-ff26-4a1f-b279-9e3f9c75e703.png)

Image Credit: [Elias Garcia](https://github.com/egarcia28/CircuitPython)

### Reflection
* Put the wires on diagonal sides while using push buttons
* Before printing, clear the LCD and set the cursor
