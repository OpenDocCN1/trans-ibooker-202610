+   a. Make a directory named monroe:
+   `mkdir monroe`

+   b. Examine the permissions for monroe:
+   `ls -l | grep monroe`

The pipe symbol (|) takes output from the left (**ls -l** returns a long listing of all files and directories) and passes it as input to the right (into the **grep** command). The **grep** (general regular expression) command filters output by the string that follows. This restricts the output to just lines from **ls -l** that match monroe.

+   c. Change the permissions on the directory to allow the U category everything, the G category read and execute permissions, and the O category just read permission:
+   `chmod 754 monroe`

+   d. Examine the new permissions for the monroe directory:
+   `ls -l | grep monroe`

Notice that the O category lost the x permission.

+   e. Change to the monroe directory:
+   `cd monroe`

+   f. Redirect the text meadowbrook to a file named brighton:
+   `echo meadowbrook > brighton`

+   g. Display the contents of the brighton file:
+   `cat brighton`

+   h. Examine the permissions for the brighton file:
+   `ls -l brighton`

Notice that the U category has rw, the G category has r, and the O category has r.

+   i. Switch to the jsw account:
+   `su jsw`

Provide the jsw password when prompted.