After the BLE reader is connected to our BLE radio on Arduino Nano 33 IoT, we can obtain BLEDevice object. Then, we turn on the LED by calling digitalWrite() with passing HIGH value. Then, we perform infinite looping by checking connection status.

```
if (central) {
  Serial.print("Connected to central: ");
  Serial.println(central.address());
  digitalWrite(LED_BUILTIN, HIGH);

  while (central.connected()) {
    // do nothing
  }

```

If the BLE reader is disconnected, we will obtain a false value from central.connected(). After that, we turn off the LED by calling digitalWrite() with passing LOW value.

```
digitalWrite(LED_BUILTIN, LOW);
Serial.print("Disconnected from central: ");
Serial.println(central.address());
}
}
```

Our program is done. You can save this program as HelloBLE.

# Testing Program

Now our Arduino program, HelloBLE, can be compiled and uploaded to Arduino Nano 33 IoT. To test this program, we need a mobile phone with Android or iOS platform. In this demo, I use an Android phone.

First, open Serial Monitor to see the program output from the HelloBLE program. Next, install nRF Connect for Mobile application on the Google Play store or Apple store. You can see nRF Connect for Mobile application from the Google Play store in Figure 6-2.