We can close the socket using the close() method. The complete program is as follows:

```
import socket

if __name__ == "__main__":
    victim_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

    hacker_IP = "192.168.74.128"
    hacker_port = 8008
    hacker_address = (hacker_IP, hacker_port)
    victim_socket.connect(hacker_address)
    data = victim_socket.recv(1024)
    print(data.decode())
    victim_socket.close()
```

Now our hacker and victim programs are complete in their simplest form. A hacker is listening for incoming connections, and the victim tries to connect with the hacker program. Once the connection is established, the hacker sends a message to the victim. The victim receives the message and simply prints it. Both parties then close their respective connections. What we have learned so far is generic socket programming. Once we understand how we can create connections between two devices in a network, we can adopt these programs to create malicious programs that can allow hackers to perform malicious activities on the victim's computer.

Let's tie all this together. First, run the hacker program and then run the victim's program. This time, the connection will be properly established and, on the hacker's machine, you will see the following output:

![](bbox=[0.128, 0.615, 0.868, 0.71])

Similarly, the victim will receive the message and display it on screen:

![](bbox=[0.128, 0.75, 0.855, 0.83])