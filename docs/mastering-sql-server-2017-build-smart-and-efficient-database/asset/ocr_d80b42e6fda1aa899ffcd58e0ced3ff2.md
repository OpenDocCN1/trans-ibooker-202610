```
private const String FtpRemotePath_MISSING_MESSAGE = "FtpRemotePath has not been set.";
private const String REMOVE_ENABLED_MESSAGE = "FtpRemove is set to TRUE, which means that the file is going to be removed from the source.";
private const String SESSION_OPEN_MESSAGE = "Session opened succesfully.";
private const String REMOTE_DIRECTORY_MISSING_MESSAGE_PATTERN = "The specified remote [{0}] directory is missing.\r\nIt will be created.";
private const String REMOTE_DIRECTORY_CREATED_MESSAGE_PATTERN = "The specified remote [{0}] directory has been created.";
private const String REMOTE_FILES_MISSING_MESSAGE_PATTERN = "The specified remote file(s) [{0}] cannot be found.";
private const String EXCEPTION_MESSAGE_PATTERN = "An error has occurred:\r\n\r\n{0}";
private const String UNKNOWN_EXCEPTION_MESSAGE = "(No other information available.)";
```

These constants are going to be used by the custom task's methods to convey information about the state of the task to the SSIS package developers when configuring the task.

18. Create the following public members of the SecureFTP class:

```
public String FtpProtocolName { get; set; }
public String FtpHostName { get; set; }
public Int32 FtpPortNumber { get; set; }
public String FtpUserName { get; set; }
public String FtpPassword { get; set; }
public String FtpSshHostKeyFingerprint { get; set; }
public String FtpOperationName { get; set; }
public String FtpLocalPath { get; set; }
public String FtpRemotePath { get; set; }
public Boolean FtpRemove { get; set; }
```

These public members of the SecureFTP class will be accessible in the SSDT as task properties, and will allow the SSIS package developers to configure the custom task; in SSDT, the values can be supplied as literal values or by using expressions.