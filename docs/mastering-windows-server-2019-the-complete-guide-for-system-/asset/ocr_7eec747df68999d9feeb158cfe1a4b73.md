# Shifting the primary server to FS02

After poking around on the file servers, you'll notice that you can see data on FS01's S: drive, but if you try to browse FS02's S: drive you are met with an error message. This is by design. You are unable to browse and see data on the destination SR server, unless you run another command to make it primary, such as in the event of a disaster recovery scenario. To prove that SR is really working, let's follow that step to make sure data is automatically replicating from FS01 to FS02.

I have created some sample text files on FS01's S: drive, but I am unable to verify that they exist on FS02. Let's pretend that FS01 has gone offline or is having some other kind of issue, and I need to make FS02 my new primary file server. All I need to do is run the following PowerShell command to flip roles, turning FS02 into the new primary SR partner:

```
Set-SRPartnership -NewSourceComputerName FS02 -SourceRGName RepGroup2 -DestinationComputerName FS01 -DestinationRGName RepGroup1
```

Immediately after running this command, I can now see and browse through the data on FS02's S: drive, which means that it is now the primary SR member, and also proves to me that SR was working in the first place:

![](bbox=[0.13, 0.47, 0.87, 0.86])

Figure 12.29: We have shifted the primary drive access to FS02