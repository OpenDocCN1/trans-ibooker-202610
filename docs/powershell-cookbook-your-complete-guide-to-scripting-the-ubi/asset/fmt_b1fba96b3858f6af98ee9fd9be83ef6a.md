
# ConvertFrom-String Template Syntax

In a template, the left curly brace `{` represents the start of your selection, and the right curly brace `}` represents the end of your selection. To name your selection, you provide a property name and a colon right after the opening brace. So, `PowerShell Rocks` becomes `{FName:PowerShell} {LName:Rocks}`.

Let's start creating a template. In a new file, start with this as an example, and save it as `addressbook.template.txt` (the name is up to you):

```
{Record:Record
------

FName: Some
LName: Body}

Last updated: {LastUpdated:05/09/2021}
```

When you run `ConvertFrom-String` on this input and template, we get:

```
PS > $book = Get-Content addressbook.txt |
    ConvertFrom-String -TemplateFile addressbook.template.txt
PS > $book.LastUpdated
05/09/2021

PS > $book.Record

Record
------

FName: Lee
LName: Holmes
```

There were several records, though. To tell `ConvertFrom-String` that the input contained multiple of a certain pattern, use an asterisk after the property name:

```
{Record*:Record
------

FName: Some
LName: Body}

Last updated: {LastUpdated:05/09/2021}
```

If we run this, we see that `ConvertFrom-String` hasn't quite figured out the record format. So let's give it another example:

```
{Record*:Record
------

FName: Some
LName: Body}

{Record*:Record
------

FName: Adam
LName: Smith}

Last updated: {LastUpdated:05/09/2021}
```

And now, `ConvertFrom-String` understands records and a footer:

```
PS > (Get-Content addressbook.txt |
    ConvertFrom-String -TemplateFile addressbook.template.txt)

Record
------
Record...
Record...
Record...

PS > (Get-Content addressbook.txt |
    ConvertFrom-String -TemplateFile addressbook.template.txt).LastUpdated

05/09/2021
```

To tell `ConvertFrom-String` about the inner structure of a record, we simply tag it and name it as well. Update the first record in your template:

```
(...)
FName: {FName:Some}
LName: {LName:Body}}
(...)
```

And now `ConvertFrom-String` fully understands our database format.

```
PS > (Get-Content addressbook.txt |
    ConvertFrom-String -TemplateFile addressbook.template.txt)

Record
------
{@{FName=Lee; LName=Holmes}}
{@{FName=Adam; LName=Smith}}
{@{FName=Some; LName=Body}}

PS > (Get-Content addressbook.txt |
    ConvertFrom-String -TemplateFile addressbook.template.txt).Record[0].FName
Lee
```

As our final magic trick, let's tell PowerShell that `LastUpdate` is a `[DateTime]`. Update your template to include:

```
(...)
Last updated: {[DateTime] LastUpdated:05/09/2021}
(...)
```

Which gives an amazing result:

```
PS > (Get-Content addressbook.txt |
    ConvertFrom-String -TemplateFile addressbook.template.txt).LastUpdated

Sunday, May 9, 2021 12:00:00 AM
```

## See Also

- [Recipe 1.2, "Run Programs, Scripts, and Existing Tools"](#ch01.xhtml#run_programs_scripts_and_existing_tools)
- [Recipe 5.14, "Convert a String Between One Format and Another"](#ch05.xhtml#convert_string_between_formats)

# 5.16 Generate Large Reports and Text Streams

## Problem

You want to write a script that generates a large report or large amount of data.

## Solution

The best approach to generating a large amount of data is to take advantage of PowerShell's streaming behavior whenever possible. Opt for solutions that pipeline data between commands:

```
Get-ChildItem C:\*.txt -Recurse | Out-File c:\temp\AllTextFiles.txt
```

rather than collect the output at each stage:

```
$files = Get-ChildItem C:\*.txt -Recurse
$files | Out-File c:\temp\AllTextFiles.txt
```

If your script generates a large text report (and streaming is not an option), use the `StringBuilder` class:

```
$output = New-Object System.Text.StringBuilder
Get-ChildItem C:\*.txt -Recurse |
    ForEach-Object { [void] $output.AppendLine($_.FullName) }
$output.ToString()
```

rather than simple text concatenation:

```
$output = ""
Get-ChildItem C:\*.txt -Recurse | ForEach-Object { $output += $_.FullName }
$output
```

## Discussion

In PowerShell, combining commands in a pipeline is a fundamental concept. As scripts and cmdlets generate output, PowerShell passes that output to the next command in the pipeline as soon as it can. In the Solution, the `Get-ChildItem` commands that retrieve all text files on the *C:* drive take a very long time to complete. However, since they *begin* to generate data almost immediately, PowerShell can pass that data on to the next command as soon as the `Get-ChildItem` cmdlet produces it. This is true of any commands that generate or consume data and is called *streaming*. The pipeline completes almost as soon as the `Get-ChildItem` cmdlet finishes producing its data and uses memory very efficiently as it does so.

The second `Get-ChildItem` example (which collects its data) prevents PowerShell from taking advantage of this streaming opportunity. It first stores all the files in an array, which, because of the amount of data, takes a long time and an enormous amount of memory. Then, it sends all those objects into the output file, which takes a long time as well.

However, most commands can consume data produced by the pipeline directly, as illustrated by the `Out-File` cmdlet. For those commands, PowerShell provides streaming behavior as long as you combine the commands into a pipeline. For commands that do not support data coming from the pipeline directly, the `ForEach-Object` cmdlet (with the aliases of `foreach` and `%`) lets you work with each piece of data as the previous command produces it, as shown in the `StringBuilder` example.

### Creating large text reports

When you generate large reports, it's common to store the entire report into a string, and then write that string out to a file once the script completes. You can usually accomplish this most effectively by streaming the text directly to its destination (a file or the screen), but sometimes this isn't possible.

Since PowerShell makes it so easy to add more text to the end of a string (as in `$output += $_.FullName`), many initially opt for that approach. This works great for small-to-medium strings, but it causes significant performance problems for large strings.

> **Note**
>
> As an example of this performance difference, compare the following:
>
> ```
> PS > Measure-Command {
>    $output = New-Object Text.StringBuilder
>    1..10000 |
>        ForEach-Object { $output.Append("Hello World") }
> }
>
> (...)
> TotalSeconds : 2.3471592
>
> PS > Measure-Command {
>    $output = ""
>    1..10000 | ForEach-Object { $output += "Hello World" }
> }
>
> (...)
> TotalSeconds      : 4.9884882
> ```
>
> In the .NET Framework (and therefore PowerShell), strings never change after you create them. When you add more text to the end of a string, PowerShell has to build a *new* string by combining the two smaller strings. This operation takes a long time for large strings, which is why the .NET Framework includes the `System.Text.StringBuilder` class. Unlike normal strings, the `StringBuilder` class assumes that you will modify its data—an assumption that allows it to adapt to change much more efficiently.

# 5.17 Generate Source Code and Other Repetitive Text

## Problem

You want to simplify the creation of large amounts of repetitive source code or other text.

## Solution

Use PowerShell's string formatting operator (`-f`) to place dynamic information inside of a preformatted string, and then repeat that replacement for each piece of dynamic information.

## Discussion

Code generation is a useful technique in nearly any technology that produces output from some text-based input. For example, imagine having to create an HTML report to show all of the processes running on your system at that time. In this case, "code" is the HTML code understood by a web browser.

HTML pages start with some standard text (`<html>`, `<head>`, `<body>`), and then you would likely include the processes in an HTML `<table>`. Each row would include columns for each of the properties in the process you're working with.
