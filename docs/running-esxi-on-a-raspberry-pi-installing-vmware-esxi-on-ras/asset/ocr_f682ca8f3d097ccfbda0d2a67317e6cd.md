7. Install the tools needed to compile VMware Tools by entering

```
apt update
apt install build-essential -y
```

8. Download the VMware Tools build script, make it executable, and execute it by entering

```
wget https://raw.githubusercontent.com/FentonTom/VMware_Tools_Ubuntu/main/Ubuntu2004
chmod 755 Ubuntu2004
./Ubuntu2004
```

9. When VMware Tools is compiled and installed, you will get a message that the service has started.

![](bbox=[0.08, 0.49, 0.87, 0.64])

10. Return to your ESXi Host Client and select the Ubuntu VM. It will show that VMware Tools is installed and running on it. You may need to refresh the screen.