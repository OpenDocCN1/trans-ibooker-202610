The second format for defining a function in a Bash shell script more closely follows how functions are defined in other programming languages:

```
name() {
commands
}
```

The empty parentheses after the function name indicate that you're defining a function. The same naming rules apply in this format as in the original shell script function format. To use a function in your script, specify the function name on a line, just as you would any other shell command. Listing 9.19 demonstrates how to do this.

# **Listing 9.19: Using a function in a shell script**

```
$ cat test16.sh
#!/bin/bash
# using a function in a script

function func1 {
echo "This is an example of a function"
}

count=1
while [ $count -le 5 ]
do
	func1
	count=$[ $count + 1 ]
done

echo "This is the end of the loop"
func1
echo "Now this is the end of the script"
$ ./test16.sh
This is an example of a function
This is an example of a function
This is an example of a function
This is an example of a function
This is an example of a function
This is an example of a function
This is the end of the loop
This is an example of a function
Now this is the end of the script
$
```