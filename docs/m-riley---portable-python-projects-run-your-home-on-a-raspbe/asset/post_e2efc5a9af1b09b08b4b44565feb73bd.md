
![](img/1f3315bdb28676808673aba305da7446_230_0.png)

After the camera ribbon clip has been snapped back into place and the seating of the ribbon cable is confirmed stable and in full contact with the camera port connectors, seat the SensorHub back on the GPIO pins. If you haven’t used the SensorHub before, refer to [Seating the Sensor](Seating the Sensor). Be careful not to crease or pinch the Pi Camera’s ribbon cable underneath when attaching the SensorHub.

With everything properly seated and installed, power up your Pi.

## Camera Activation

Open a Terminal window on your Pi and use the raspi-config program to enable the Pi’s camera port that you just attached the camera to:

```
$ sudo raspi-config
```

From the raspi-config screen, select Interface Options, followed by Camera and enable the camera port by selecting the Camera menu option and selecting <Yes> when asked if you would like the camera interface to be enabled.

While you’re in the Interface Options screen, double-check to make sure the ARM I2C interface is enabled as well. If you already completed the previous Hue AutoFan or AutoLight projects, this interface should already be enabled. But it can’t hurt to double-check, just in case.

With the camera attached and the camera port enabled, you’re ready to take your first photo using the Pi Camera.

## Take a Picture

With the Terminal window still open, change to your PhotoHook project directory:

```
$ cd /home/pi/projects/photohook
```

Use the raspistill program already installed in the Raspberry Pi OS to take a photo, passing it the file path and name where you want to save the picture:

```
$ raspistill -o test0.jpg
```

After a few seconds, the test0.jpg file should appear in that directory. If raspistill reports an error, verify that your camera ribbon is attached correctly and that the Pi camera port has been enabled.

If you’re logged into the Raspberry Pi desktop, navigate to the project directory using the desktop File Manager, and double-click the test0.jpg file to view it in the photo viewer app.

Notice that the default image resolution for photos taken this way is at the full 8-megapixel quality level, 3280 x 2464 pixels. Photos taken at this resolution will be around 4 megabytes in file size.

While that level of image quality is great for still photos that require a high degree of detail, they’re rather large to transmit, especially as bot image attachments. Fortunately, raspistill can be configured to reduce both the resolution and quality of the capture. For example, let’s take another photo, this time reducing the resolution to 640 x 480 pixels and the image compression quality to 75%. Name this new photo test1.jpg so you can compare it with the higher quality test0.jpg taken earlier:

```
$ raspistill --width 640 --height 480 --quality 75 -o test1.jpg
```

Open and examine the test1.jpg image. If it’s not still open, reopen the original test0.jpg photo and compare the two images side by side. Unless you zoom in on the photos, you most likely will see little difference between the two in the photo viewer window. However, the difference in file size is huge, with test1.jpg likely coming in at around 203 kilobytes, a difference of over 1,800 percent!

Here’s a photo, taken to remotely monitor the results of Chapter 9, Voice IR Control, testing at 640x480 resolution and 75% quality. The image looks pretty good even in the format presented in this book.

![](img/1f3315bdb28676808673aba305da7446_233_0.png)

A 200-kilobyte file will transfer significantly faster than a 4,000-kilobyte file and will also consume far less resources and overhead than the higher-

resolution image. As such, that is the file quality I recommend using for this project. Of course, if you have a fast Internet connection and an unlimited data plan on your smartphone, you’re welcome to use higher resolution for your still-photo captures. Feel free to play with the raspistill values until you attain just the right balance of image quality and file size that best suits your needs.

## Motion Detector

For this project, we’re going to once again employ the infrared heat sensor on the SensorHub to detect motion, just like we did in Chapter 6, *Hue Auto Light*. Since we already wrote the code for that motion detection routine, we can copy it for this project. The only new routines we need to add are to capture and transmit an image via a Discord webhook.

Joe asks:

## Why not use the Motion program?

Motion[93] is a popular, free software utility that works the way most security cameras detect movement. It constantly monitors an incoming image stream for significant variations in pixel values, indicating a state change. When this change is detected, Motion can execute additional programs or scripts in response to the event. It can also be configured to identify motion in specifically defined regions of an image, just like commercial motion-detecting security camera packages.

However, to perform this analysis, Motion requires that the cameras be operational and streaming data at all times, whether there’s any movement or not. This means another daemon must be left running to capture and analyze any changes and act on them accordingly. Oh, and the version of Motion that can be installed on the Raspberry Pi via apt install is incompatible with the native Pi camera configuration. Additional configuration overhead is required to make it work, or better yet, you could use an inexpensive webcam and connect it to one of the Pi’s USB ports instead.

Since we already have the SensorHub working for our other projects, it will be faster and less computationally expensive to use it in lieu of Motion. However, keep in mind that because the infrared sensor on the Sensorhub requires a heat signature to detect motion, it won’t work when operating behind a window or other obstruction blocking the measurement of infrared radiation.

If you decide to give Motion a try via sudo apt install motion, be sure to review the project documentation, adding whatever Python script you want to execute whenever movement is detected to the motion.conf. For example, if you wanted a test.py script to run whenever Motion detected movement, you would add the following line to the motion.conf file:

```
On_motion_detected python3 test.py
```

Now, back to our regularly scheduled project.

## Hook a Photo

If you haven’t already done so, create a Discord account and log in to your Discord server. Create a new text channel called #photos using the + icon to the right of the Text Channels listing. Edit the #photos channel settings by selecting the sprocket icon to the right of the #photos channel name. From there, select Integrations and click the Create Webhook button.

Give the new webhook a name like PhotoBot and save the changes.

Select the Copy Webhook URL button and store the generated URL to a safe file. Webhooks are fast and lightweight because they don’t require authentication to post messages to a designated channel. That also means anyone with this sensitive webhook URL can do the same thing and spam your channel with unwanted texts.

Now that the basic Discord configuration is done and the webhook URL is activated, we can post a raspistill captured image (or any other file, for that matter) to the #photos text channel.

## Post a Picture

Create a new Python script file in the projects/photohook directory using your preferred code editor and name it phototest.py. Add the following lines of Python code, paste your generated webhook URL in place of WEBHOOK_URL_GOES_HERE, and save the file:

```
photohook/phototest.py

import requests

WEBHOOK_URL = 'WEBHOOK_URL_GOES_HERE'
CAPTURE_FILE = r'/home/pi/projects/photohook/capture.jpg'

photo = {'media': open(CAPTURE_FILE, 'rb')}
requests.post(WEBHOOK_URL, files=photo)
```

Open a Terminal window, change to the PhotoHook project directory, and snap a Pi Camera picture using the raspistill app:

```
$ cd ~/projects/photohook
$ raspistill --width 640 --height 480 --quality 75 -o capture.jpg
```

Confirm that the new capture.jpg file exists in the directory and run the phototest.py script:

```
$ python3 phototest.py
```

You should see the capture.jpg posted to your Discord #photos channel. Run this script a few more times, capturing different photos with raspistill. Install Discord app on your Android or iOS smartphone, log in, and make sure notifications for the #photos channel are enabled (they should be by default, but check to make sure).

Run the phototest.py and verify that you see the image notification appear on
