```
pinMode(LED_BUILTIN, OUTPUT);

if (!BLE.begin()) {
    Serial.println("starting BLE failed!");
    while (1);
}
```

Now we define the BLE service name and add to the advertised service. Then, add all the BLE characteristics into the BLE service.

```
BLE.setLocalName("Gyroscope");
BLE.setAdvertisedService(sensorService);

sensorService.addCharacteristic(xSensorLevel);
sensorService.addCharacteristic(ySensorLevel);
sensorService.addCharacteristic(zSensorLevel);
BLE.addService(sensorService);
```

We set initial default data on all BLE characteristics using the writeValue() function.

```
xSensorLevel.writeValue(String(0));
ySensorLevel.writeValue(String(0));
zSensorLevel.writeValue(String(0));
```

Now we can start to advertise the BLE service by calling the BLE.advertise() function. BLE readers will recognize this BLE server.

```
BLE.advertise();
Serial.println("Bluetooth device active, waiting for connections...");
}
```

On the loop() function, we wait for the incoming BLE reader. Once the BLE reader is connected, we print the MAC address of the BLE reader. Then, we turn on the LED.