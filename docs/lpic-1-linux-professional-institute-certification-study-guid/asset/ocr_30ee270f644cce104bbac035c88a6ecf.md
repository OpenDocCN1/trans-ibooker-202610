# # Configuring User Accounts

Adding and modifying user account credentials, which includes usernames, account information, and passwords, is an important (but tedious) part of system administration. In addition, you need to know how to delete these credentials, when warranted.

To add a new user account on the system, the `useradd` utility is typically used. However, the process involves players besides the `useradd` command. A depiction of the process is illustrated in Figure 7.1.

# **FIGURE 7.1** The process of adding a user account

![](bbox=[0.235, 0.292, 0.775, 0.522])

You can see in Figure 7.1 that several team players are involved in the account creation process. Notice that the `/etc/skel` directory is bolded. This is because, depending on the other configuration files, it may not be used in the process. The same goes for the `/home/userid` directory. It may not be created or it may have an alternative name, depending on the system's account creation configuration. You'll learn more about these directories shortly.

Before we jump into the `useradd` utility details, let's look at the two files and the directory involved in the creation side of the process.

# ## The /etc/login.defs File

The `/etc/login.defs` configuration file is typically installed by default on most Linux distributions. It contains directives for use in various shadow password suite commands. Shadow password suite is an umbrella term for commands dealing with account credentials, such as the `useradd`, `userdel`, and `passwd` commands.

The directives in this configuration file control password length, how long until the user is required to change the account's password, whether or not a home directory is created by default, and so on. The file is typically filled with comments and commented-out directives (which make the directives inactive). Listing 7.1 shows only the active directives within the `/etc/login.defs` file, after stripping out blank and comment lines on a CentOS distribution.