
information about technical documentation of LSM6DS3, we can read the detailed datasheet document at this link, https://content.arduino.cc/assets/st_imu_lsm6ds3_datasheet.pdf. You can see the LSM6DS3 chip on Arduino Nano 33 IoT board in Figure 3-1.

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_87_0.png)

Figure 3-1. LSM6DS3 chip on Arduino Nano 33 IoT

In Chapter 2, we learned about the I2C interface. We also perform a scan of the I2C address. You can run the i2c_scanner program from Chapter 2. Figure 3-2 shows a list of I2C addresses of I2C devices. The IMU sensor runs on 0x6A and 0x6B I2C addresses.

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_88_0.png)

Figure 3-2. A list I2C addresses from sensor device-based I2C interface

In this chapter, we will explore the IMU sensor, LSM6DS3, on Arduino Nano 33 IoT. We access accelerator and gyroscope sensors from the Arduino program.

## Set Up LSM6DS3 Library

To access the IMU sensor-based LSM6DS3 chip on Arduino Nano 33 IoT, we need to install the Arduino LSM6DS3 library. This library can be used to access the IMU sensor for accelerator and gyroscope sensors. We will use this library in this chapter. Details about the LSM6DS3 library can be read at this link, https://www.arduino.cc/en/Reference/ArduinoLSM6DS3.

To install the Arduino LSM6DS3 library, you can open Arduino software. Then, click the menu Sketch ▶ Include Library ▶ Manage Libraries, as shown in Figure 3-3.

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_89_0.png)

Figure 3-3. Opening the Manage Libraries menu in Arduino software

After we click the Manage Libraries menu, we will obtain a Library Manager dialog, as shown in Figure 3-4. You can type arduino_lsm6ds3 in the search textbox. Then, you should see a list of libraries. You should also see the Arduino_LSM6DS3 library. In Figure 3-4 you can see the Arduino_LSM6DS3 library that is noted by a red arrow.

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_89_1.png)

Figure 3-4. Installing the Arduino LSM6DS3 library

Click the Install button after you click the Arduino_LSM6DS3 library. Make sure your computer is connected to the Internet. After completing installation, we can access the IMU sensor on Arduino Nano 33 IoT.

## Working with an Accelerometer

The IMU sensor in Arduino Nano 33 IoT has an accelerometer sensor. This sensor measures acceleration on x, y, and z coordinates. The sensor value ranges from -4 to 4. We will use the Arduino LSM6DS3 library to access the IMU accelerometer sensor.

In general, we can start to use the LSM6DS3 library by calling `IMU.begin()`. Then, we can read the sensor value by calling the `IMU.readAcceleration()` function.

For the demo, we read the IMU accelerometer on the Arduino Nano 33 IoT board. Then, we print the measurement result on the serial terminal. You can open Arduino software and write this complete program.

```
#include <Arduino_LSM6DS3.h>

void setup() {
  Serial.begin(115200);
  while (!Serial);

  if (!IMU.begin()) {
    Serial.println("Failed to initialize IMU!");

    while (1);
  }

  Serial.print("Accelerometer sample rate = ");
  Serial.print(IMU.accelerationSampleRate());
  Serial.println(" Hz");
  Serial.println();
}
```

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_92_0.png)

Figure 3-5. Program output from reading the IMU accelerometer

How does it work?

First, we include the Arduino LSM6DS3 library in the Arduino program.

```
#include <Arduino_LSM6DS3.h>
```

We initialize the IMU sensor and the Serial object on the setup() function.

```
void setup() {
  Serial.begin(115200);
  while (!Serial);

  if (!IMU.begin()) {
    Serial.println("Failed to initialize IMU!");

    while (1);
  }
}
```

We also can print the current accelerometer sample rate by calling the IMU.accelerationSampleRate() function on the serial terminal.

```
Serial.print("Accelerometer sample rate = ");
Serial.print(IMU.accelerationSampleRate());
Serial.println(" Hz");
Serial.println();
Serial.println("Acceleration in G's");
Serial.println("X\tY\tZ");
```

On the loop() function, we read the accelerometer sensor. We should check whether there is available accelerometer sensor data by calling the IMU.accelerationAvailable() function. If it’s available for sensor data, we can read the sensor data using the IMU.readAcceleration() function.

