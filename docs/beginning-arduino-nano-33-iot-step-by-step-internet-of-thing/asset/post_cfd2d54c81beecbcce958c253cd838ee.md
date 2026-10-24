
## Figure 5-8. Adding a new Thing

## Figure 5-9. A dashboard of an Arduino Thing

## Adding a Property

We can expose I/O from Arduino Nano 33 IoT through the Thing property. For instance, we want to expose sensor data to the Arduino IoT Cloud. In this scenario, we want to expose our built-in LED in Arduino Nano 33 IoT. We can set I/O on LED so we can turn on/off the LED over the web.

You can start by opening a Thing on the Arduino IoT Cloud. Figure 5-9 shows our Thing that we already created. Click the Add Property button to add a new property. After clicking, you will obtain a form, as shown in Figure 5-10.

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_146_0.png)

Figure 5-10. Adding a new property

In this scenario, you can set the following options:

- Name: LED1
- VARIABLE NAME: LED1
- TYPE: ON/OFF (Boolean)
- Permission: Read & Write
- Update: when the value changes
- History: checked

My entry is shown in Figure 5-10. After filled all fields, you can click the ADD PROPERTY button. You will come back to the Things form. You should see LED1 property on the Thing form, as shown in Figure 5-11.

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_147_0.png)

Figure 5-11. A Thing with one property

You have created a Thing. Next, we can modify the Sketch program in order to connect to this LED1 property.

## Editing the Sketch Program

Now we can edit our Sketch program. On the property form from the Thing dashboard (see Figure 5-11), you can click the EDIT SKETCH button. After clicking, you will obtain the Arduino web editor, as shown in Figure 5-12.

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_148_0.png)

*Figure 5-12.* Editing the Sketch program

First, we modify the Secret program to configure the WiFi network on Arduino Nano 33 IoT. Fill in SSID ID and SSID key on the Secret tab. Now we modify codes on our main Sketch.

You can see our property variable, LED1, is declared on the main Sketch program.

```
#include "arduino_secrets.h"
bool LED1;
#include "thingProperties.h"
```

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_150_0.png)

## Figure 5-13. A dashboard of Arduino IoT Cloud

We can add a new switch on our dashboard editor. Click the ADD button and select Switch widget. You can see the Switch widget option in Figure 5-14.

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_150_1.png)

## Figure 5-14. Adding a widget on the dashboard

Click Switch widget and then drag it to the dashboard editor. After dragging the Switch widget, you should see Switch widget, as shown in Figure 5-15.

Click Example Data to link the Switch widget to the Thing property. After clicking, you will obtain a form as shown in Figure 5-16.

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_151_0.png)

Figure 5-15. Adding a switch

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_152_0.png)

## Figure 5-16. Setting a Switch widget

Click the Linked Property button to link with the Thing property. We will obtain a form, as shown in Figure 5-17. Select our Thing name, and select Property (LED1). If done, click the LINK PROPERTY button. Then, we will back to our dashboard editor.

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_153_0.png)

*Figure 5-17.* Linking a switch to thing properties

You also can rename our dashboard program—for instance, LED Demo. Now we can test our dashboard program.

## Testing

To test our program, we need to activate our dashboard program on running mode. You can click the USE DASHBOARD button to activate the dashboard program.

You can attempt to toggle the Switch to ON mode. Then, you should see the built-in LED on Arduino Nano 33 IoT lights up. You also can turn off the LED by toggling the Switch to OFF mode.

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_154_0.png)

*Figure 5-18. LED demo program on the dashboard editor*

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_154_1.png)

*Figure 5-19. Toggling the Switch to turn on the LED*

You can customize this program by adding some LEDs. Then, you can add some properties on the Things program.

## Develop Sensor Monitoring

We have created Arduino IoT Cloud to turn on/off LED on Arduino Nano 33 IoT. Now we can continue to build the Arduino IoT Cloud program for monitoring sensors. In this demo, we use built-in sensors on Arduino Nano 33 IoT. For testing, we use the Gyroscope sensor.

To implement this demo, we can perform some tasks with the following steps:

- adding a new thing
- adding properties
- editing the Sketch program
- building a dashboard
- testing

We implement these steps in the next section.

## Add a New Thing

You can create a new Thing on Arduino IoT Cloud. If you have a free membership, you should delete the existing Thing on Arduino IoT Cloud because you can only create one Thing.

Now you can create a new Thing. For instance, we set the Thing name as GyroscopeThing. Then, we can add some properties to GyroscopeThing.

## Add Property

After creating a Thing, we can add properties. For this demo, we create three properties to monitor the Gyroscope sensor from Arduino Nano 33 IoT. These properties will be linked to X, Y, and Z degrees from the Gyroscope sensor. When we can add a new property, we have a form, as shown in Figure 5-20. We add three properties with property parameters as shown in Table 5-1.

After we create three Thing properties, we will return to our Things dashboard. You can see three properties in Figure 5-21.

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_157_0.png)

Figure 5-21. Three properties on GyroscopeThing

## Editing the Sketch Program

After we add three properties on GyroscopeThing, we can edit the Sketch program. We will read the gyroscope sensor and then update sensor data to property variables.

Click the EDIT SKETCH button to edit our program on Arduino IoT Cloud. Then, we have the Sketch web editor. We modify this Sketch program to enable us to read the Gyroscope sensor and update three Thing properties.

First, we set the SSID ID and SSID key on Arduino Secret. Then, we open the main program. We add the LSM6DS3 library on the Sketch program.

```
#include "arduino_secrets.h"
#include <Arduino_LSM6DS3.h>
```

We will see our property variables, such as xDegree, yDegree, and zDegree.

```
float xDegree;
float yDegree;
float zDegree;
```

```
#include "thingProperties.h"
```

On the setup() function, we initialize the LSM6DS3 library by calling IMU.begin() API. Then, we can access the Gyroscope sensor on Arduino Nano 33 IoT.

```
void setup() {
...
  if (!IMU.begin()) {
    Serial.println("Failed to initialize IMU!");
    while (1);
  }
...
}
```

We can read the Gyroscope sensor and then update to xDegree, yDegree, and zDegree variables. We can call IMU.gyroscopeAvailable() to check whether sensor data is available or not. To read sensor data, we can use the IMU.readGyroscope() function.

```
void loop() {
    ArduinoCloud.update();
    // Your code here
    if (IMU.gyroscopeAvailable()) {
        IMU.readGyroscope(xDegree, yDegree, zDegree);
    }
    delay(1000);
}
```

Save this program. Now you can compile this Sketch program and upload to Arduino Nano 33 IoT device.

## Build a Dashboard

We build a dashboard to create interaction between Arduino Nano 33 IoT and Arduino IoT Cloud. We perform a new dashboard program with the following steps:

- Create a new dashboard.
- Add three value widgets into the dashboard editor.
- Each value widget is to be linked to each Thing Property, as shown in Figure 5-22.
- Do the same action for XDegree, YDegree, and ZDegree value widgets.

Last, we can set dashboard name. Now we can test our widgets on the dashboard.

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_160_0.png)

Figure 5-22. Linking a property to a widget

## Testing

We can test our Arduino IoT Cloud program. You can navigate to the Arduino IoT Cloud dashboard. Click the dashboard form that we already created. Click the USE DASHBOARD button to be in RUN mode.

You can see the widget output in Figure 5-23. Shake your Arduino Nano 33 IoT board to see sensor data changes on dashboard widgets.
