
# Introduction

Arduino Nano 33 IoT is one of IoT platforms from Arduino. This board uses WiFi and Bluetooth modules to connect to a network. WiFi is a common network that people use to access the Internet. Bluetooth is a part of the wireless personal network (WPAN) that enables communication with other devices within a short distance.

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_111_0.png)

Figure 4-1. WiFi module on Arduino Nano 33 IoT

Arduino Nano 33 IoT is designed for low-cost IoT devices to leverage IoT solutions. Arduino Nano 33 IoT has a WiFi module, as shown in Figure 4-1. In this chapter, we apply a WiFi module in Arduino Nano 33 IoT to communicate with others.

## Set up the WiFiNINA Library

To access the WiFi module on the Arduino Nano 33 IoT board, we should install the WiFiNINA library. We can install it via Library Manager. You can click the menu Sketch ▶ Include Library ▶ Manage Libraries. After we click the menu for Manage Libraries, we will obtain a dialog as shown in Figure 4-2. You can type WiFiNINA in the search textbox. After that, you should see WiFiNINA on the list (see Figure 4-2).

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_112_0.png)

Figure 4-2. Installing the WiFiNINA library

Make sure your computer is connected to the Internet. After you install the WiFiNINA library, we can create Arduino programs to communicate with other systems over the WiFi network.

## Scanning WiFi Hotspot

We can access a WiFi hotspot if we know the WiFi SSID name. In this section, we build an Arduino program to scan existing WiFi SSIDs and then print the list on the serial terminal.

Now you can open Arduino software. You can write the following completed program.

```
#include <SPI.h>
#include <WiFiNINA.h>

int led = 13;
```

```
void setup() {
    Serial.begin(115200);
    pinMode(led, OUTPUT);

    // check for the WiFi module:
    if (WiFi.status() == WL_NO_MODULE) {
        Serial.println("Communication with WiFi module failed!");
        // don't continue
        while (true);
    }
}
void loop() {
    digitalWrite(led, HIGH);
    scanWiFi();
    digitalWrite(led, LOW);
    delay(15000);
}

void scanWiFi() {
    Serial.print("Scanning...");
    byte ssid = WiFi.scanNetworks();

    Serial.print("found ");
    Serial.println(ssid);

    for (int i = 0; i<ssid; i++) {
        Serial.print(">> ");
        Serial.print(WiFi.SSID(i));
        Serial.print("\tRSSI: ");
        Serial.print(WiFi.RSSI(i));
        Serial.print(" dBm");
        Serial.print("\tEncryption: ");
        Serial.println(WiFi.encryptionType(i));
    }
}
```

```
Serial.println("");
Serial.println("");
}
```

Save this program as WifiScan. Now you can compile and upload this program into Arduino Nano 33 IoT. We can see program output using the Serial Monitor.

Figure 4-3 shows my program output for the WifiScan program. You can see a list of WiFi SSIDs. If you don’t see a list of WiFi SSIDs, you should move to a place where you are certain they can be found.

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_114_0.png)

Figure 4-3. Scanning WiFi hotspots

How does it work?

First, we define the WiFiNINA library and digital pin for built-in LED on Arduino Nano 33 IoT.

```
#include <SPI.h>
#include <WiFiNINA.h>

int led = 13;
```

On setup() function, we initialize Serial and WiFi objects.

```
void setup() {
    Serial.begin(115200);
    pinMode(led, OUTPUT);

    // check for the WiFi module:
    if (WiFi.status() == WL_NO_MODULE) {
        Serial.println("Communication with WiFi module failed!");
        // don't continue
        while (true);
    }
}
```

To scan existing WiFi SSIDs, we create the scanWiFi() function. We can call WiFi.scanNetworks() to retrieve all existing WiFi SSIDs. Once we have the list of WiFi SSIDs, we print WiFi information such as SSID name, RSSI, and encryption model.

```
void scanWiFi() {
    Serial.print("Scanning...");
    byte ssid = WiFi.scanNetworks();

    Serial.print("found ");
    Serial.println(ssid);

    for (int i = 0; i<ssid; i++) {
        Serial.print(">> ");
        Serial.print(WiFi.SSID(i));
        Serial.print("\tRSSI: ");
        Serial.print(WiFi.RSSI(i));
        Serial.print(" dBm");
        Serial.print("\tEncryption: ");
        Serial.println(WiFi.encryptionType(i));
    }
}
```

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

