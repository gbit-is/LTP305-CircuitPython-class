# LTP305 For Pi Pico (circuitPython)

Was ist das ?  
Pimoroni has a [product](https://shop.pimoroni.com/products/led-dot-matrix-breakout) named "LTP-305" that I think is pretty cool, bought it and then I realised their code examples are all for raspberry pi, not raspberry pi pico.  

It's an I2C device so, they can be annoying to deal with but having their code for reference, it wasn't much of a hazzle to write a basic class to interface with it (this is not a recreation of their full class, just the parts I needed for my own project, which is just interfacing with it and codes to write 0-9)

# Usage example
### Import and init 

If you are using GPIO 16/17 for I2C and haven't modified the I2C address  
```
from LPT305 import lpt305  
lpt = lpt305() 
```

If you need to specify GPIO's and/or I2C address  
```
from LPT305 import lpt305 
lpt = lpt305(sda=board.GP0,scl=board.GP1,i2cAddress=0x61)
```

### write 0-9 to screen/s
(When writing on the screens, it get's added to the buffer, to refresh the screen an update() command is needed)

```
lpt.writeCharPair(4,2)  # writes 4 to the left screen, 2 to the right
lpt.writeChar("l",4)    # writes 4 to the left screen
lpt.writeChar("r",2)    # writes 2 to the right screen
lpt.writechar("lr",8)   # writes 8 to both screens
lpt.update()            # nothing happens on the screens until an update 
                        # command is issued and it would print the last command written to the buffer
lpt.clear()             # clear the screen (includes update() because, funky behaviour without it)
```

#### write custom characters to screen/s
```
smileyFace = [
  0b00000,
  0b00000,
  0b01010,
  0b00000,
  0b10001,
  0b01110,
  0b00000,
]

ltp.write("l",smileyFace)   # writes the smiley face to the left screen
ltp.update()                # so the screen get's updated
```
While the example above makes the custom characters readable in the code, it takes up a lot of space and in my opinion makes the code a lot less readable overall. To solve this each binary line can be written as an interger and the code takes care of binary conversion and padding 

The following code defines a smiley face and a sad face and prints one the left display and one to the right 

```
smileyFace = [0, 0, 10, 0, 17, 14, 0]
sadFace = [0, 0, 10, 0, 14, 17, 0]
ltp.write("l",smileyFace)
ltp.write("r",sadFace)
```

#### Misc
```
ltp.brightness(1)     # screen brightness so it is barely visible
ltp.brightness(127)   # screen brightness at full
ltp.brightness(64)    # Not too cold, not too warm 

ltp.sencCommand(address,frame)  # Send a raw I2c address/frame command to the device

```

