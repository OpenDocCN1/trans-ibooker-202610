
## Next Steps

Now that you have the foundation to control appliances like fans based on external inputs like ambient light and room temperature, expand upon that base project. Create schedules to trigger such sensor tests, or enable other Hue-enabled accessories like light strips and wall switches to trigger at certain times or environmental conditions in the home or workplace.

Before leaving the exciting world of sensor management, we’ll explore one additional sensor-related situation in our next project. Using the light sensor on the SensorHub coupled with code fragments assembled from our previously completed projects, we’ll construct the means to turn on a light when motion is detected and turn it off after a set duration.

## Footnotes

- [37] https://www.seeedstudio.com/DockerPi-Sensor-Hub-Development-Board-p-4101.html
- [38] https://www.philips-hue.com/en-us/p/hue-bridge/046677458478
- [39] https://www.philips-hue.com/en-us/p/hue-smart-plug/046677552343
- [40] https://apps.apple.com/us/app/philips-hue/id1055281310
- [41] https://github.com/studioimagin aire/phue
- [42] https://wiki.52pi.com/index.php/DockerPi_Sensor_Hub_Development_Board_SKU:_EP-0106

## Chapter 6

## Hue Auto Light

The concept of turning lights on when motion is detected has been operational for decades. Originally designed for industrial and commercial building spaces, manufactured solutions are expensive and often proprietary installations. But as we’ve seen with other projects in this book, the Pi has redefined the cost of computing and, with it, the cost to implement automation solutions.

Professional lighting systems allow operators to set a timer for how long motion is no longer detected before turning lights off. These systems also allow for lighting schedules to turn on and off lights depending on the time of day. Only the most expensive and sophisticated commercial installations also take into account if additional lighting is required based on existing light sources illuminating a room. Thanks to the Pi-SensorHub combination, our implementation will cost a tenth of that of traditional proprietary lighting systems yet perform the same functionality.

For this project, we’re going to borrow designs from our previous sensor detector projects. We’ll also utilize the SensorHub’s motion detector to trigger lighting events, with a twist; we’ll want to account for motion and time variants so we’re not turning the light on and off every second. Let’s get started.

## Setup

Here’s what you need to build this project.

### Hardware

- DockerPi SensorHub
- Philips Hue bridge
- Philips Hue smart bulb[^43]

You can use either the standard white or color light Hue smart bulbs. Although a bit more expensive than the white bulb, I prefer the color light model so I can dial in the perfect color for the environment or mood I’m in. Here’s a photo of the Hue color bulb model in my hand.

![](img/1f3315bdb28676808673aba305da7446_126_0.png)

### Software

- phue Python library
- Philips Hue smartphone app

If you’ve completed Chapter 5, *Hue Fan*, then you already have the phue Python library installed. If not, you can install the phue library via the pip3 install command.

```
$ sudo pip3 install phue
```

Assuming you successfully completed the last Hue project involving the SensorHub and that hardware add-on is still attached to your Pi, then you’re ready to proceed. If not, follow the setup in that project to attach the SensorHub and activate the I2C interface. Then register your Pi with the Hue bridge by following the instructions in the Hue Fan Bridge Registration section.

## And There Was Light

Make sure you’ve already configured and labeled your Hue smart bulb that you want to use in this project with the Hue bridge via the Hue smartphone app. Confirm that you can turn that particular smart bulb on and off from the Hue smartphone app before proceeding.

Next, identify that particular smart bulb you want to automate via the listdevices.py script we used in Chapter 5, *Hue Fan*:

```
hueautolight/listdevices.py

from phue import Bridge

huebridge = Bridge('YOUR_HUE_BRIDGE_IP_GOES_HERE')

lights = huebridge.lights

for light in lights:
    print(light.name)
```

Identify the label of the preferred smart bulb name in the generated list, and then confirm you can turn on and off that bulb programmatically with a smartbulbtest.py Python script. In my example, I’ll be turning on and off the

smart bulb I installed in my kitchen light fixture. Be sure to change the Kitchen placeholder for the name of the Hue smart bulb you are testing on your network:

```
hueautolight/smartbulbtest.py

from phue import Bridge
import time

huebridge = Bridge('YOUR_HUE_BRIDGE_IP_GOES_HERE')

huebridge.set_light('Kitchen','on', True)
time.sleep(10)
huebridge.set_light('Kitchen','on', False)
```

Just like the Hue Fan project, you should see your specified smart bulb turn on and then turn off ten seconds later. Be sure this script works correctly with your Hue setup before continuing with this project.

See how quickly that went? We’re nearly halfway done with completing the project’s objective. For the other half, we need to call upon the SensorHub’s motion detector to identify when motion takes place:

```
hueautolight/motionsensortest.py

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
```

```
BMP280_PRESSURE_REG_M = 0x0A
BMP280_PRESSURE_REG_H = 0x0B
BMP280_STATUS = 0x0C
MOTION_DETECT = 0x0D

bus = smbus.SMBus(DEVICE_BUS)

alert_trigger = False

while True:
    aReceiveBuf = []

    aReceiveBuf.append(0x00)

    for i in range(TEMP_REG,MOTION_DETECT + 1):
        aReceiveBuf.append(bus.read_byte_data(DEVICE_ADDR, i))

    if aReceiveBuf[MOTION_DETECT] == 1 :
        if alert_trigger != True:
            print("Movement was detected.")
            alert_trigger = True
    else:
        if alert_trigger != False:
            print("No movement was detected.")
            alert_trigger = False

    time.sleep(1)
```

Recall from the SensorHub overview in the Hue Fan project that when motion is detected, a blue LED along the middle-right side on the SensorHub will light up. The motion detection event will also report its status when running our motionsensortest.py script. Try running the script now:

```
$ python3 motionsensortest.py
```

Wave your hand over the SensorHub and when the blue LED motion indicator illuminates, you should see a Movement was detected. message print out in the Terminal window where the script is running. It will be followed by a No movement was detected. message if no additional movement was picked up by the sensor after a one-second delay.

## Light Bright

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
