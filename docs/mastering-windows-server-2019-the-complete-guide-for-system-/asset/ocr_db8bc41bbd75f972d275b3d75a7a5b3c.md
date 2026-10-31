We can take the output of this Get-Service cmdlet and pipe it to Stop-Service, and the resulting command will immediately stop all services that begin with the word "Hyper":

```
Get-Service -DisplayName hyper* | Stop-Service
```

# PowerShell Integrated Scripting Environment

Most server administrators are familiar with the concept of creating batch files for use in the Command Prompt world. Have a series of commands that you want to run in sequence? Need to run this sequence of commands multiple times across different servers or over and over again in the future? Throwing multiple commands inside a text document and then saving it with the .BAT file extension will result in a batch file that can be run on any Windows computer, issuing those commands in sequence, which saves you the time and effort of having to plunk out these commands over and over inside the command-line interface.

Scripting in PowerShell is the same idea but is much more powerful. Commands in Command Prompt are useful, but limited, while PowerShell cmdlets have the ability to manipulate anything within the operating system. With PowerShell, we can reference items from inside environment variables or the registry, we can easily issue commands to remote systems, and we can even utilize variables inside a PowerShell script, just like you would do with any full programming language.

Let's explore a couple of different ways that can be used to start creating your first PowerShell scripts.

# PS1 files

Creating a simple .PS1 file (a PowerShell script file) is almost exactly the same idea as creating a .BAT file. Simply open up a text document using your favorite editor, throw in a series of commands or cmdlets, and then save the file as FILENAME.PS1. As long as your PowerShell environment allows the running of scripts – see earlier in the chapter about the DEP – you now have the ability to double-click on that .PS1 file, or launch it from any PowerShell prompt, to run the series of cmdlets inside that script. Let's give it a try and prove that we can get a simple script up and operational.

Since you are only going to create scripts that serve a purpose, let's think of a real-world example. I work with terminal servers quite a bit – pardon me, RDS servers – and a common request from customers is a log of what users logged in to which servers.