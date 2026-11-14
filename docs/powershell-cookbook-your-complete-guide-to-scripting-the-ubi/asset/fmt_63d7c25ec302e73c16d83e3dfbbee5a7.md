
# 2.2 Filter Items in a List or Command Output

## Problem

You want to filter the items in a list or command output.

## Solution

Use the `Where-Object` cmdlet to select items in a list (or command output) that match a condition you provide. The `Where-Object` cmdlet has the standard aliases `where` and `?`.

To list all running processes that have "search" in their name, use the `-like` operator to compare against the process's `Name` property:

```
Get-Process | Where-Object { $_.Name -like "*Search*" }
```

To list all stopped services, use the `-eq` operator to compare against the service's `Status` property:

```
Get-Service | Where-Object { $_.Status -eq "Stopped" }
```

To list all processes not responding, test the `Responding` property:

```
Get-Process | Where-Object { -not $_.Responding }
```

For simple comparisons on properties, you can omit the script block syntax and use the comparison parameters of `Where-Object` directly:

```
Get-Process | Where-Object Name -like "*Search*"
```

## Discussion

For each item in its input (which is the output of the previous command), the `Where-Object` cmdlet evaluates that input against the script block that you specify. If the script block returns `True`, then the `Where-Object` cmdlet passes the object along. Otherwise, it does not. A script block is a series of PowerShell commands enclosed by the `{` and `}` characters. You can write any PowerShell commands inside the script block. In the script block, the `$_` (or `$PSItem`) variable represents the current input object. For each item in the incoming set of objects, PowerShell assigns that item to the `$_` (or `$PSItem`) variable and then runs your script block. In the preceding examples, this incoming object represents the process, file, or service that the previous cmdlet generated.

This script block can contain a great deal of functionality, if desired. It can combine multiple tests, comparisons, and much more. For more information about script blocks, see [Recipe 11.4](#ch11.xhtml#write_a_script_block). For more information about the type of comparisons available to you, see ["Comparison Operators"](#app01.xhtml#comparison_operators).

For simple filtering, the syntax of using script blocks in the `Where-Object` cmdlet may sometimes seem overbearing. For these scenarios, `Where-Object` offers parameters that directly support parameters to apply simple comparisons like `-Eq`, `-Match`, `-In`, and more.

In addition to the script block syntax offered by the `Where-Object` cmdlet, PowerShell also offers a version built into the language itself: the `where()` method. This is slightly faster for very large data collections, although the time it takes to collect those items (such as getting the list of files in a directory) normally dwarfs any time it takes to filter them. The `where()` method does offer several additional useful modes, however, through its second parameter.

### Get the first part of a list

```
PS > (1..10).where( { $_ -eq 5 }, "Until" )
1
2
3
4
```

### Get the second part of a list

```
PS > (1..10).where( { $_ -eq 5 }, "SkipUntil" )
5
6
7
8
9
10
```

### Split a list

```
PS > $even,$odd = (1..10).where( { $_ % 2 -eq 0 }, "Split" )
PS > $even -join ","
2,4,6,8,10
PS > $odd -join ","
1,3,5,7,9
```

For complex filtering (for example, the type you would normally rely on a mouse to do with files in an Explorer window), writing the script block to express your intent may be difficult or even infeasible. If this is the case, [Recipe 2.4](#ch02.xhtml#program_interactively_filter_lists_of_ob) shows a script that can make manual filtering easier to accomplish.

For more information about the `Where-Object` cmdlet, type `Get-Help Where-Object`. For more information about the `where()` method, type `Get-Help about_Arrays`.

## See Also

[Recipe 2.4, "Interactively Filter Lists of Objects"](#ch02.xhtml#program_interactively_filter_lists_of_ob)

[Recipe 11.4, "Write a Script Block"](#ch11.xhtml#write_a_script_block)

["Comparison Operators"](#app01.xhtml#comparison_operators)

# 2.3 Group and Pivot Data by Name

## Problem

You want to easily access items in a list by a property name.

## Solution

Use the `Group-Object` cmdlet (which has the standard alias `group`) with the `-AsHash` and `-AsString` parameters. This creates a hashtable with the selected property (or expression) used as keys in that hashtable:

```
PS > $h = dir | group -AsHash -AsString Length
PS > $h

Name                           Value
----                           -----
746                            {ReplaceTest.ps1}
499                            {Format-String.ps1}
20494                          {test.dll}

PS > $h["499"]

    Directory: C:\temp


Mode                LastWriteTime     Length Name
----                -------------     ------ ----
-a---        10/18/2009   9:57 PM        499 Format-String.ps1


PS > $h["746"]

    Directory: C:\temp

Mode                LastWriteTime     Length Name
----                -------------     ------ ----
-a---        10/18/2009   9:51 PM        746 ReplaceTest.ps1
```

## Discussion

In some situations, you might find yourself repeatedly calling the `Where-Object` cmdlet to interact with the same list or output:

```
PS > $processes = Get-Process
PS > $processes | Where-Object { $_.Id -eq 1216 }

Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
     62       3     1012       3132    50     0.20   1216 dwm


PS > $processes | Where-Object { $_.Id -eq 212 }

Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
    614      10    28444       5484   117     1.27    212 SearchIndexer
```

In these situations, you can instead use the `-AsHash` parameter of the `Group-Object` cmdlet. When you use this parameter, PowerShell creates a hashtable to hold your results. This creates a map between the property you're interested in and the object it represents:

```
PS > $processes = Get-Process | Group-Object -AsHash Id
PS > $processes[1216]

Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
     62       3     1012       3132    50     0.20   1216 dwm

PS > $processes[212]

Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
    610      10    28444       5488   117     1.27    212 SearchIndexer
```

For simple types of data, this approach works well. Depending on your data, though, using the `-AsHash` parameter alone can create difficulties.

The first issue you might run into arises when the value of a property is `$null`. Hashtables in PowerShell (and the .NET Framework that provides the underlying support) don't support `$null` as a value, so you get a misleading error message:

```
PS > "Hello",(Get-Process -id $pid) | Group-Object -AsHash Id
Group-Object : The objects grouped by this property cannot be expanded
since there is a duplication of the key. Please give a valid property and try
again.
```

A second issue crops up when more complex data gets stored within the hashtable. This can unfortunately be true even of data that *appears* to be simple:

```
PS > $result = dir | Group-Object -AsHash Length
PS > $result

Name                           Value
----                           -----
746                            {ReplaceTest.ps1}
499                            {Format-String.ps1}
20494                          {test.dll}

PS > $result[746]
(Nothing appears)
```
