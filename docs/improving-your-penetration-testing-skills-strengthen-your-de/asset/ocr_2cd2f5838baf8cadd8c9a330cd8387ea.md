# How to do it...

Adding a new DLL definition to Railgun is an easy task. Suppose you want to add a DLL that ships with Windows, but is not present in your Railgun; you can create a DLL definition under /usr/share/metasploit-framework/lib/rex/post/meterpreter/extensions/stdapi/railgun/def, select the Linux, macOS, or Windows operating system folder, and name it def_dllname.rb.

The following template should demonstrate how a DLL is defined:

```
# -*- coding: binary -*-
module Rex
module Post
module Meterpreter
module Extensions
module Stdapi
module Railgun
module Def

class Def_somedll

  def self.create_dll(dll_path = 'somedll')
    dll = DLL.new(dll_path, ApiConstants.manager)

    # 1st argument = Name of the function
    # 2nd argument = Return value's data type
    # 3rd argument = An array of parameters
    dll.add_function('SomeFunction', 'DWORD', [
      ["DWORD","hwnd","in"]
    ])

    return dll
  end
end
end; end; end; end; end; end
```

+   1. For this recipe, first we need to create a backup of the original def_shell32.rb Railgun DLL definition so we can write our own:

```
root@kali:~# cd /usr/share/metasploit-framework/lib/rex/post/meterpreter/extensions/stdapi/railgun/def/windows
root@kali:/usr/share/metasploit-framework/lib/rex/post/meterpreter/extensions
```