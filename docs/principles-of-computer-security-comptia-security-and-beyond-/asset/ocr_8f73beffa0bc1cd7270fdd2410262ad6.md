Step 2 Discover targets and clients with Wifite and then launch an attack to capture the WPA2 password.

+   a. Start Wifite with the command

```
sudo wifite --new-hs --dict ./crack-it
```

where **crack-it** is the name of the text file in the current directory containing the WPA2 password of your SSID.

Wifite will start scanning, as shown in Figure 12-6.

![](bbox=[0.185, 0.295, 0.815, 0.47])

# FIGURE 12-6 Wifite starting

Wifite will find targets (APs) and clients. Give it a few minutes to collect information and many clients. You’ll see a list of targets and the number of clients shortly thereafter, as shown in Figure 12-7.