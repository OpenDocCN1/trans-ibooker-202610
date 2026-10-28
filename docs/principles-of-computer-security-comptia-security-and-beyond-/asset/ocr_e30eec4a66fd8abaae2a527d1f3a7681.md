again. You’ll notice from these commands that the port status is Secure-shutdown, the unauthorized MAC address (BAD1.BAD2.BAD3) is logged, and the line protocol is down (err-disabled) for interface FastEthernet0/1.

```
show port-security
show port-security interface FastEthernet0/1
show interface FastEthernet0/1
```

4a–4c

# Step 4 Allow the port that was shut down by port security to come back up and return to the way it was before it entered the err-disabled state.

+   a. To recover from this state, an administrator must manually disable (yes, disable) the interface with the **shutdown** interface command and then enable the interface with the **no shutdown** command. So, now is a good time to execute the following commands in interface configuration mode for interface FastEthernet0/1:
+   
+   shutdown
+   no shutdown

+   b. Another look at the output from these commands shows that the port status has now returned to Secure-up, the last MAC address heard on the interface is still the unauthorized one, and the line protocol is up (connected) for interface FastEthernet0/1:
+   
+   show port-security
+   show port-security interface FastEthernet0/1
+   show interface FastEthernet0/1

+   c. Now to get things fully back to normal. If you look in the running-config, you’ll notice the following line (your MAC address might vary):
+   
+   switchport port-security mac-address sticky 0060.2FE3.20C0
+   
+   That was the MAC address of PC0 before you changed it. Restore that MAC address back to PC0 now.
+   
+   If you wanted to remove the sticky address from the configuration (which isn’t necessary now and shouldn’t be done), you would use this the command: