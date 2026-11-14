
Fortunately, PowerShell makes scripting files and directories as easy as working at the command line—a point that many seasoned programmers and scripters often miss. A perfect example of this comes when you wrestle with limited disk space and need to find the files taking up the most space.

A typical programmer might approach this task by writing functions to scan a specific directory of a system. For each file, they check whether the file is big enough to care about. If so, they add it to a list. For each directory in the original directory, the programmer repeats this process (until there are no more directories to process).

As the saying goes, though, "You can write C in any programming language." The habits and preconceptions you bring to a language often directly influence how open you are to advances in that language.

Being an administrative shell, PowerShell directly supports tasks such as visiting all the files in a subdirectory or moving a file from one directory to another. That complicated programmer-oriented script turns into a one-liner:

```
Get-ChildItem -Recurse | Sort-Object -Descending Length | Select -First 10
```

Before diving into your favorite programmer's toolkit, check to see what PowerShell supports in that area. In many cases, it can handle the task without requiring your programmer's bag of tricks.

# 20.1 Determine and Change the Current Location

## Problem

You want to determine the current location from a script or command or change to a different directory.

## Solution

To retrieve the current location, use the `Get-Location` cmdlet. The `Get-Location` cmdlet provides the drive and path as two common properties:

```
$currentLocation = (Get-Location).Path
```

As a short form for `(Get-Location).Path`, use the `$pwd` automatic variable.

To change the current location, use the `Set-Location` cmdlet. For example, to change to your home directory:

```
Set-Location ~
```

## Discussion

### Getting your current location

The `Get-Location` cmdlet returns information about the current location. From the information it returns, you can access the current drive, provider, and path.

This current location affects PowerShell commands and programs that you launch from PowerShell. This doesn't apply when you interact with the .NET Framework, however. If you need to call a .NET method that interacts with the filesystem, always be sure to provide fully qualified paths:

```
[System.IO.File]::ReadAllText("c:\temp\file.txt")
```

If you're sure that the file exists, the `Resolve-Path` cmdlet lets you translate a relative path to an absolute path:

```
$filePath = (Resolve-Path file.txt).Path
[System.IO.File]::ReadAllText($filePath)
```

Or alternatively,

```
[System.IO.File]::ReadAllText("$pwd\file.txt")
```

If the file doesn't exist, use the `Join-Path` cmdlet in combination with the `Get-Location` cmdlet to specify the file:

```
$filePath = Join-Path (Get-Location) file.txt
```

Another alternative that combines the functionality of both approaches is a bit more advanced but also lets you specify relative locations. It comes from methods in the PowerShell `$executionContext` variable, which provides functionality normally used by cmdlet and provider authors:

```
$executionContext.SessionState.Path.`
    GetUnresolvedProviderPathFromPSPath("..\file.txt")
```

For more information about the `Get-Location` cmdlet, type `Get-Help Get-Location`.

### Setting your current location

The PowerShell command to change your current location is `Set-Location`, with an alias of `cd`. It supports three main modes of operation:

*   `Set-Location <path>`: Changes the current directory to the path specified. This can be an absolute path (like `c:\users`) or a relative path (like `..` or `..\..\system32`). See [Recipe 1.11](#ch01.xhtml#override_command_resolution) for a way to add support for further levels of parent directories, such as `Set-Location ....`. The `<path>` parameter supports several shortcuts: `~` (for your home directory), `$pshome` for the installation directory of PowerShell, or even complicated wildcards. For example, if you don't know which of the `Program Files` directories some software is installed to, you can still quickly navigate to it with: `cd c:\progra*\*steam*`.

*   `Set-Location -`: Changes the current directory to the last directory you visited, similar to Alt+Back in a web browser.

*   `Set-Location +`: Changes the current directory to the next directory in your directory history, similar to Alt+Forward in a web browser.

To add hot key support for navigating back and forth in your directory history, see [Recipe 1.10](#ch01.xhtml#customize_user_input).

## See Also

[Recipe 1.11, "Customize PowerShell's Command Resolution Behavior"](#ch01.xhtml#override_command_resolution)

[Recipe 1.10, "Customize PowerShell's User Input Behavior"](#ch01.xhtml#customize_user_input)

# 20.2 Get the Files in a Directory

## Problem

You want to get or list the files in a directory.

## Solution

To retrieve the list of files in a directory, use the `Get-ChildItem` cmdlet. To get a specific item, use the `Get-Item` cmdlet:

*   To list all items in the current directory, use the `Get-ChildItem` cmdlet:

    ```
    Get-ChildItem
    ```

*   To list all items that match a wildcard, supply a wildcard to the `Get-ChildItem` cmdlet:

    ```
    Get-ChildItem *.txt
    ```

*   To list all files that match a wildcard in the current directory (and all its children), use the `-Recurse` parameter of the `Get-ChildItem` cmdlet:

    ```
    Get-ChildItem *.txt -Recurse
    ```

*   To list all directories in the current directory, use the `-Attributes` parameter:

    ```
    Get-ChildItem -Attributes Directory
    dir -ad
    ```

*   To get information about a specific item, use the `Get-Item` cmdlet:

    ```
    Get-Item test.txt
    ```

## Discussion

Although most commonly used on the filesystem, the `Get-ChildItem` and `Get-Item` cmdlets in fact work against any items on any of the PowerShell drives. In addition to `A:` through `Z:` (the standard filesystem drives), they also work on `Alias:`, `Cert:`, `Env:`, `Function:`, `HKLM:`, `HKCU:`, `and Variable:`.

While the Solution demonstrates some simple wildcard scenarios that the `Get-ChildItem` cmdlet supports, PowerShell in fact enables several more advanced scenarios. For more information about these scenarios, see [Recipe 20.6](#ch20.xhtml#find_files_that_match_a_pattern). One specific point of interest is that the third example in the Solution lists files that match a wildcard in a directory and all its children. That example works on any PowerShell provider. However, PowerShell can retrieve your results more quickly if you use a provider-specific filter, as described in [Recipe 20.6](#ch20.xhtml#find_files_that_match_a_pattern).

When you're working in the filesystem, the `Get-ChildItem` cmdlet exposes several parameters (`-Directory`, `-File`, `-ReadOnly`, `-Hidden`, and `-System`) to make filtering as simple as possible. These parameters have aliases as well (for example, `-ad`), making short work of most common tasks:

```
Get-ChildItem -Directory
dir -ad
```

For less common attributes, the `-Attributes` parameter supports powerful filtering against all other file and directory properties. At its most basic level, you can supply any standard file attribute. PowerShell only returns files with that attribute set:

```
Get-ChildItem -Attributes Compressed
```

To return items that do not have an attribute set (a "not" scenario), use an exclamation point:

```
Get-ChildItem -Attributes !Archive
```

To return items that have *any* of several attributes set (an "or" scenario), use a comma:

```
Get-ChildItem -Attributes "Hidden, ReadOnly"
```

To return items that have *all* of several attributes set (an "and" scenario), use a plus:

```
Get-ChildItem -Attributes "ReadOnly + Hidden"
```
