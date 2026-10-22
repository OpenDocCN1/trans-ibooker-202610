# Post a Picture

Create a new Python script file in the projects/photohhook directory using your preferred code editor and name it phototest.py. Add the following lines of Python code, paste your generated webhook URL in place of WEBHOOK_URL_GOES_HERE, and save the file:

```
photohook/phototest.py

import requests

WEBHOOK_URL = 'WEBHOOK_URL_GOES_HERE'
CAPTURE_FILE = r'/home/pi/projects/photohhook/capture.jpg'

photo = {'media': open(CAPTURE_FILE, 'rb')}
requests.post(WEBHOOK_URL, files=photo)
```

Open a Terminal window, change to the PhotoHook project directory, and snap a Pi Camera picture using the raspistill app:

```
$ cd ~/projects/photohhook
$ raspistill --width 640 --heigh 480 --quality 75 -o capture.jpg
```

Confirm that the new capture.jpg file exists in the directory and run the phototest.py script:

```
$ python3 phototest.py
```

You should see the capture.jpg posted to your Discord #photos channel. Run this script a few more times, capturing different photos with raspistill. Install Discord app on your Android or iOS smartphone, log in, and make sure notifications for the #photos channel are enabled (they should be by default, but check to make sure).

Run the phototest.py and verify that you see the image notification appear on