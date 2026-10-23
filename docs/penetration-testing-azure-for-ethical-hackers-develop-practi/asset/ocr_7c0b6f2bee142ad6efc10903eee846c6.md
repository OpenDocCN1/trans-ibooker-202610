9. List all the modules in Lava using the following command:

```
Lava $> ls
```

This is what it looks like:

![](bbox=[0.345, 0.18, 0.62, 0.51])

Figure 5.19 – Results of the Lava ls command

Lava has a lot of modules that can be used to exploit resources in different scenarios but in this chapter, we will be using two main modules: vm_list_privileged and vm_rce.

10. Use the vm_list_privileged module to check whether any VM in the subscription is associated with a privileged managed identity. To execute a module in Lava, the command needs to be preceded by the exec keyword:

```
exec vm_list_privileged
```