
## Wiring for Arduino Nano 33 IoT and the OLED I2C Display

We use the OLED display with an I2C interface so we can connect this OLED display to Arduino Nano 33 IoT over I2C pins. You can see my wiring in Figure 3-9. You can perform this wiring as follows:

- The OLED I2C display module serial data is connected to the Arduino A4 pin.
- The OLED I2C display module serial clock is connected to the Arduino A5 pin.
- The OLED I2C display module voltage common collector (VCC) is connected to Arduino 3.3V.
- The OLED I2C display module ground (GND) is connected to the Arduino GND pin.

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_100_0.png)

Figure 3-9. Wiring the OLED I2C display on Arduino Nano 33 IoT

Next, we can build the Arduino program for the OLED I2C display.

## Checking the I2C Address of the OLED I2C Display

After we make wiring between the Arduino Nano 33 IoT and the OLED I2C display, we can use the i2c_scanner program from Chapter 2 to check the I2C address from devices. We want to know the I2C address from the OLED I2C display.

Load the i2c_scanner program into Arduino software. Then, compile and upload this program into Arduino Nano 33 IoT. After that, open the Serial Monitor tool. You should see three I2C addresses. Two of them are I2C built-in sensors on Arduino Nano 33 IoT. The rest is our OLED I2C display.

You can see my program output in Figure 3-10. You can see my OLED I2C display running on the 0x3C I2C address. Two I2C addresses, 0x60 and 0x6A, are I2C built-in sensors on Arduino Nano 33 IoT.

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_101_0.png)

Figure 3-10. Detecting I2C addresses for the OLED I2C display

Next, we set up libraries in order to build programs for OLED I2C display on Arduino Nano 33 IoT.

## Setting up the OLED I2C Display Library

To work with the OLED I2C display on Arduino, we need to install two of the following libraries from Adafruit:

- Adafruit_SSD1306, https://github.com/adafruit/Adafruit_SSD1306
- Adafruit GFX Library, https://github.com/adafruit/Adafruit-GFX-Library

We can install these libraries via Library Manager on Arduino software.
Type Adafruit_SSD1306 and Adafruit GFX Library to install these libraries.
Figure 3-11 shows Adafruit_SSD1306 library.

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_102_0.png)

*Figure 3-11.* Adding libraries for the OLED I2C display

Install these libraries. You will probably be asked to install additional libraries to enable work with Adafruit_SSD1306 and Adafruit GFX library—for instance, Adafruit BusIO.

Next, we modify this program with the I2C address from our OLED I2C display. In the previous section, we had the 0x3C address for the OLED I2C display. Replace the I2C address display.begin() with 0x3C, as shown in Figure 3-12.

Now you can compile and upload this program to Arduino Nano 33 IoT. You should see some forms on the OLED I2C display. Figure 3-13 shows program output from ssd1306_128x64_i2c on the OLED I2C display with 128x64 pixels.

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_104_0.png)

Figure 3-13. Running the ssd1306_128x64_i2c program on the OLED I2C display

If you can see display output with the ssd1306_128x64_i2c program, it means your OLED I2C display works. We will use this OLED to display sensor data.

## Displaying the Gyroscope Sensor

If you don’t see display output, first, check the I2C address of your OLED I2C display. Then, make sure your OLED I2C displays with display size 128x64 pixels.

In this section, we will build the Arduino program to display the Gyroscope sensor for the OLED I2C display. We will use a program from the previous section to read the Gyroscope sensor.

Now we can open Arduino software and create a new program. We start by importing all required libraries for the OLED I2C display and the Gyroscope sensor.

```
#include <SPI.h>
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>
#include <Arduino_LSM6DS3.h>
```

We define the OLED I2C display size. In this demo, I use 128x64 pixels. You can change its size based on your OLED module.

```
#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64
```

Next, we configure Adafruit_SSD1306 with passing the I2C address of the OLED module and display size.

```
#define OLED_RESET  4 // Reset pin
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);
```

On the setup() function, we initialize serial communication, the Gyroscope sensor, and Adafruit_SSD1306.

```
void setup() {
    Serial.begin(115200);

    if (!IMU.begin()) {
        Serial.println("Failed to initialize IMU!");
        while (1);
    }
    if(!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) { // Address
    // 0x3D for 128x64
        Serial.println(F("SSD1306 allocation failed"));
        for(;;); // Don't proceed, loop forever
    }
}
```

After that, we test the OLED I2C display by calling display() for 2 seconds. Then, we clear the screen of the OLED display.

```
display.display();
delay(2000); // Pause for 2 seconds

// Clear the buffer
display.clearDisplay();
}
```

On the loop() function, we read the Gyroscope sensor. First, we check the sensor data with IMU.gyroscopeAvailable(). If it's available, we can read the sensor data using the IMU.readGyroscope() function. Store all the sensor data on the x, y, and z variables.

```
void loop() {
    float x, y, z;

    if (IMU.gyroscopeAvailable()) {
        IMU.readGyroscope(x, y, z);
```

Next, we display sensor data on the OLED I2C display using the print() function. We also use setTextSize() to set font size.

```
display.clearDisplay();
display.setTextSize(1);
display.setTextColor(SSD1306_WHITE);
display.setCursor(0,0);
display.print("Gyroscope: X, Y, Z");
display.setTextSize(2);
display.setCursor(0,12);
display.print(String(x));
display.setCursor(0,30);
display.print(String(y));
display.setCursor(0,48);
display.print(String(z));
display.display();
```

Finally, we display the sensor data into serial terminal using the Serial.print() and Serial.println() functions.

```
Serial.print(x);
Serial.print('\t');
Serial.print(y);
Serial.print('\t');
Serial.println(z);

delay(300);
}
}
```

Save this program as OledSensor. Now you can compile and upload this program in Arduino Nano 33 IoT. You should see sensor data on the OLED I2C display, as shown in Figure 3-14.

## CHAPTER 3    IMU SENSOR: ACCELERATOR AND GYROSCOPE

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_108_0.png)

**Figure 3-14.** *Displaying the Gyroscope sensor on the OLED I2C display*

You also can see program output on the Serial Monitor tool. You can see my program output in Figure 3-15.

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_109_0.png)

# Figure 3-15. Program output from OledSensor

This is the end of the chapter. You can practice by applying the IMU sensor in your projects.

## Summary

You have learned how to access internal IMU sensors in Arduino Nano 33 IoT. We began by setting up the LSM6DS3 library. Then, we created Arduino programs to access accelerator and gyroscope sensors on Arduino Nano 33 IoT. Finally, we displayed sensor data on the Serial Plotter tool and OLED I2C display.

Next, we will learn how to access networks on Arduino Nano 33 IoT and make IoT programs.

## CHAPTER 4

## Arduino Nano 33 IoT Networking

Arduino Nano 33 IoT is designed for IoT implementation. This board has a network module, WiFi, which enables communication with other systems. In this chapter, we focus on how Arduino Nano 33 IoT accesses and collaborates with external systems, such as servers.

You will learn the following topics in this chapter:

- Setting up WiFi with WiFiNINA library
- Scanning WiFi hotspots
- Connecting to a network
- Accessing network time protocol (NTP) servers
- Building a simple IoT application

This chapter requires an environment such as a WiFi network. You should provide a WiFi hotspot with enabled Internet so Arduino Nano 33 IoT can communicate with other systems.
