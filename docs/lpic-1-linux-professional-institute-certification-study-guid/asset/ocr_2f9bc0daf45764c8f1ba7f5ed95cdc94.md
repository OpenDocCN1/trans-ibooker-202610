Notice that after the `.forward` file was created and the proper permissions set on it, when an email was sent to christine, it was forwarded to rich instead. To stop the forwarding, all christine needs to do is delete the `.forward` file in her `$HOME` directory:

```
$ pwd
/home/christine
$ rm -i .forward
rm: remove regular file '.forward'? y
$
```

> When referring to files in a user's home directory, such as the `.forward` file, it can be written multiple ways. You may see `$HOME/.forward`, `~/.forward`, or `/home/username/.forward`.

# Emulating Commands

Because Sendmail was a popular MTA for so long, the Postfix MTA program wanted to maintain compatibility with it. To accomplish this, Postfix implemented a `sendmail` emulation layer. This allows certain Sendmail commands to work with the Postfix program. These commands include

+   - mailq
+   - sendmail -bp
+   - newaliases
+   - sendmail -I

We've covered all the commands in the preceding list except for `sendmail -I`. It operates just like the `newaliases` command.

You can find out more about the Sendmail emulation layer at www.postfix.org/sendmail.1.html.

Although we've only just touched on managing email, the concepts we've covered here are a good step in the right direction. You can use these various tools to assist in troubleshooting Linux email problems.

# Using Log and Journal Files

Lots of things happen on a Linux system while it's running. Part of your job as a Linux administrator is to know everything that is happening and to watch for when things go wrong. The primary tool for accomplishing that task is the logging service.

All Linux distributions implement some method of `logging`. Logging directs short messages that indicate what events happen, and when they happen, to users, files, or even