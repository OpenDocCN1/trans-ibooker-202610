# Plotting Sensor Data

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