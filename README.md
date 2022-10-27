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

```python
Code goes here

```

### Evidence

Pictures / Gifs of your work should go here.  You need to communicate what your thing does.

### Wiring

### Reflection




## CircuitPython_LCD

### Description & Code

```python
Code goes here

```

### Evidence

Pictures / Gifs of your work should go here.  You need to communicate what your thing does.

### Wiring

### Reflection





## NextAssignment

### Description & Code

```python
Code goes here

```

### Evidence

### Wiring

### Reflection
