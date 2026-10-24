```
Serial.println("");
Serial.println("");
}
```

We will use the scanWiFi() function in the loop() function. We can also turn on LED while the program is scanning WiFi SSIDs. After completely scanning WiFi SSIDs, we turn off the LED and set the delay for 15 seconds.

```
void loop() {
  digitalWrite(led, HIGH);
  scanWiFi();
  digitalWrite(led, LOW);
  delay(15000);
}
```

# Connecting to a WiFi Network

We already learned how to obtain a list of WiFi SSIDs. Now we can connect and join with a certain WiFi SSID. We need information about the WiFi SSID name and its key if that WiFi SSID applies an access security.

For our demo, we use the Arduino program, called WiFiWebClient. This program will connect to the WiFi SSID and then access the Google website. Figure 4-4 is a program sample: WiFiWebClient.