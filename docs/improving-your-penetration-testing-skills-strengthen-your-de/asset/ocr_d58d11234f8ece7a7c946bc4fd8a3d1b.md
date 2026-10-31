```
msf > db_status
[*] postgresql connected to msf
```

If you want the database to connect every time you launch msfconsole, copy the database configuration file to the .msf4 directory which was created in your home directory by the Metasploit installer.

# Creating workspaces

Workspaces in Metasploit are used to separate datasets, allowing you to stay organized. It is a good idea to create a new workspace to organize all your collected data before starting a new penetration test, thereby avoiding contamination by previous tests.

# How to do it...

+   1. The default workspace is selected when connecting to the database, which is represented by the * character before its name:
+   
+   ```
+   msf > workspace
+   * default
+   ```
+   2. To display the usage for the workspace command, use the -h option as follows:
+   
+   ```
+   msf > workspace -h
+   Usage:
+       workspace                List workspaces
+       workspace -v             List workspaces verbosely
+       workspace [name]         Switch workspace
+       workspace -a [name] ...  Add workspace(s)
+       workspace -d [name] ...  Delete workspace(s)
+       workspace -D             Delete all workspaces
+       workspace -r <old> <new> Rename workspace
+       workspace -h             Show this help information
+   ```
+   3. To add a new workspace, use the -a option followed by the name of the workspace:
+   
+   ```
+   msf > workspace -a book
+   [*] Added workspace: book
+   ```