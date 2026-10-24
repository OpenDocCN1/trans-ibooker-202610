Next, we can build the Arduino program for the OLED I2C display.

# Checking the I2C Address of the OLED I2C Display

After we make wiring between the Arduino Nano 33 IoT and the OLED I2C display, we can use the i2c_scanner program from Chapter 2 to check the I2C address from devices. We want to know the I2C address from the OLED I2C display.

Load the i2c_scanner program into Arduino software. Then, compile and upload this program into Arduino Nano 33 IoT. After that, open the Serial Monitor tool. You should see three I2C addresses. Two of them are I2C built-in sensors on Arduino Nano 33 IoT. The rest is our OLED I2C display.

You can see my program output in Figure 3-10. You can see my OLED I2C display running on the 0x3C I2C address. Two I2C addresses, 0x60 and x6A, are I2C built-in sensors on Arduino Nano 33 IoT.

![](bbox=[0.09, 0.488, 0.865, 0.77])

Figure 3-10. Detecting I2C addresses for the OLED I2C display

Next, we set up libraries in order to build programs for OLED I2C display on Arduino Nano 33 IoT.