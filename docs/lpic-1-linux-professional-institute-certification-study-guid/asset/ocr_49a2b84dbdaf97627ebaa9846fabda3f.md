The output from the ls command is sent directly to the sort command as input but behind the scenes. You don't see the output from the ls command displayed on the monitor; you see only the output from the last command in the pipe line, which in this case is the sort command. There's no limit to how many commands you can link together with piping.

> The >, >>, and | symbols are part of a group of characters often referred to as metacharacters. Metacharacters are characters that have a special meaning when used in the Linux shell. If you need to use a metacharacter as a standard character (such as using the > character as a greater-than symbol in your output instead of as a redirect symbol), you must identify the metacharacter by either placing a backslash (\\) in front of it or enclosing the metacharacter in single (') or double quotes ("). This method is called escaping.

# The Shell Script Format

Placing multiple commands on a single line, either by using the semicolon or piping, is a great way to process data, but it can still get rather tedious. Each time you want to run the set of commands, you need to type them all at the command prompt.

However, Linux allows us to place multiple commands in a text file and then run the text file as a program from the command line. This is called a shell script because we're scripting out commands for the Linux shell to run.

Shell script files are plain text files. To create a shell script file, you just need to use any text editor that you're comfortable with. If you're working from a KDE-based graphical desktop, you can use the KWrite program, or if you're working from a GNOME-based graphical desktop, you can use the GEdit program.

> Shell script files must be text files. If you use a word processing program to create the file (or even copy and paste text from a word processing document into a file), the script will not run correctly in the shell.

If you're working directly in a command-line environment, you still have some options. Many Linux distributions include either the pico or nano editor to provide a graphical editor environment by using ASCII control characters to create a full-screen editing window.

If your Linux distribution doesn't include either the pico or nano editor, there is still one last resort: the vi editor. The vi editor is a text-based editor that uses simple single-letter commands. It's the oldest text editor in the Linux environment, dating back to the early days of Unix, which may be one reason why it's not overly elegant or user-friendly.

When you've chosen your text editor, you're ready to create your shell scripts. First, for your shell script to work you'll need to follow a specific format for the shell script file. The