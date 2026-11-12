```
Conflicts=rescue.service rescue.target
After=multi-user.target rescue.service rescue.target display-manager.service
AllowIsolate=yes
$
```

Notice in Listing 5.13 that the graphical.target unit file has many of the same directives as a service unit file. These directives were described back in Table 5.6. Of course, these directives apply to a target type unit file instead of a service type unit file. For example, the After directive in the graphical.target unit file sets this target unit to start after the designated units, such as multi-user.target. Target units, similar to service units, have various target dependency chains as well as conflicts.

In Listing 5.13, there is one directive we have not covered yet. The AllowIsolate directive, if set to yes, permits this target file to be used with the systemctl isolate command. This command is covered later in this chapter.

# Real World Scenario

# Modifying systemd Configuration Files

Occasionally you may need to change a unit configuration file for your Linux system's requirements or add additional components. However, be careful when doing this task. You should not modify any unit files in the /lib/systemd/system/ or /usr/lib/systemd/system/ directory.

To modify a unit configuration file, copy the file to the /etc/systemd/system/ directory and modify it there. This modified file will take precedence over the original unit file left in the original directory. Also, it will protect the modified unit file from software updates.

If you just have a few additional components, you can extend the configuration. Using super user privileges, create a new subdirectory in the /etc/systemd/system/ directory named service.service-name.d, where service-name is the service's name. For example, for the openSSH daemon, you would create the /etc/systemd/system/service.sshd.d directory. This newly created directory is called a drop-in file directory, because you can drop in additional configuration files. Create any configuration files with names like description.conf, where description describes the configuration file's purpose, such as local or script. Add your modified directives to this configuration file.

After you make these modifications, you must complete a few more steps. Find and compare any unit file that overrides another unit file by issuing the systemd-delta command. It will display any unit files that are duplicated, extended, redirected, and so on. Review this list. It will help you avoid any unintended consequences from modifying or extending a service unit file.

To have your changes take effect, issue the systemctl daemon-reload command for the service whose unit file you modified or extended. After you accomplish that task, issue the systemctl restart command to start or restart the service. These commands are explained in the next section.