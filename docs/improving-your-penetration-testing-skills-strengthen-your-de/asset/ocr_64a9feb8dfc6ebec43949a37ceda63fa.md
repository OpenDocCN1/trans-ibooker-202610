To list all the available payloads, we use the show payloads command:

![](bbox=[0.125, 0.222, 0.87, 0.685])

The sessions command has one of my favorite options, -u, which will try to upgrade a shell to a meterpreter session on many platforms, and allows us to take advantage of all the advanced features of meterpreter:

```
msf exploit(usermap_script) > sessions -u 1
[*] Executing 'post/multi/manage/shell_to_meterpreter' on session(s): [1]

[*] Upgrading session ID: 1
[*] Starting exploit/multi/handler
```