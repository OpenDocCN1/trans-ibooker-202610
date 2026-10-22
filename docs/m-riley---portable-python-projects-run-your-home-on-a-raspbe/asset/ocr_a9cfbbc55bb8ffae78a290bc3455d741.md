# Add It

Cool, we can now programmatically control legacy infrared-receiving consumer electronics. It’s time to incorporate that single line of irsend code into the voicewol.py Python script we created and configured in the Voice Wake on LAN project. You can either associate it with the existing WAKE_PC body tag we associated with waking the PC or create a new IFTTT/Pushbullet sequence to control a device separate from the Wake PC event trigger. For example, I created a separate body message called PWR_TV to simulate pressing the power button on my Philips TV. Then I created a new IFTTT applet, following the same steps described in the Voice Wake on LAN project, calling this new applet “Turn on/off TV”, and assigned a new phrase. I couldn’t use the phrase “Turn on the TV” because it was too generic for Google to parse. Instead, I more specifically used the phrase “Turn on the Philips,” which was more specific for Google to translate and properly route through IFTTT. For the Pushbullet message, I added the aforementioned PWR_TV label. Here’s the final modified voicewol-ir.py script incorporating these additions:

```
voiceir/voicewol-ir.py
import os
from pushbullet import Listener
from pushbullet import Pushbullet

ACCESS_CODE = 'PUSHBULLET_ACCESS_CODE_GOES_HERE'
HTTP_PROXY_HOST = None
HTTP_PROXY_PORT = None
MAC = 'YOUR_COMPUTER_MAC_ADDRESS_GOES_HERE'

def on_push(data):
    pbullet = Pushbullet(API_KEY)
    pushes = pbullet.get_pushes()
    try:
        latest = pushes[0]
        if latest['body'] == "WAKE_PC":
```