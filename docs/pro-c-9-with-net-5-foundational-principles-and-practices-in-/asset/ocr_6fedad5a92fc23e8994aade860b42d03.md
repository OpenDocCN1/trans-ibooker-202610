```
public string[] Verbs { get; }
public ProcessWindowStyle WindowStyle { get; set; }
public string WorkingDirectory { get; set; }
}
```

To illustrate how to fine-tune your process startup, here is a modified version of StartAndKillProcess(), which will load Microsoft Edge and navigate to www.facebook.com, using the windows association MsEdge:

```
static void StartAndKillProcess()
{
    Process proc = null;

    // Launch Microsoft Edge, and go to Facebook, with maximized window.
    try
    {
        ProcessStartInfo startInfo = new
            ProcessStartInfo("MsEdge", "www.facebook.com");
        startInfo.UseShellExecute = true;
        proc = Process.Start(startInfo);
    }
    catch (InvalidOperationException ex)
    {
        Console.WriteLine(ex.Message);
    }
    ...
}
```

In .NET Core, the UseShellExecute property defaults to false, while in prior versions of .NET, the UseShellExecute property defaults to true. This is the reason that the previous version of Process.Start(), shown here, no longer works without using ProcessStartInfo and setting the UseShellExecute property to true:

```
Process.Start("msedge")
```

# Leveraging OS Verbs with ProcessStartInfo

In addition to using the OS shortcuts to launch applications, you can also take advantage of file associations with ProcessStartInfo. On Windows, if you right-click a Word document, there are options to edit or print the document. Let’s use the ProcessStartInfo to determine the verbs available and then use them to manipulate the process.

Create a new method with the following code:

```
static void UseApplicationVerbs()
{
    int i = 0;
    //adjust this path and name to a document on your machine
    ProcessStartInfo si =
        new ProcessStartInfo("@..\TestPage.docx");
    foreach (var verb in si.Verbs)
    {
        Console.WriteLine($" {i++}. {verb}");
    }
}
```