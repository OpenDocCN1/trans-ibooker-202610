# List Subfolder Contents with dir /s and find

On Windows, running dir /s displays the cwd’s folders and their subfolders. For example, the following command displays every .py file in my C:\github\ ezgemail folder and all of its subfolders:

```
C:\github\ezgmail>dir /s *.py
Volume in drive C is Windows
Volume Serial Number is DEE0-8982

Directory of C:\github\ezgmail

06/17/2019  06:58 AM            1,396 setup.py
               1 File(s)            1,396 bytes

Directory of C:\github\ezgmail\docs

12/07/2018  09:43 PM            5,504 conf.py
               1 File(s)            5,504 bytes

Directory of C:\github\ezgmail\src\ezgmail

06/23/2019  07:45 PM           23,565 __init__.py
12/07/2018  09:43 PM               56 __main__.py
               2 File(s)           23,621 bytes

    Total Files Listed:
               4 File(s)           30,521 bytes
               0 Dir(s)  505,407,283,200 bytes free
```

The find . -name command does the same thing on macOS and Linux:

```
al@ubuntu:~/Desktop$ find . -name "*.py"
./someSubFolder/eggs.py
./someSubFolder/bacon.py
./spam.py
```

The . tells find to start searching in the cwd. The -name option tells find to find folders and filenames by name. The "*.py" tells find to display folders and files with names that match the *.py pattern. Note that the find command requires the argument after -name to be enclosed in double quotes.

# Copy Files and Folders with copy and cp

To create a duplicate of a file or folder in a different directory, run copy [source file or folder] [destination folder] or cp [source file or folder] [destination folder]. Here’s an example in a Linux terminal window:

```
al@ubuntu:~/someFolder$ ls
hello.py  someSubFolder
al@ubuntu:~/someFolder$ cp hello.py someSubFolder
al@ubuntu:~/someFolder$ cd someSubFolder
al@ubuntu:~/someFolder/someSubFolder$ ls
hello.py
```