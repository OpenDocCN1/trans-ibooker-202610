
Processes remain separate from each other, even when running the same program. For example, if you ran several instances of a Python program at the same time, each process might have separate variable values. Every process, even processes running the same program, has its own cwd and environment variable settings. Generally speaking, a command line will run only one process at a time (although you can have multiple command lines open simultaneously).

Each operating system has a way of viewing a list of running processes. On Windows, you can press CTRL-SHIFT-ESC to bring up the Task Manager application. On macOS, you can run Applications ▸ Utilities ▸ Activity Monitor. On Ubuntu Linux, you can press CTRL-ALT-DEL to open an application also called the Task Manager. These task managers can force a running process to terminate if it’s unresponsive.

## The Command Line

The command line is a text-based program that lets you enter commands to interact with the operating system and run programs. You might also hear it called the command line interface (CLI, which rhymes with “fly”), command prompt, terminal, shell, or console. It provides an alternative to a graphical user interface (GUI, pronounced “gooey”), which allows the user to interact with the computer through more than just a text-based interface. A GUI presents visual information to a user to guide them through tasks more easily than the command line does. Most computer users treat the command line as an advanced feature and never touch it. Part of the intimidation factor is due to the complete lack of hints of how to use it; although a GUI might display a button showing you where to click, a blank terminal window doesn’t remind you what to type.

But there are good reasons for becoming adept at using the command line. For one, setting up your environment often requires you to use the command line rather than the graphical windows. For another, entering commands can be much faster than clicking graphical windows with the mouse. Text-based commands are also less ambiguous than dragging an icon to some other icon. This lends them to automation better, because you can combine multiple specific commands into scripts to perform sophisticated operations.

The command line program exists in an executable file on your computer. In this context, we often call it a shell or shell program. Running the shell program makes the terminal window appear:

- On Windows, the shell program is at C:\Windows\System32\cmd.exe.
- On macOS, the shell program is at /bin/bash.
- On Ubuntu Linux, the shell program is at /bin/bash.

Over the years, programmers have created many shell programs for the Unix operating system, such as the Bourne Shell (in an executable file named sh) and later the Bourne-Again Shell (in an executable file named Bash). Linux uses Bash by default, whereas macOS uses the similar Zsh or Z shell in Catalina and later versions. Due to its different development history, Windows uses a shell named Command Prompt. All these programs do the same thing: they present a terminal window with a text-based CLI into which the user enters commands and runs programs.

In this section, you’ll learn some of the command line’s general concepts and common commands. You could master a large number of cryptic commands and become a real sorcerer, but you only need to know about a dozen or so to solve most problems. The exact command names might vary slightly on different operating systems, but the underlying concepts are the same.

## Opening a Terminal Window

To open a terminal window, do the following:

- On Windows, click the Start button, type Command Prompt, and then press ENTER.
- On macOS, click the Spotlight icon in the upper-right corner, type Terminal, and then press ENTER.
- On Ubuntu Linux, press the WIN key to bring up Dash, type Terminal, and press ENTER. Alternatively, use the keyboard shortcut CTRL-ALT-T.

Like the interactive shell, which displays a >>> prompt, the terminal displays a shell prompt at which you can enter commands. On Windows, the prompt will be the full path to the current folder you are in:

```
C:\Users\Al>your commands go here
```

On macOS, the prompt shows your computer’s name, a colon, and the cwd with your home folder represented as a tilde (~). After this is your username followed by a dollar sign ($):

```
Al’s-MacBook-Pro:~ al$ your commands go here
```

On Ubuntu Linux, the prompt is similar to the macOS prompt except it begins with the username and an at (@) symbol:

```
al@al-VirtualBox:~$ your commands go here
```

Many books and tutorials represent the command line prompt as just $ to simplify their examples. It’s possible to customize these prompts, but doing so is beyond the scope of this book.

## Running Programs from the Command Line

To run a program or command, enter its name into the command line. Let’s run the default calculator program that comes with the operating system. Enter the following into the command line:

