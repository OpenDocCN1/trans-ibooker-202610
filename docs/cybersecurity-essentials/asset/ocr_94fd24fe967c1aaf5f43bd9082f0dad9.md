# Protect

Use a good encrypted protocol suite such as IPSec, and create the credentials for each user adhering to strict good password concepts. Your options may be dictated by your firewall or appliance, but you should be able to find a scheme that all of your users and their devices can support.

Your policies should include some expiration of credentials generally in months not years. Ideally you assign cryptic passwords to each user, but many organizations fight this because users cannot remember cryptic passwords and many don’t use password management software. If you must use less cryptic passwords, use a mixture of common words plus a short number that the employee can remember, such as Jumpbranch92 or better yet Jump$Branch92. I tend to put the capital letters at the beginning or end for ease of typing.

If a user has access to many assets and therefore presents a higher security risk, then those policies should be tighter, with cryptic passwords more frequently changed. If a remote user will always come from a fixed IP, include that in their access profile to add another layer of protection.

Be sure to enable as much logging as you can so that you have access to robust reports of user activity.

If users will access shared files remotely, try to find a way to limit or eliminate the remote user’s ability to delete files or even write files if that is possible. Read only access is ideal. If the remote user must access confidential information stored in internal databases, make sure the database content is encrypted. Much of this depends upon the operating system of the server and the database technology, but look for these solutions as they will minimize your risk.

While security through obscurity is a weak concept in general, be smart when you create hostnames for remote employee access. vpn.yourcompany.com is terribly obvious. It could also be worth registering a separate domain for this kind of thing that doesn’t give away the company name.

Simply creating access schemes where the user only has access to the resources they need and not the entire network is always wise. The best way to reduce your risk is to manage the worst-case scenarios and minimize what can possibly be accessed remotely. If sensitive information must be accessed remotely, then you must inconvenience those users a bit more with extra password challenges or force them to access from a fixed IP address.

# Detect

With good credentials and everything locked as tightly as you can on a fully hardened network, your main risk is mostly limited to an unauthorized user