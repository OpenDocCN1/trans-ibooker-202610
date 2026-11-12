```
Router(config)# access-list ACL_# remark remark
```

or

```
Router(config)# ip access-list standard|extended ACL_name  Router(config-{std|ext}-acl)# remark remark
```

The remark can be up to 100 characters in length.

![](bbox=[0.14, 0.28, 0.24, 0.34])

**TIP** Go ahead and use copious remarks in your ACLs, since some ACLs can have thousands of statements in them! Without the remark feature, you would have an almost impossible task of determining what a statement or group of statements was doing when you are confronted with so many ACL entries.

# Access List Verification

Once you have created and activated your ACLs, you can verify their configuration and operation with various **show** commands. One common command that you can use is the Privilege EXEC **show running-config** command, which will display your ACL and the interface or interfaces on which it is activated. However, you can use many other commands as well.

If you simply want to see which ACLs are activated on your router’s interfaces, you can use the **show ip interfaces** command: