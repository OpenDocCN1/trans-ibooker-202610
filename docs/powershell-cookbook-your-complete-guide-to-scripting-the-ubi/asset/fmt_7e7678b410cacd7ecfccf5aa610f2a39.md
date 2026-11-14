
## 1.24 Program: Search Formatted Output for a Pattern

If you type a number, tab completion finds the entry in your command history with that ID, and then replaces your command line with the text of that history entry. This is especially useful when you want to slightly modify a previous history entry, since `Invoke-History` by itself doesn't support that.

### Pattern completion

If you type anything else, tab completion searches for entries in your command history that contain that text. Under the hood, PowerShell uses the `-like` operator to match your command entries, so you can use all of the wildcard characters supported by that operator. For more information on searching text for patterns, see [Recipe 5.7](#ch05.xhtml#search_a_string_for_text_or_a_pattern).

PowerShell's tab completion is largely driven by the fully customizable `TabExpansion2` function. You can easily change this function to include more advanced functionality, or even just customize specific behaviors to suit your personal preferences. For more information, see [Recipe 1.18](#ch01.xhtml#customize_tab_expansion).

### See Also

- [Recipe 1.9, "Customize Your Shell, Profile, and Prompt"](#ch01.xhtml#customize_your_shell_profile_and_prompt)
- [Recipe 1.18, "Understand and Customize PowerShell's Tab Completion"](#ch01.xhtml#customize_tab_expansion)
- [Recipe 1.31, "Save State Between Sessions"](#ch01.xhtml#save-state-exiting)
- [Recipe 5.7, "Search a String for Text or a Pattern"](#ch05.xhtml#search_a_string_for_text_or_a_pattern)

While PowerShell's built-in filtering facilities are incredibly flexible (for example, the `Where-Object` cmdlet), they generally operate against specific properties of the incoming object. If you are searching for text in the object's formatted output, or don't know which property contains the text you are looking for, simple text-based filtering is sometimes helpful.

To solve this problem, you can pipe the output into the `Out-String` cmdlet before passing it to the `Select-String` cmdlet:

```
Get-Service | Out-String -Stream | Select-String audio
```

Or, using built-in aliases:

```
Get-Service | oss | sls audio
```

In script form, `Select-TextOutput` (shown in [Example 1-17](#ch01.xhtml#select-textoutput.ps1)) does exactly this, and it lets you search for a pattern in the visual representation of command output.

#### Example 1-17. Select-TextOutput.ps1

```
##############################################################################
##
## Select-TextOutput
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

Searches the textual output of a command for a pattern.

.EXAMPLE

PS > Get-Service | Select-TextOutput audio
Finds all references to "Audio" in the output of Get-Service

#>

param(
    ## The pattern to search for
    $Pattern
)

Set-StrictMode -Version 3
$input | Out-String -Stream | Select-String $pattern
```

For more information about running scripts, see [Recipe 1.2](#ch01.xhtml#run_programs_scripts_and_existing_tools).

### See Also

- [Recipe 1.2, "Run Programs, Scripts, and Existing Tools"](#ch01.xhtml#run_programs_scripts_and_existing_tools)

## 1.25 Interactively View and Process Command Output

### Problem

You want to graphically explore and analyze the output of a command.

### Solution

Use the `Out-GridView` cmdlet to interactively explore the output of a command.

### Discussion

The `Out-GridView` cmdlet is one of the rare PowerShell cmdlets that displays a graphical user interface. While the `Where-Object` and `Sort-Object` cmdlets are the most common way to sort and filter lists of items, the `Out-GridView` cmdlet is very effective at the style of repeated refinement that sometimes helps you develop complex queries. [Figure 1-4](#ch01.xhtml#out_gridview_1) shows the `Out-GridView` cmdlet in action.

![](Images/wps4_0104.png)

`Out-GridView` lets you primarily filter your command output in two ways: a *quick filter* expression and a *criteria filter*.

*Quick filters* are fairly simple. As you type text in the topmost "Filter" window, `Out-GridView` filters the list to contain only items that match that text. If you want to restrict this text filtering to specific columns, simply provide a column name before your search string and separate the two with a colon. You can provide multiple search strings, in which case `Out-GridView` returns only rows that match all of the required strings.

##### Note

Unlike most filtering cmdlets in PowerShell, the quick filters in the `Out-GridView` cmdlet do not support wildcards or regular expressions. For this type of advanced query, criteria-based filtering can help.

*Criteria filters* give fine-grained control over the filtering used by the `Out-GridView` cmdlet. To apply a criteria filter, click the "Add criteria" button and select a property to filter on. `Out-GridView` adds a row below the quick filter field and lets you pick one of several operations to apply to this property:

- Less than or equal to
- Greater than or equal to
- Between
- Equals
- Does not equal
- Contains
- Does not contain

In addition to these filtering options, `Out-GridView` also lets you click and rearrange the header columns to sort by them.

### Processing output

Once you've sliced and diced your command output, you can select any rows you want to keep and press `Ctrl+C` to copy them to the clipboard. `Out-GridView` copies the items to the clipboard as tab-separated data, so you can easily paste the information into a spreadsheet or other file for further processing.

In addition to supporting clipboard output, the `Out-GridView` cmdlet supports full-fidelity object filtering if you use its `-PassThru` parameter. For an example of this full-fidelity filtering, see [Recipe 2.4](#ch02.xhtml#program_interactively_filter_lists_of_ob).

### See Also

- [Recipe 2.4, "Interactively Filter Lists of Objects"](#ch02.xhtml#program_interactively_filter_lists_of_ob)

## 1.26 Program: Interactively View and Explore Objects

When working with unfamiliar objects in PowerShell, much of your time is spent with the `Get-Member` and `Format-List` commands—navigating through properties, reviewing members, and more.

For ad hoc investigation, a graphical interface is often useful.

To solve this problem, [Example 1-18](#ch01.xhtml#show-object-ps1) provides an interactive tree view that you can use to explore and navigate objects. For example, to examine the structure of a script as PowerShell sees it (its *abstract syntax tree*):

```
$ps = { Get-Process -ID $pid }.Ast
Show-Object $ps
```

For more information about parsing and analyzing the structure of PowerShell scripts, see [Recipe 10.10](#ch10.xhtml#parse_and_interpret_powershell_scripts).

#### Example 1-18. Show-Object.ps1
