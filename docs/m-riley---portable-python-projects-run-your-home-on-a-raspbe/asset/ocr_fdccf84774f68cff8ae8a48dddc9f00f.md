# We’ve Got Movement

With the smart bulb and light and motion sensor programmatically confirmed working, we can combine these together to turn the light on in low-light situations whenever motion is detected.

We’ll alter the Hue smart plug code from the previous Hue project and swap out the temperature code with our motion test code. But unlike the Hue Fan project, we shouldn’t immediately turn off the light when motion is no longer detected; otherwise our light will just flicker on briefly and turn off seconds later.

We’ll do so in our script by keeping the light on for five minutes. And we’ll only turn on the smart bulb when there isn’t already enough light illuminating the room:

```
hueautolight/hueautolight.py

from phue import Bridge
import smbus
import time

huebridge = Bridge('YOUR_HUE_BRIDGE_IP_GOES_HERE')

DEVICE_BUS = 1
DEVICE_ADDR = 0x17

TEMP_REG = 0x01
LIGHT_REG_L = 0x02
LIGHT_REG_H = 0x03
STATUS_REG = 0x04
ON_BOARD_TEMP_REG = 0x05
ON_BOARD_HUMIDITY_REG = 0x06
ON_BOARD_SENSOR_ERROR = 0x07
BMP280_TEMP_REG = 0x08
BMP280_PRESSURE_REG_L = 0x09
BMP280_PRESSURE_REG_M = 0x0A
BMP280_PRESSURE_REG_H = 0x0B
```