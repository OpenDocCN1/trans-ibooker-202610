In Listing 1.62, the output from the grep command is fed as input into the cut command. The cut utility removes only the first field from each password record, which is the account username. The output of the cut command is used as input into the sort command, which alphabetically sorts the usernames. Finally, the sort utility's output is piped as input into the less command for leisurely perusing through the account usernames.

In cases where you want to keep a copy of the command pipeline's output as well as view it, the tee command will help. Similar to a tee pipe fitting in plumbing, where the water flow is sent in multiple directions, the tee command allows you to both save the output to a file and display it to STDOUT. Listing 1.63 contains an example of this handy command.

# Listing 1.63: Employing the tee command

```
$ grep /bin/bash$ /etc/passwd | tee BashUsers.txt
root:x:0:0:root:/root:/bin/bash
user1:x:1000:1000:Student User One:/home/user1:/bin/bash
Christine:x:1001:1001::/home/Christine:/bin/bash
$
$ cat BashUsers.txt
root:x:0:0:root:/root:/bin/bash
user1:x:1000:1000:Student User One:/home/user1:/bin/bash
Christine:x:1001:1001::/home/Christine:/bin/bash
$
```

The first command in Listing 1.63 searches the password file for any user account records that end in /bin/bash. That output is piped into the tee command, which displays the output as well as saves it to the BashUsers.txt file. The tee command is handy when you are installing software from the command line and want to see what is happening as well as keep a log file of the transaction for later review.

# Using sed

Another interesting command-line program is a stream editor. There are times where you will want to edit text without having to pull out a full-fledged text editor. A stream editor modifies text that is passed to it via a file or output from a pipeline. This editor uses special commands to make text changes as the text "streams" through the editor utility.

The command to invoke the stream editor is sed. The sed utility edits a stream of text data based on a set of commands you supply ahead of time. It is a very quick editor because it makes only one pass through the text to apply the modifications.

The sed editor changes data based on commands either entered into the command line or stored in a text file. The process the editor goes through is as follows:

+   1. Reads one text line at a time from the input stream
+   2. Matches that text with the supplied editor commands
+   3. Modifies the text as specified in the commands
+   4. Displays the modified text