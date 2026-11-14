
# To build PowerShell scripting into your own program, use the PowerShell's `System.Management.Automation` SDK

## Discussion

One of the fascinating aspects of PowerShell is how easily it lets you add many of its capabilities to your own program. This is because PowerShell is, at its core, a powerful engine that any application can use. The PowerShell console application is in fact just a text-based interface to this engine.

Although a full discussion of the PowerShell hosting model is outside the scope of this book, the following example illustrates the techniques behind exposing features of your application for your users to script.

To frame the premise of [Example 17-10](#ch17.xhtml#ruleswizardexamplecs) (shown later), imagine an email application that lets you run rules when it receives an email. While you'll want to design a standard interface that allows users to create simple rules, you also will want to provide a way for users to write incredibly complex rules. Rather than design a scripting language yourself, you can simply use PowerShell's scripting language. In the following example, we provide user-written scripts with a variable called `$message` that represents the current message and then runs the commands.

```
PS > Get-Content VerifyCategoryRule.ps1
if($message.Body -match "book")
{
    [Console]::WriteLine("This is a message about the book.")
}
else
{
    [Console]::WriteLine("This is an unknown message.")
}
PS > .\RulesWizardExample.exe .\VerifyCategoryRule.ps1
This is a message about the book.
```

For more information on how to host PowerShell in your own application, see the MSDN topic "Windows PowerShell Host Quickstart," available in the [PowerShell Developer's Documentation](https://oreil.ly/H1Y8M).

### Step 1: Download the .NET SDK

The .NET SDK contains the compiler, reference assemblies, and other information that you will need to develop PowerShell hosting applications. This example installs the .NET SDK to a temporary working directory, but there are also options to install the SDK system-wide if you wish:

```
Invoke-WebRequest https://dot.net/v1/dotnet-install.ps1 -OutFile ./dotnet-install.ps1
.\dotnet-install.ps1 -Channel Current
```

### Step 2: Create a project to hold the cmdlet source code and PowerShell SDK

```
dotnet new console --name RulesWizardExample
cd RulesWizardExample
dotnet add package System.Management.Automation
```

### Step 3: Customize the application source code

Edit the file called `Program.cs` with the content from [Example 17-10](#ch17.xhtml#ruleswizardexamplecs), and save it on your hard drive.

#### Example 17-10. Program.cs

```
using System;
using System.Management.Automation;
using System.Management.Automation.Runspaces;

namespace Template
{
    // Define a simple class that represents a mail message
    public class MailMessage
    {
        public MailMessage(string to, string from, string body)
        {
            this.To = to;
            this.From = from;
            this.Body = body;
        }

        public String To;
        public String From;
        public String Body;
    }

    public class RulesWizardExample
    {
        public static void Main(string[] args)
        {
            // Ensure that they've provided some script text
            if(args.Length == 0)
            {
                Console.WriteLine("Usage:");
                Console.WriteLine(" RulesWizardExample <script text>");
                return;
            }

            // Create an example message to pass to our rules wizard
            MailMessage mailMessage =
                        new MailMessage(
                            "guide_feedback@leeholmes.com",
                            "guide_reader@example.com",
                            "This is a message about your book.");

            // Create a variable, called "$message" in the Runspace, and populate
            // it with a reference to the current message in our application.
            // Scripts in the PowerShell instance can interact with this object like any
            // other .NET object.
            InitialSessionState iss = InitialSessionState.CreateDefault2();
            iss.Variables.Add(
                new SessionStateVariableEntry(
                    "message", mailMessage, "The message to be processed."));

            // Create a PowerShell instance (an environment for running commands) based on
            // that initial session state
            using(PowerShell psInstance = PowerShell.Create(iss))
            {
                // Add a script (given in the first command line argument) to the
                // PowerShell instance
                psInstance.AddScript(args[0]);

                // Invoke (execute) the pipeline.
                psInstance.Invoke();
            }
        }
    }
}
```

### Step 4: Compile the project

```
dotnet build
```

### Step 5: Run the project

Although the example itself provides very little functionality, it demonstrates the core concepts behind adding PowerShell scripting to your own program.

Here we give our rules wizard a simple rule to just output the sender of the sample mail message:

```
PS > bin\Debug\net5.0\RulesWizardExample.exe '[Console]::WriteLine($message.From)'
guide_reader@example.com
```

or, we can have it run more complicated rules based on the script we saw earlier:

```
PS > bin\Debug\net5.0\RulesWizardExample.exe .\VerifyCategoryRule.ps1
This is a message about the book.
```

## See Also

[Structured Commands (Cmdlets)](#preface02.xhtml#structured_commands_cmdlets)

# Chapter 18. Security and Script Signing

## 18.0 Introduction

Security plays two important roles in PowerShell. The first role is the security of PowerShell itself. Scripting languages have long been a vehicle of email-based malware on Windows, so PowerShell's security features have been carefully designed to thwart this danger. The second role is the set of security-related tasks you are likely to encounter when working with your computer: script signing, certificates, and credentials, just to name a few.

When it comes to talking about security in the scripting and command-line world, a great deal of folklore and superstition clouds the picture. One of the most common misconceptions is that scripting languages and command-line shells somehow let users bypass the security protections of the Windows graphical user interface.

The Windows security model protects resources—not the way you get to them. That's because, in effect, the programs that you run *are* you. If you can do it, so can a program. If a program can do it, then you can do it without having to use that program. For example, consider the act of changing critical data in the Windows Registry. If you use the Windows Registry Editor graphical user interface, it provides an error message when you attempt to perform an operation that you don't have permission for, as shown in [Figure 18-1](#ch18.xhtml#error_message_from_the_windows_registry_).

![](Images/wps4_1801.png)

The Registry Editor provides this error message because it's *unable* to delete that key, not because it wanted to prevent you from doing it. Windows itself protects the registry keys, not the programs you use to access them.

Likewise, PowerShell provides an error message when you attempt to perform an operation that you don't have permission for—not because PowerShell contains extra security checks for that operation, but simply because Windows itself prevents the operation:

```
PS > New-Item "HKLM:\Software\Microsoft\Windows\CurrentVersion\Run\New"
New-Item : Requested registry access is not allowed.
At line:1 char:9
+ New-Item <<<< "HKLM:\Software\Microsoft\Windows\CurrentVersion\Run\New"
```

While perhaps clear after explanation, this misunderstanding often gets used as a reason to prevent users from running command shells or scripting languages altogether.

## Defending Against PowerShell Attacks
