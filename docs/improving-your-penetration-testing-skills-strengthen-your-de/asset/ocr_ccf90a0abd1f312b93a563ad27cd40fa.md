The developer may set filters to block the semicolon metacharacter. This would block your injected data, and therefore you need to experiment with other metacharacters too, as shown in the following table:

| Symbol | Usage |
|---|---|
| ; | The semicolon is the most common metacharacter used to test an injection flaw. The shell runs all of the commands in sequence separated by the semicolon. |
| && | The double ampersand runs the command to the right of the metacharacter only if the command to the left executed successfully.
An example would be to inject the password field along with the correct credentials. A command can be injected that will run once the user is authenticated to the system. |
| || | The double pipe metacharacter is the direct opposite of the double ampersand. It runs the command on the right-hand side only if the command on the left-hand side failed. The following is an example of this command:
`cd invalidDir || ping -c 2 attacker.com` |
| ( ) | Using the grouping metacharacter, you can combine the outputs of multiple commands and store them in a file. The following is an example of this command:
`(ps; netstat) > running.txt` |
| ` | The single quote metacharacter is used to force the shell to interpret and run the command between the backticks. The following is an example of this command:
`Variable= "OS version `uname -a`" && echo $variable` |
| >> | This character appends the output of the command on the left-hand side to the file named on the right-hand side of the character. The following is an example of this command:
`ls -la >> listing.txt` |
| | | The single pipe will use the output of the command on the left-hand side as an input to the command specified on the right-hand side. The following is an example of this command:
`netstat -an | grep :22` |