# Turn It On Again

Create a smartplugtest.py file, and replace the YOUR_HUE_BRIDGE_IP_GOES_HERE and the Fan identifiers with the IP address and Hue name of the device in your environment:

```
huefan/smartplugtest.py

from phue import Bridge
import time

huebridge = Bridge('YOUR_HUE_BRIDGE_IP_GOES_HERE')

① huebridge.set_light('Fan','on', True)
time.sleep(10)
② huebridge.set_light('Fan','on', False)
```

① Power on the device by calling the set_light() function with the name of the device you want to control (in this case, Fan), and setting its value to True.

② After waiting ten seconds, the set_light() function is called again, this time setting the Fan device to False to turn it off.

Be near the fan or whatever other appliance you connected your smart plug to and verify that your smart plug is working before running the script. Do this using the Philips Hue app on your smartphone by toggling on and off the smart plug. Once confirmed that everything works as expected, set the smart plug to off via the Hue smartphone app and execute the smartplugtest.py script:

```
$ python3 smartplugtest.py
```

Your smart plug should power on. After ten seconds, it should power off. If you had a fan or other power toggle appliance connected to the smart plug, it should do the same.