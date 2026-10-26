```
```csharp
// Verify the connection.
using (Session winScpSession = this.EstablishSession())
{
    componentEvents.FireInformation(0, TASK_NAME,
SESSION_OPEN_MESSAGE, String.Empty, 0, ref fireAgain);

    // Verify the remote resources.
    OperationMode operation =
(OperationMode)Enum.Parse(typeof(OperationMode),
this.FtpOperationName);
    switch (operation)
    {
        case OperationMode.PutFiles:
            Boolean remoteDirectoryExists =
winScpSession.FileExists(this.FtpRemotePath);
            if (!remoteDirectoryExists)
            {
                componentEvents.FireInformation(0, TASK_NAME,
String.Format(REMOTE_DIRECTORY_MISSING_MESSAGE_PATTERN,
this.FtpRemotePath), String.Empty, 0, ref fireAgain);
            }
            break;
        case OperationMode.GetFiles:
        default:
            Boolean remoteFileExists =
winScpSession.FileExists(this.FtpRemotePath);
            if (!remoteFileExists)
            {
                componentEvents.FireInformation(0, TASK_NAME,
String.Format(REMOTE_FILES_MISSING_MESSAGE_PATTERN,
this.FtpRemotePath), String.Empty, 0, ref fireAgain);
            }
            break;
    }
}

return DTSExecResult.Success;
}
catch (Exception exc)
{
    String exceptionMessage = exc != null ? exc.Message :
UNKNOWN_EXCEPTION_MESSAGE;
    componentEvents.FireError(0, TASK_NAME,
String.Format(EXCEPTION_MESSAGE_PATTERN, exceptionMessage),
String.Empty, 0);
    return DTSExecResult.Failure;
}
```
```