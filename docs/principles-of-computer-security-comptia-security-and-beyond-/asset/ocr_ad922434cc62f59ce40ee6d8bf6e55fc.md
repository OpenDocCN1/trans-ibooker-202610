a. On earlier Microsoft operating systems (before Windows 10) in ophcrack, you were able to click the Load button and then Encrypted SAM. On Windows 10, that doesn’t work anymore. Fear not, as Mimikatz, which was downloaded, installed, and run earlier, is the tool that will once again grab the Windows password hashes. Although, we already have grabbed Windows hashes.

b. Earlier we put the NT hash, obtained with Mimikatz, into a format that John the Ripper wanted. Now we’ll put the NT hash into a format that ophcrack requires:

```
<Username>:<User ID>:<LM Hash>:<NT Hash>::
```

In reality, the Username and UserID fields could be anything and don’t have to be the actual ones that the hash is associated with.

Open the formattedhashes.txt file from the previous lab exercise, located at C:\Users\<your username>\Downloads\mimikatz_trunk\x64.

If you want to use my two entries from the previous lab exercise, they are:

```
Beatles:5be2f274f2f80c5d4d0c597f023f4f61:::
StarWars:b7c899154197e8a2a33121d76a240ab5:::
```

As the entries are currently in the format that John the Ripper is expecting, you have to make some small modifications to put them in the format that ophcrack is expecting. Following the preceding format specification, the entries should now appear as follows:

```
Beatles:64::5be2f274f2f80c5d4d0c597f023f4f61::
StarWars:66::b7c899154197e8a2a33121d76a240ab5::
```

Save the file as formattedhashes2.txt.

c. Select Load | PWDUMP File and browse to the formattedhashes2.txt file from the previous step, which is found at C:\Users\<your username>\Downloads\mimikatz_trunk\x64. Alternatively, select Load | Single Hash and paste a single hash in. You should see the user accounts and their corresponding hashes in the main pane.

d. Click the Crack button.