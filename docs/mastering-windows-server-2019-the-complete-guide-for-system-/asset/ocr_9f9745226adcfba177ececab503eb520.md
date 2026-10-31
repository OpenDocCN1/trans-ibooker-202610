Set Allow remote server management through WinRM to Enabled, as follows:

![](bbox=[0.13, 0.108, 0.87, 0.498])

Figure 11.27: Enabling remote management via GPO

# Allowing machines from other domains or workgroups

If you are working with servers that are all part of the same corporate domain, which will most often be the case, then authentication between machines is easy to accomplish. They automatically trust each other at this level. However, on the server you are prepping to accept remote connections, if you expect those remote computers will be members of a different domain that is not trusted – or even members of a workgroup – then you will have to issue another command to manually trust the individual computers that are going to be connecting in. For example, if I am planning to manage all of my servers from a client computer called Win10Client that is not trusted by the servers, I would need to run the following command on these servers:

```
Set-Item wsman:\localhost\client\trustedhosts Win10Client
```