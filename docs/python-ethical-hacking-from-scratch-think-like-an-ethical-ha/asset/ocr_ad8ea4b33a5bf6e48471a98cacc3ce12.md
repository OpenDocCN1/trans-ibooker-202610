Now, if everything is done properly, you will see a folder created named venv.

Next, activate the virtual environment by running the following command:

```
source venv/bin/activate
```

Now, your virtual environment should be activated. Once it is activated, you can see the installed packages in the environment by typing the following command:

```
pip freeze
```

If you have any packages already installed in the environment, they will be listed down; otherwise, you will have nothing showing. Now, to install Scapy, write the following command:

```
pip3 install scapy
```

This should take some time to install. Once done, you can write the pip freeze command again to see the installed packages:

![](bbox=[0.32, 0.425, 0.65, 0.5])

In this section, we have learned how we can install Scapy in our virtual environment and how to see whether it has been installed properly. Note that some functionalities of Scapy require the program to be run with higher privileges, otherwise they won’t work. In the next section, we will learn more about how Scapy is used and how we can manipulate network packets with Scapy.

# Understanding how Scapy works

In this part, we will learn about how Scapy works and how we can use it to create our own network manipulation tools. Let’s create a new file called main.py and open it. Once the file is open, we can import any Scapy module inside the file. In this section, we will create a small ping request to any website. Ping requests are usually used to test whether a device is available or not. A ping request (also called an echo request) uses an underlying ICMP application layer protocol. To import a package inside your program, write the following code:

```
from scapy.all import scapy
```