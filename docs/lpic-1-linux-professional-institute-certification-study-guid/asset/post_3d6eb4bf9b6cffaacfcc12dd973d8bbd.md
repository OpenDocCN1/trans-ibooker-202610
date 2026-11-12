
For sending email messages, the binmail program has the following basic syntax:
mail [OPTIONS] recipient...

The recipient can be either a username on the same system or a fully qualified email address, such as jdoe@example.com. You also can designate multiple addresses as the recipient. A few of the more commonly used OPTIONS include:

- -s subject: Adds a subject line to the email. If your subject contains spaces, you will need to encase it in quotation marks.
- -cc recipient: Designates an email address or addresses to receive a copy of the message. All email recipients can see this address or addresses.
- -bcc recipient: Designates an email address or addresses to receive a copy of the message. Only the sender can see this address or addresses.
- -v: Displays delivery details for the email message

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

To read your emails, simply type the mail command and press Enter. You'll see a list of email messages (if you have any) and their associated index number, and you'll receive a & prompt. At the prompt, type the number of the email you wish to read. An example is shown in Listing 7.25.

### Listing 7.25: Using mail to read an email message

```
$ whoami
rich
$
$ mail
Heirloom Mail version 12.5 7/5/10. Type ? for help.
"/var/spool/mail/rich": 1 message 1 new
>N 1 christine@localhost. Wed May 22 13:04 23/721 "LPIC-1 Book Progress"
& 1
Message 1:
[...]
From: christine@localhost.localdomain
Status: R

Hi Rich,
I'm working on Chapter 7 right now.
How's Chapter 8 coming along?
Best regards,
Christine

& q
Held 1 message in /var/spool/mail/rich
You have mail in /var/spool/mail/rich
$
```

If you wish to delete email messages, type d #, where # is the number of the email message you wish to delete. When you are done reading and/or deleting your email messages, type q at the mail prompt to quit the utility.

> If you don't have any email messages and you use the mail program to attempt to read a message, you'll receive the No email for username response.

If you have your email file stored in a nondefault location, you can tell the mail utility where to find it. Just tack on the -f DirectoryName/FileName option to the mail command.

If you have the correct permissions and need to read another user’s email messages, you can also use the -f option along with the appropriate argument to designate the user’s email file location. If the other user has their email file in a default location, you can use the -u username option and argument instead.

## Checking the Email Queue

Occasionally problems occur and your outbound mail cannot be sent. You can quickly see this problem arising by viewing the local mail queue. There are two commands to accomplish this task: mailq and sendmail -bp.

To see these commands in action, we created an email that cannot be sent out due to a phony recipient email address. This bad email address causes the message to get stuck in the local mail queue long enough for us to view it. The event is shown in Listing 7.26.

### Listing 7.26: Using mailq and sendmail -bp to view the local email queue

```
$ mail -s "Test of Mail Queue" bogususer@example.com
Testing mail queue
EOT
$
$ mailq
-Queue ID- --Size-- ----Arrival Time---- -Sender/Recipient--------
62D301CE55* 474 Wed May 22 14:03:20 christine@localhost.localdomain
bogususer@example.com
-- 0 Kbytes in 1 Request.
$
$ sendmail -bp
-Queue ID- --Size-- ----Arrival Time---- -Sender/Recipient--------
62D301CE55* 474 Wed May 22 14:03:20 christine@localhost.localdomain
bogususer@example.com
-- 0 Kbytes in 1 Request.
$
```

If there is nothing in the mail queue, when you enter the mailq or sendmail -bp command, you will receive the following response:

Mail queue is empty

The mail queue location(s) varies depending on which MTA you employ. It is typically located somewhere in the /var/spool/ directory tree. A quick way to locate the queue directory(ies) with the stuck mail files is to use super user privileges and the find command. Enter find /var/spool -name QueueID at the command line, where QueueID is the identification number listed under the Queue ID column in the mailq or sendmail -bp command output. After you have the directory location(s), you can employ the rm utility to delete it.

## Redirecting Email

An email alias allows you to redirect email messages to a different recipient. For example, on a corporate web server, instead of listing your email address (and at the same time letting every hacker in the world know your username) you can employ an alias, such as hostmaster. Via aliases, you configure email messages sent to hostmaster to go to your account instead.

Aliases are useful not only for security purposes but for common misspellings as well. If you have a difficult username, such as bresnahan, you can set up aliases for the prevalent incorrect spellings, such as bresan or brenanan. That way, you'll never miss an email.

While you do need to use super user privileges, there are only two steps to setting up an email alias:

- 1. Add the alias to the /etc/aliases file.
- 2. Run the newaliases command to update the aliases database, /etc/aliases.db.

> The /etc/aliases.db is a binary file. Thus, you want to edit the text-based /etc/aliases file with your new aliases and run the newaliases command to update the binary file.

The format of the alias records in the /etc/aliases file is

```
ALIAS-NAME: RECIPIENT1[,RECIPIENT2[,...]]
```

An example of setting up an alias for the hostmaster is shown in Listing 7.27. Prior to the modification, email for hostmaster was sent to the root account. After the /etc/aliases file modification and the newaliases command is run, email for hostmaster is now sent to two different accounts, christine and rich.

### Listing 7.27: Using /etc/aliases and newaliases to set up email aliases

```
# grep ^hostmaster /etc/aliases
hostmaster:         root
#
# nano /etc/aliases
#
# grep ^hostmaster /etc/aliases
hostmaster:         christine,rich
#
# newaliases
#
```

```
# mail -s "Test of Aliases" hostmaster
Testing the new hostmaster alias
EOT
#
# exit
[...]
$ whoami
christine
$
$ mail
[...]
>N 1 root               Wed May 22 15:10   18/656   "Test of Aliases"
& 1
Message 1:
[...]
To: hostmaster@localhost.localdomain
Subject: Test of Aliases
[...]
From: root@localhost.localdomain (root)
Status: R
Testing the new hostmaster alias
&
```

As shown in Listing 7.27, it's always a good idea to test out an alias modification. This is because many administrators forget to run the newaliases command after their modification to the /etc/aliases file.

Although aliases are useful for security and common misspellings, when a fellow team member is going to be gone on vacation for a few weeks, forwarding email is handy.

Setting up a forwarding email is done at the user level. It also involves only two steps:

- 1. The user creates the .forward file in their $HOME directory and puts in the username who should be receiving the forwarded emails.
- 2. The chmod command is used on the .forward file to set the permissions to 644 (octal). File permissions were covered in Chapter 4.

It's helpful to see an example of setting up a .forward file in order to forward email. One is provided in Listing 7.28.
