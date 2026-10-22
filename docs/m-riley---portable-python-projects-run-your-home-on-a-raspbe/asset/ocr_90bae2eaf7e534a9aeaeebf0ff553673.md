# Light Bright

We don’t need to turn the light on and unnecessarily waste electricity if there’s enough natural daylight in the room already. To test for that condition, we’ll poll the SensorHub’s onboard light sensor. The light sensor will help determine if additional room lighting is necessary. The onboard SensorHub light sensor detects the amount of light energy present in units of lux.[44]

For this project, we’ll turn on the Hue smart bulb when motion is detected as long as the amount of available light detected in the room is less than 25 lux, which is roughly equivalent to the ambient light diffused in a slightly darkened room. Let’s write a quick light-sensor test script to detect the light illumination value and react accordingly:

```
hueautolight/lightsensortest.py

import smbus
import time

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
BMP280_STATUS = 0x0C
MOTION_DETECT = 0x0D

bus = smbus.SMBus(DEVICE_BUS)
```