```
void loop() {
    float x, y, z;

    if (IMU.accelerationAvailable()) {
        IMU.readAcceleration(x, y, z);
```

Then, we print the sensor data on the serial terminal.

```
Serial.print(x);
Serial.print('\t');
Serial.print(y);
Serial.print('\t');
Serial.println(z);
```

## Working with Gyroscope

Gyroscope is a sensor to measure orientation and angular velocity. Arduino Nano 33 IoT has a built-in gyroscope sensor over the IMU LSM6DS3 sensor chip. We can access this sensor using the Arduino LSM6DS3 library.

Save this program as SimpleGyroscope. Now you can compile and upload this program into Arduino Nano 33 IoT. We can see the program output using the Serial Monitor. Change your board orientation to see the change values on the Serial Monitor tool.

Figure 3-6 shows my program output for the SimpleGyroscope program. You can see gyroscope values for x, y, and z.

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_95_0.png)

Figure 3-6. Program output from reading the gyroscope sensor

How does it work?

First, we include the Arduino LSM6DS3 library in the Arduino program.

```
#include <Arduino_LSM6DS3.h>
```

We initialize the IMU sensor to enable work with the gyroscope sensor and the Serial object on the setup() function.

```
void setup() {
  Serial.begin(115200);
  while (!Serial);
}
```

```
if (!IMU.begin()) {
    Serial.println("Failed to initialize IMU!");
    while (1);
}
```

We also can print the current gyroscope sample rate by calling the IMU.gyroscopeSampleRate() function on the serial terminal.

```
Serial.print("Gyroscope sample rate = ");
Serial.print(IMU.gyroscopeSampleRate());
Serial.println(" Hz");
Serial.println();
Serial.println("Gyroscope in degrees/second");
Serial.println("X\tY\tZ");
```

On the loop() function, we read the gyroscope sensor. We should check whether there is available gyroscope sensor data by calling the IMU.gyroscopeAvailable() function. If it’s available for the Gyroscope sensor data, we can read sensor data using the IMU.readGyroscope() function.

```
void loop() {
    float x, y, z;
    if (IMU.gyroscopeAvailable()) {
    IMU.readGyroscope(x, y, z);
```

Then, we print the sensor data on the serial terminal.

```
Serial.print(x);
Serial.print('\t');
Serial.print(y);
Serial.print('\t');
Serial.println(z);
```

This is the end of the project. You can practice by applying the IMU sensor in your projects.

## Plotting Sensor Data

We can read sensor data from built-in sensor devices on Arduino Nano 33 IoT. In this section, we will plot our sensor using the Serial Plotter tool from Arduino. For testing, we will use previous projects that read the Gyroscope sensor.

You can open Arduino software. We initialize our Gyroscope sensor and serial communication on the setup() function. We set serial baudrate 115200 and initialize Gyroscope by calling the IMU.begin() function.

```
#include <Arduino_LSM6DS3.h>

void setup() {
  Serial.begin(115200);
  while (!Serial);

  if (!IMU.begin()) {
    Serial.println("Failed to initialize IMU!");

    while (1);
  }
}
```

On the loop() function, we read the Gyroscope sensor using IMU.readGyroscope(). First, we should check availability of sensor data by calling the IMU.gyroscopeAvailable() function. We store the Gyroscope sensor to x, y, and z variables.

```
void loop() {
  float x, y, z;

  if (IMU.gyroscopeAvailable()) {
    IMU.readGyroscope(x, y, z);
```

## Displaying Sensor Data with Organic Light-Emitting Diode I2C Display

In this section, we want to display sensor data on an organic light-emitting diode (OLED) display. There are two interface models on OLED display: serial peripheral interface (SPI) and I2C. In this demo, we will use the OLED I2C display. You can buy any OLED I2C display module in a local electronic store. You can probably find it on Aliexpress or Alibaba.

For this demo, I use the OLED I2C display with 0.96 inch or 128 x 64 pixels. You can see my OLED I2C display in Figure 3-8.

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_99_0.png)

Figure 3-8. OLED 0.96-inch I2C display

You can use any display size for the OLED I2C display. Next, we will wire the OLED I2C display for Arduino Nano 33 IoT.

## CHAPTER 3 | IMU SENSOR: ACCELERATOR AND GYROSCOPE
