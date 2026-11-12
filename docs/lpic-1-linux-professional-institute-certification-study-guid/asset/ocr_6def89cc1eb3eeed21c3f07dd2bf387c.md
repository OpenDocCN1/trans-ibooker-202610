spool -name QueueID at the command line, where QueueID is the identification number listed under the Queue ID column in the mailq or sendmail -bp command output. After you have the directory location(s), you can employ the rm utility to delete it.

# Redirecting Email

An email alias allows you to redirect email messages to a different recipient. For example, on a corporate web server, instead of listing your email address (and at the same time letting every hacker in the world know your username) you can employ an alias, such as hostmaster. Via aliases, you configure email messages sent to hostmaster to go to your account instead.

Aliases are useful not only for security purposes but for common misspellings as well. If you have a difficult username, such as bresnahan, you can set up aliases for the prevalent incorrect spellings, such as bresan or brenanan. That way, you'll never miss an email.

While you do need to use super user privileges, there are only two steps to setting up an email alias:

+   1. Add the alias to the /etc/aliases file.
+   2. Run the newaliases command to update the aliases database, /etc/aliases.db.

> The /etc/aliases.db is a binary file. Thus, you want to edit the text-based /etc/aliases file with your new aliases and run the newaliases command to update the binary file.

The format of the alias records in the /etc/aliases file is

```
ALIAS-NAME: RECIPIENT1[,RECIPIENT2[,...]]
```

An example of setting up an alias for the hostmaster is shown snipped in Listing 7.27. Prior to the modification, email for hostmaster was sent to the root account. After the /etc/aliases file modification and the newaliases command is run, email for hostmaster is now sent to two different accounts, christine and rich.

# Listing 7.27: Using /etc/aliases and -newaliases to set up email aliases

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