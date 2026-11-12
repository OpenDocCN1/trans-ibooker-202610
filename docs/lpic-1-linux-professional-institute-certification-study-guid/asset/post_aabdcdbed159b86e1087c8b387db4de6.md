
The shell script shown in Listing 9.7 adds three echo commands to the test1.sh script. Notice that the first echo command doesn't use any quotes, but the third one does. The reason for that is the text output from the third echo command contains single quotes. The single quote is also a metacharacter in the shell, which will confuse the echo command, so you need to place double quotes around the text. Also notice that the second echo command doesn't have any text on the line. That outputs a blank line, which is useful when you want to separate output from multiple commands.

## Using Variables in Scripts

Part of programming is the ability to temporarily store data to use later on in the program. As mentioned earlier in the "Shell Variables" section, you do that in the Bash shell by using either global or local variables. This method also works when working with shell scripts.

### Using Global Environment Variables

As discussed earlier in the "Using Variables" section, global environment variables are often used to track specific system information, such as the name of the system, the name of the user logged into the shell, the user's user ID (UID), the default home directory for the user, and the search path the shell uses to find executable programs. You can tap into these environment variables from within your scripts by using the environment variable name, preceded by a dollar sign, as shown in Listing 9.8.

Listing 9.8: The test2.sh shell script file to display environment variables

```bash
$ cat test2.sh
#!/bin/bash
# display user information from the system.
echo User info for userid: $USER
echo UID: $UID
echo HOME: $HOME
$
```

The $USER, $UID, and $HOME environment variables are commonly used to display information about the logged-in user. If you run the test2.sh shell script shown in Listing 9.8, the output should look like this:

```bash
$ chmod u+x test2.sh
$ ./test2.sh
User info for userid: rich
UID: 1000
HOME: /home/rich
$
```

The values you see should be related to your user account. This allows you to dynamically retrieve information about the user account running your shell script to customize the output.

### Defining Local Variables

Most variables used in shell scripts are local variables used for storing your own data within your shell scripts. You assign values to local variables using the equal sign. Spaces must not appear between the variable name, the equal sign, and the value. Here are a few examples:

```bash
var1=10
var2=23.45
var3=testing
var4="Still more testing"
```

The shell script automatically determines the data type used for the variable value. Listing 9.9 shows an example of writing a shell script that uses local variables.

### Listing 9.9: Using local variables in a shell script

```bash
$ cat test3.sh
#!/bin/bash
# testing variables
days=10
guest=Katie
echo $guest checked in $days days ago
$
```

Running the test3.sh script from Listing 9.9 produces the following output:

```bash
$ chmod u+x test3.sh
$ ./test3.sh
Katie checked in 10 days ago
$
```

After you store the data in a local variable, you can reference it anywhere in your shell script. However, remember that local variables defined within the shell script are accessible only from within the shell script by default. If you want the variables within a shell script to be visible from the parent shell that launched the shell script, use the export command.

> Be careful when using variables within the echo statement. Since variable names are just text values, if you try to append text to a variable name, the shell will then consider the text as part of the variable name and you won't get the results you expected. If you need to do that, you can enclose the variable name in braces, such as ${guest}. This ensures any text appended to the end of the variable will be separate from the variable name.

## Command-Line Arguments

One of the most versatile features of shell scripts is the ability to pass data into the script when you run it. This allows you to customize the script with new data each time you run it.

One method of passing data into a shell script is to use command-line arguments. Command-line arguments are data you include on the command line when you run the command. Just start listing them after the command, separating each data value with a space, in this format:

```bash
command argument1 argument2 ...
```

You retrieve the values in your shell script code using special numeric positional variables. Use the variable $1 to retrieve the first command-line argument, $2 the second argument, and so on. Listing 9.10 shows how to use positional variables in your shell script.

### Listing 9.10: Using command-line arguments in a shell script

```bash
$ cat test4.sh
#!/bin/bash
# Testing command line arguments
echo $1 checked in $2 days ago
$ chmod u+x test4.sh
$ ./test4.sh Barbara 4
Barbara checked in 4 days ago
$ ./test4.sh Jessica 5
Jessica checked in 5 days ago
$
```

The test4.sh shell script uses two command-line arguments. The $1 variable holds the name of the person, and the $2 variable holds the number of days ago they checked in. When you run the test4.sh shell script, be sure to include both data values in the command line. The shell won't produce an error message if a positional variable doesn't exist; you just won't get the results you expected:

```bash
$ ./test4.sh rich
rich checked in days ago
$
```

It's up to you to check if the positional variable exists within your program code. We'll explore how to do that later when we discuss logic statements.

## Getting User Input

Providing command-line options and parameters is a great way to get data from your script users, but sometimes your script needs to be more interactive. There are times when you have to ask a question while the script is running and wait for a response from the person running your script. The Bash shell provides the read command just for this purpose.

### Basic Reading

The read command accepts input from the standard input (the keyboard) or from another file descriptor. After receiving the input, the read command places the data into a standard variable. Listing 9.11 shows the read command at its simplest.

### Listing 9.11: Accepting user input in your scripts

```bash
$ cat test5.sh
#!/bin/bash
# testing the read command

echo -n "Enter your name: "
read name
echo "Hello $name, welcome to my program."
$ ./test5.sh
Enter your name: Rich Blum
Hello Rich Blum, welcome to my program.
$
```

Notice that the echo command that produced the prompt uses the -n option. This suppresses the newline character at the end of the string, allowing the script user to enter data immediately after the string instead of on the next line. This gives your scripts a more form-like appearance.

In fact, the read command includes the -p option, which allows you to specify a prompt directly in the read command line. Listing 9.12 demonstrates this feature.

### Listing 9.12: Using the read command with the -p option

```bash
$ cat test6.sh
#!/bin/bash
# testing the read -p option

read -p "Please enter your age:" age
days=$[ $age * 365 ]
echo "That makes you over $days days old!"
$ ./test6.sh
Please enter your age:10
That makes you over 3650 days old!
$
```

You'll notice in the Listing 9.11 example, when I typed my name the read command assigned both my first name and last name to the same variable. The read command will assign all data entered at the prompt to a single variable, or you can specify multiple variables. Each data value entered is assigned to the next variable in the list. If the list of variables runs out before the data does, the remaining data is assigned to the last variable:

```bash
$ cat test7.sh
#!/bin/bash
# entering multiple variables

read -p "Enter your name: " first last
echo "Checking data for $last, $first..."
$ ./test7.sh
Enter your name: Rich Blum
Checking data for Blum, Rich...
$
```

You can also specify no variables on the read command line. If you do that, the read command places any data it receives in the special environment variable REPLY:

```bash
$ cat test8.sh
#!/bin/bash
# testing the REPLY environment variable

read -p "Enter a number: "
factorial=1
for (( count=1; count <= $REPLY; count++ ))
do
    factorial=$[ $factorial * $count ]
done
echo "The factorial of $REPLY is $factorial"
$ ./test8.sh
Enter a number: 5
The factorial of 5 is 120
$
```

This example uses the for loop, which is explained later on in "Logic Statements," to iterate through the numbers from 1 to the entered number. The REPLY environment variable will contain all of the data entered in the input, and it can be used in the shell script like any other variable.

### Timing Out

There's a danger when using the read command. It's quite possible that your script will get stuck waiting for the script user to enter data. If the script must go on regardless of whether any data was entered, you can use the -t option specify a timer. The -t option specifies the
