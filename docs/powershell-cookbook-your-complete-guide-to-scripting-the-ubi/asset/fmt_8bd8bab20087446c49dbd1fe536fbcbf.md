
The Solution discusses three techniques to make useful customizations to your PowerShell environment: aliases, functions, and a hand-tailored prompt. You can (and will often) apply these techniques at any time during your PowerShell session, but your profile script is the standard place to put customizations that you want to apply to every session.

### Note

To remove an alias or function, use the `Remove-Item` cmdlet:

```
Remove-Item function:\MyCustomFunction
Remove-Item alias:\new
```

Although the `prompt` function returns a simple string, you can also use the function for more complex tasks. For example, many users update their console window title (by changing the `$host.UI.RawUI.WindowTitle` variable) or use the `Write-Host` cmdlet to output the prompt in color. If your prompt function handles the screen output itself, it still needs to return a string (for example, a single space) to prevent PowerShell from using its default. If you don't want this extra space to appear in your prompt, add an extra space at the end of your `Write-Host` command and return the backspace (`"`b"`) character, as shown in [Example 1-4](#ch01.xhtml#an_example_powershell_prompt).

### Example 1-4. An example PowerShell prompt

```
##############################################################################
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

Set-StrictMode -Version 3

function prompt
{
    $id = 1
    $historyItem = Get-History -Count 1
    if($historyItem)
    {
        $id = $historyItem.Id + 1
    }

    Write-Host -ForegroundColor DarkGray "`n[$(Get-Location)]"
    Write-Host -NoNewLine "PS:$id > "
    $host.UI.RawUI.WindowTitle = "$(Get-Location)"

    "`b"
}
```

In addition to showing the current location, this prompt also shows the ID for that command in your history. This lets you locate and invoke past commands with relative ease:

```
[C:\]
PS:73 >5 * 5
25

[C:\]
PS:74 >1 + 1
2

[C:\]
PS:75 >Invoke-History 73
5 * 5
25

[C:\]
PS:76 >
```

Although the profile referenced by `$profile` is the one you will almost always want to use, PowerShell actually supports four separate profile scripts. For further details on these scripts (along with other shell customization options), see [“Common Customization Points”](#app01.xhtml#common_customization_points).

## See Also

[Recipe 18.1, “Enable Scripting Through an Execution Policy”](#ch18.xhtml#enable_scripting_through_an_execution_po)

[“Common Customization Points”](#app01.xhtml#common_customization_points)

# 1.10 Customize PowerShell's User Input Behavior

## Problem

You want to override the way that PowerShell reads and handles input at the prompt.

## Solution

Use the `Set-PSReadLineOption` cmdlet to configure properties such as `EditMode` (Windows, VI, Emacs) and history management. For example, to make the continuation line for incomplete input a bit more red than usual:

```
Set-PSReadLineOption -Colors @{ ContinuationPrompt = "#663333" }
```

Use the `Set-PSReadLineKeyHandler` command to configure how PSReadLine responds to your actual keypresses. For example, to add forward and backward directory history navigation for `Alt+Comma` and `Alt+Period`:

```
Set-PSReadLineKeyHandler -Chord 'Alt+,' -ScriptBlock {
    Set-Location -
    [Microsoft.PowerShell.PSConsoleReadLine]::RevertLine()
    [Microsoft.PowerShell.PSConsoleReadLine]::AcceptLine()
}

Set-PSReadLineKeyHandler -Chord 'Alt+.' -ScriptBlock {
    Set-Location +
    [Microsoft.PowerShell.PSConsoleReadLine]::RevertLine()
    [Microsoft.PowerShell.PSConsoleReadLine]::AcceptLine()
}
```

## Discussion

When PowerShell first came on the scene, Unix folks were among the first to notice. They'd enjoyed a powerful shell and a vigorous heritage of automation for years—and “when I'm forced to use Windows, PowerShell rocks” is a phrase we've heard many times.

This natural uptake was no mistake. There are many on the team who come from a deep Unix background, and similarities to traditional Unix shells were intentional. For folks coming from other shells, though, we still hear the occasional grumble that some feature or another feels weird. `Alt+P` doesn't launch the built-in paging utility? `Ctrl+XX` doesn't move between the beginning of the line and current cursor position? Abhorrent!

In early versions of PowerShell, there was nothing you could reasonably do to address this. In those versions, PowerShell read its input from the console in what is known as `Cooked Mode`—where the Windows console subsystem handles all the keypresses, fancy `F7` menus, and more. When you press `Enter` or `Tab`, PowerShell gets the text of what you have typed so far, but that's it. There is no way for it to know that you had pressed the (Unix-like) `Ctrl+R`, `Ctrl+A`, `Ctrl+E`, or any other keys.

In later versions of PowerShell, most of these questions have gone away with the introduction of the fantastic `PSReadLine` module that PowerShell uses for command-line input. `PSReadLine` adds rich syntax highlighting, tab completion, history navigation, and more.

The `PSReadLine` module lets you configure it to an incredible degree. The `Set-PSReadLineOption` cmdlet supports options for its UI, input handling mode, history processing, and much more:

```
EditMode                       BellStyle
ContinuationPrompt             CompletionQueryItems
HistoryNoDuplicates            WordDelimiters
AddToHistoryHandler            HistorySearchCaseSensitive
CommandValidationHandler       HistorySaveStyle
HistorySearchCursorMovesToEnd  HistorySavePath
MaximumHistoryCount            AnsiEscapeTimeout
MaximumKillRingCount           PromptText
ShowToolTips                   ViModeIndicator
ExtraPromptLineCount           ViModeChangeHandler
DingTone                       PredictionSource
DingDuration                   Colors
```

In addition to letting you configure its runtime behavior, you can also configure how your keypresses cause it to react. To see all of the behaviors that you can map to key presses, run `Get-PSReadLineKeyHandler`. `PSReadLine` offers pages of options—many of them not currently assigned to any keypress:

```
PS > Get-PSReadLineKeyHandler

Basic editing functions
=======================

Key              Function           Description
---              --------           -----------
Enter            AcceptLine         Accept the input or move to the next line if
                                    input is missing a closing token.
Shift+Enter      AddLine            Move the cursor to the next line without
                                    attempting to execute the input
Backspace        BackwardDeleteChar Delete the character before the cursor
Ctrl+h           BackwardDeleteChar Delete the character before the cursor
Ctrl+Home        BackwardDeleteLine Delete text from the cursor to the start of
                                    the line
Ctrl+Backspace   BackwardKillWord   Move the text from the start of the current
                                    or previous word to the cursor to the kill
                                    ring
Ctrl+w           BackwardKillWord   Move the text from the start of the current
                                    or previous word to the cursor to...
(...)
```

To configure any of these functions, use the `Set-PSReadLineKeyHandler` command. For example, to set `Ctrl+Shift+C` to capture colorized regions of the buffer into your clipboard, run:

```
Set-PSReadLineKeyHandler -Chord Ctrl+Shift+C -Function CaptureScreen
```

If there isn't a pre-defined function to do what you want, you can use the `-ScriptBlock` parameter to have `PSReadLine` run any code of your choosing when you press a key or key combination. The example given by the Solution demonstrates this by adding forward and backward directory history navigation.