- On Windows, enter calc.exe.
- On macOS, enter `open -a Calculator`. (Technically, this runs the `open` program, which then runs the Calculator program.)
- On Linux, enter `gnome-calculator`.

Program names and commands are case sensitive on Linux but case insensitive on Windows and macOS. This means that even though you must type `gnome-calculator` on Linux, you could type `calc.exe` on Windows and `OPEN -a Calculator` on macOS.

Entering these calculator program names into the command line is equivalent to running the Calculator program from the Start menu, Finder, or Dash. These calculator program names work as commands because the `calc.exe`, `open`, and `gnome-calculator` programs exist in folders that are included in the PATH environment variables. “Environment Variables and PATH” on page 35 explains this further. But suffice it to say that when you enter a program name on the command line, the shell checks whether a program with that name exists in one of the folders listed in PATH. On Windows, the shell looks for the program in the cwd (which you can see in the prompt) before checking the folders in PATH. To tell the command line on macOS and Linux to first check the cwd, you must enter `./` before the filename.

If the program isn’t in a folder listed in PATH, you have two options:

- Use the `cd` command to change the cwd to the folder that contains the program, and then enter the program name. For example, you could enter the following two commands:
- Enter the full file path for the executable program file. For example, instead of entering `calc.exe`, you could enter `C:\Windows\System32\calc.exe`.

```
cd C:\Windows\System32
calc.exe
```

On Windows, if a program ends with the file extension `.exe` or `.bat`, including the extension is optional: entering `calc` does the same thing as entering `calc.exe`. Executable programs in macOS and Linux often don’t have file extensions marking them as executable; rather, they have the executable permission set. “Running Python Programs Without the Command Line” on page 39 has more information.

## Using Command Line Arguments

*Command line arguments* are bits of text you enter after the command name. Like the arguments passed to a Python function call, they provide the command with specific options or additional directions. For example, when you run the command `cd C:\Users`, the `C:\Users` part is an argument to the `cd` command that tells `cd` to which folder to change the cwd. Or, when you run a Python script from a terminal window with the `python yourScript.py` command, the `yourScript.py` part is an argument telling the `python` program what file to look in for the instructions it should carry out.

*Command line options* (also called flags, switches, or simply options) are a single-letter or short-word command line arguments. On Windows, command line options often begin with a forward slash (/); on macOS and Linux, they begin with a single dash (-) or double dash (--). You already used the -a option when running the macOS command open -a Calculator. Command line options are often case sensitive on macOS and Linux but are case insensitive on Windows, and we separate multiple command line options with spaces.

Folders and filenames are common command line arguments. If the folder or filename has a space as part of its name, enclose the name in double quotes to avoid confusing the command line. For example, if you want to change directories to a folder called *Vacation Photos*, entering cd Vacation Photos would make the command line think you were passing two arguments, Vacation and Photos. Instead, you enter cd "Vacation Photos":

```
C:\Users\Al>cd "Vacation Photos"

C:\Users\Al\Vacation Photos>
```

Another common argument for many commands is --help on macOS and Linux and /? on Windows. These bring up information associated with the command. For example, if you run cd /? on Windows, the shell tells you what the cd command does and lists other command line arguments for it:

```
C:\Users\Al>cd /?
Displays the name of or changes the current directory.

CHDIR [/D] [drive:][path]
CHDIR [..]
CD [/D] [drive:][path]
CD [..]

..  Specifies that you want to change to the parent directory.

Type CD drive: to display the current directory in the specified drive.
Type CD without parameters to display the current drive and directory.

Use the /D switch to change current drive in addition to changing current directory for a drive.
--snip--
```

This help information tells us that the Windows cd command also goes by the name chdir. (Most people won’t type chdir when the shorter cd command does the same thing.) The square brackets contain optional arguments. For example, CD [/D] [drive:][path] tells you that you could specify a drive or path using the /D option.

Unfortunately, although the /? and --help information for commands provides reminders for experienced users, the explanations can often be cryptic. They’re not good resources for beginners. You’re better off using a
