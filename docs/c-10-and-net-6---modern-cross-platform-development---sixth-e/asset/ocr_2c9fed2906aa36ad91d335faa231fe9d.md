```
.module extern libSystem.Native
.assembly extern System.Runtime
{
    .publickeytoken = (
        b0 3f 5f 7f 11 d5 0a 3a
    )
    .ver 6:0:0:0
}
```

.module extern libSystem.Native means this assembly makes function calls to Linux system APIs as you would expect from code that interacts with the filesystem. If we had decompiled the Windows equivalent of this assembly, it would use .module extern kernel32.dll instead, which is a Win32 API.

9. In EXPLORER, in ILS PY DECOMPILED MEMBERS, expand the assembly, expand the System.IO namespace, select Directory, and note the two edit windows that open showing the decompiled Directory class using C# code on the left and IL code on the right, as shown in Figure 7.7:

![](bbox=[0.17, 0.4, 0.93, 0.66])

Figure 7.7: The decompiled Directory class in C# and IL code

10. Compare the C# source code for the GetParent method, shown in the following code:

```
public static DirectoryInfo? GetParent(string path)
{
    if (path == null)
    {
        throw new ArgumentNullException("path");
    }
    if (path.Length == 0)
    {
        throw new ArgumentException(SR.Argument_PathEmpty, "path");
    }
    string fullPath = Path.GetFullPath(path);
```