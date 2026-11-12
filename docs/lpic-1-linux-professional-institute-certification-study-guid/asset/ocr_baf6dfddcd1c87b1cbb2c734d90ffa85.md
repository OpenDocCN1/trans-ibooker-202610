For sending email messages, the binmail program has the following basic syntax:
mail [OPTIONS] recipient...

The recipient can be either a username on the same system or a fully qualified email address, such as jdoe@example.com. You also can designate multiple addresses as the recipient. A few of the more commonly used OPTIONS include:

+   - s subject: Adds a subject line to the email. If your subject contains spaces, you will need to encase it in quotation marks.
+   - cc recipient: Designates an email address or addresses to receive a copy of the message. All email recipients can see this address or addresses.
+   - bc recipient: Designates an email address or addresses to receive a copy of the message. Only the sender can see this address or addresses.
+   - v: Displays delivery details for the email message

An example of sending an email to a local user on the same Linux system is shown in Listing 7.24.

```
Listing 7.24: Using mail to send an email message
$ mail -s "LPIC-1 Book Progress" rich
Hi Rich,
I'm working on Chapter 7 right now.
How's Chapter 8 coming along?
Best regards,
Christine
EOT
$
```

Notice in Listing 7.24 that the subject matter is enclosed in quotation marks. This is important if your email subject line has spaces within it. Because the email is being sent to a user, rich, on the same system, only the username is needed.

When you press the Enter key after typing the mail command syntax, the program waits for you to enter your message without any prompts. When you have finished typing your message, you'll need to signal the mail utility to send the message. To do this, press the Enter key and then the Ctrl+D key combination. You'll see an EOT, which stands for "end of transmission," and be returned to the shell command-line prompt.

> If your system is using Postfix as its MTA and you have usernames that are not lowercase, be aware that email may not be delivered. You'll have to either make the usernames all lowercase or dig into the various fixes for allowing multiple-case usernames with Postfix. You can check the /var/log/maillog or /var/log/mail.log file to find messages concerning undeliverable emails.