## Connecting to a WiFi Network

We already learned how to obtain a list of WiFi SSIDs. Now we can connect and join with a certain WiFi SSID. We need information about the WiFi SSID name and its key if that WiFi SSID applies an access security.

For our demo, we use the Arduino program, called WiFiWebClient. This program will connect to the WiFi SSID and then access the Google website. Figure 4-4 is a program sample: WiFiWebClient.

```
#include <SPI.h>
#include <WiFiNINA.h>

#include "arduino_secrets.h"
char ssid[] = SECRET_SSID;
char pass[] = SECRET_PASS;
int keyIndex = 0;

int status = WL_IDLE_STATUS;
char server[] = "www.google.com";

WiFiClient client;
```

We initialize Serial and WiFi objects. We set baudrate 115200 for serial communication. We also verify the WiFi module by calling the WiFi.status() function.

```
void setup() {
  Serial.begin(115200);
  while (!Serial) {
    ;
  }
  // check for the WiFi module:
  if (WiFi.status() == WL_NO_MODULE) {
    Serial.println("Communication with WiFi module failed!");
    // don't continue
    while (true);
  }
}
```

After the WiFi object is initialized, Arduino connects to the WiFi network. Once our Arduino is connected, we print the WiFi connection status on the serial terminal using the printWiFiStatus() function.

// attempt to connect to Wifi network:
while (status != WL_CONNECTED) {
    Serial.print("Attempting to connect to SSID: ");
    Serial.println(ssid);
    status = WiFi.begin(ssid, pass);

    // wait 10 seconds for connection:
    delay(10000);
}
Serial.println("Connected to wifi");
printWifiStatus();

Then, we connect to Google website using the client.connect() function. After connected to Google website, we can make an HTTP GET request.

```
Serial.println("\nStarting connection to server...");
if (client.connect(server, 80)) {
    Serial.println("connected to server");
    // Make a HTTP request:
    client.println("GET /search?q=arduino HTTP/1.1");
    client.println("Host: www.google.com");
    client.println("Connection: close");
    client.println();
}
```

```
void loop() {
    while (client.available()) {
        char c = client.read();
        Serial.write(c);
    }
}
```

```
// if the server's disconnected, stop the client:
if (!client.connected()) {
    Serial.println();
    Serial.println("disconnecting from server.");
    client.stop();

    // do nothing forevermore:
    while (true);
}
}
```

The following is an implementation of the printWiFiStatus() function. We print the IP address using the WiFi.localIP() function. We also obtain RSSI by calling the WiFi.RSSI() function.

```
void printWifiStatus() {
    // print the SSID of the network you're attached to:
    Serial.print("SSID: ");
    Serial.println(WiFi.SSID());

    // print your WiFi shield's IP address:
    IPAddress ip = WiFi.localIP();
    Serial.print("IP Address: ");
    Serial.println(ip);

    // print the received signal strength:
    long rssi = WiFi.RSSI();
    Serial.print("signal strength (RSSI):");
    Serial.print(rssi);
    Serial.println(" dBm");
}
```

Save all codes. Now you can compile and upload this program into Arduino Nano 33 IoT. We can see program output using Serial Monitor.

Figure 4-5 shows my program output for the WiFiWebClient program.
You can see the output program by accessing the Google website.

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_121_0.png)

Figure 4-5. Programming output for accessing the Google website

## Accessing Network Time Protocol Server

Sometimes we want to get current time on Arduino Nano 33 IoT. We can apply the network time protocol (NTP) server to retrieve current time. This time uses the UTC timezone.

For our demo, we can use a program sample from Arduino. This program is called WiFiUdpNtpClient. You can see this program in Figure 4-6.

We will explore this program. First, this program has two files: WiFiUdpNtpClient.ino and arduino_secret.h. You can set your WiFi SSID name and key on arduino_secret.h file.

```
#define SECRET_SSID "SSID"
#define SECRET_PASS "SSID-PIN"
```

Our core program is implemented in the WiFiUdpNtpClient.ino file. We declare WiFiNINA, WiFi SSID information, and WiFiClient objects.
