# Step 1 Unpack a binary with UPX.

+   a. To decompress (-d) and unpack Lab01-02.exe and then write to an output (-o) file, type the following, as shown in Figure 15-2:

![](bbox=[0.116, 0.186, 0.882, 0.56])

# FIGURE 15-2 Running UPX

```
upx -o Unpacked_Lab01-02.exe -d Lab01-02.exe
```

Notice the 18.75 percent compression ratio that was used as well as the other information provided.

+   b. Type upx --help | more for usage information.

+   c. Copy the Unpacked_Lab01-02.exe file to the Strings directory.

2a

2b–2d

# Step 2 Analyze the unpacked Lab01-02.exe with Strings and form a