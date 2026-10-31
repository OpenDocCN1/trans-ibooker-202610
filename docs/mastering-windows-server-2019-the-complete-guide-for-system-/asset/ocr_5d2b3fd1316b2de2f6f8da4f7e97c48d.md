# Central Store

When opening up the Group Policy Management Console and creating or editing a GPO, the settings available within your console session are settings pulled from ADMX/ADML files that are on the hard drive of the computer or server from which you are using GPMC. When implementing new settings via ADMX files, it would be a huge chore to have to copy those new files into place on every one of your domain controllers, in addition to all of the client computers where you might have the RSAT tools installed. Thankfully, there is a solution to automate this for you!

The Central Store is something that can be enabled in Active Directory that allows the replication of ADMX/ADML files. Once you enable the Central Store, all of your Group Policy management machines, such as domain controllers, will look to the store as its repository for these template files.

# Enable the Central Store

All that it takes to enable the Central Store in Active Directory is the creation of two folders inside a special folder called SYSVOL. Once created, GPMC will check this location on subsequent launches, and pull in any new ADMX/ADML files that have been added. Log into a domain controller server, and create the following new folder:

```
%systemroot%\SYSVOL\sysvol\contoso.local\Policies\PolicyDefinitions
```

Create an additional folder within your new PolicyDefinitions folder to contain language-specific ADML files. In my case, I need one for English such as the following:

```
%systemroot%\SYSVOL\sysvol\contoso.local\Policies\PolicyDefinitions\en-US
```

Obviously you'll want to replace contoso.local with your own internal domain name, and you're finished! As soon as you have created these two new folders, utilize this location from this point forward to plug in new ADMX and ADML files, and those new settings will show up from wherever you launch GPMC in the future.