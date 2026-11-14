
# 14.1 `Set-StrictMode` Cmdlet

`Set-StrictMode` cmdlet 是*有作用域的*，这意味着在一个脚本或函数中设置的严格模式不会影响调用它的脚本或函数。要暂时禁用脚本某个区域的严格模式，请在新的脚本块中执行：

```
& { Set-StrictMode -Off; $tsetVariable }
```

为了你的脚本调试健康和理智着想，严格模式应该是你添加到 PowerShell 配置文件中的首批内容之一。

## 参见

[配方 1.9，“自定义 Shell、配置文件和提示符”](#ch01.xhtml#customize_your_shell_profile_and_prompt)

# 14.2 为脚本编写单元测试

## 问题

你想要编写测试，以自动验证脚本的行为。

## 解决方案

安装 `Pester` 模块：

```
Install-Module Pester -Scope CurrentUser
```

创建一个文件名中包含 `.tests.ps1` 的文件，其中包含一个 `Describe` 语句和至少一个 `It` 语句：

```
BeforeAll {
    Import-Module PowerShellCookbook
}

## Test the Use-Culture command
Describe 'Use Culture tests' {
    It 'Testing on English' {
        $result = Use-Culture en-US { "quit".ToUpper() }
        $result | Should -Be "QUIT"
    }

    It 'Testing on Turkish' {
        $result = Use-Culture tr-TR { "quit".ToUpper() }
        $result | Should -Be "QUİT"
    }
}
```

在该文件所在的目录中，运行 `Invoke-Pester`。

## 讨论

编写脚本时，最棒的感觉莫过于修复了一个 Bug。但最糟糕的感觉是，当你意识到你的“简单 Bug 修复”却引入了两个新 Bug。

作为脚本编写者，我们试图在发布新版本前手动验证主要场景来防止这种情况。对于特别复杂的更改，也许我们会花额外时间手动验证一些边缘情况。但由于手动测试耗时，我们倾向于避免它。

如果能让自动化为你完成所有这些手动测试呢？而且不仅是简单的测试——而是你曾经考虑过的所有困难和具有挑战性的边缘情况？事实证明，这完全可能——这被称为*单元测试*，也是任何主要软件开发项目保持高质量的关键。既然你的目标是 PowerShell 脚本（理想情况下本应是自动化的），那么单元测试比在大多数其他领域都容易得多。

PowerShell 的单元测试框架是一个令人惊叹的社区项目，称为 `Pester`。事实上，PowerShell 本身的大多数单元测试都是作为 `Pester` 测试编写的。虽然它在 Windows 上默认安装，但最好从 PowerShell 库获取最新版本。

当你运行 `Pester` 时，它会查找你在该目录中编写的所有脚本并运行它们。如果你引入了错误，你将看到类似如下的输出：

```
PS > Invoke-Pester

Starting discovery in 1 files.
Discovery finished in 171ms.
[-] Use Culture tests.Testing on Turkish 76ms (75ms|2ms)
 Expected strings to be the same, but they were different.
 String lengths are both 4.
 Strings differ at index 2.
 Expected: 'QUIT'
 But was:  'QUİT'
 at $result | Should -Be "QUIT", ...\UseCulture.tests.ps1:14
 at <ScriptBlock>, ...UseCulture.tests.ps1:14
Tests completed in 1.75s
Tests Passed: 1, Failed: 1, Skipped: 0 NotRun: 0
```

一旦你修复了根本问题，你将看到软件工程领域中最令人满意的 UI 之一（[图 14-1](#ch14.xhtml#pester_tests_passing)）。

![](Images/wps4_1401.png)

## 参见

[配方 1.29，“查找并安装额外的 PowerShell 脚本和模块”](#ch01.xhtml#install_module_gallery)

# 14.3 跟踪脚本执行

## 问题

你想要查看 PowerShell 运行脚本时所采取的执行流程。

## 解决方案

使用 `Set-PsDebug` cmdlet 的 `-Trace` 参数，让 PowerShell 在执行脚本时进行跟踪：

```
PS > function BuggyFunction
{
    $testVariable = "Hello"
    if($testVariab1e -eq "Hello")
    {
        "Should get here"
    }
    else
    {
        "Should not get here"
    }
}

PS > Set-PsDebug -Trace 1
PS > BuggyFunction
DEBUG:    1+  <<<< BuggyFunction
DEBUG:    3+     $testVariable = <<<<  "Hello"
DEBUG:    4+     if <<<< ($testVariab1e -eq "Hello")
DEBUG:   10+         "Should not get here" <<<<
Should not get here
```

## 讨论

就简单的交互式调试而言（与 Bug 预防相对），PowerShell 支持几个你可能习惯的最有用的调试功能。要获得完整体验，Visual Studio Code 提供了成熟的图形化调试器。有关在 Visual Studio Code 中调试的更多信息，请参阅[配方 19.1](#ch19.xhtml#debug_script_ise)。

不过，从命令行你仍然可以访问跟踪（通过 `Set-PsDebug -Trace` 语句）、单步执行（通过 `Set-PsDebug -Step` 语句）和环境检查（通过 `$host.EnterNestedPrompt()` 调用）。`*-PsBreakpoint` cmdlet 除了这些基本功能外还支持更多功能，但 `Set-PsDebug` cmdlet 对于一些简单问题很有用。

作为这些技术的演示，请参阅[示例 14-2](#ch14.xhtml#a_complex_script_that_interacts_with_pow)。

### 示例 14-2. 与 PowerShell 调试功能交互的复杂脚本

```
#############################################################################
##
## Invoke-ComplexScript
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

演示 PowerShell 调试支持的功能。

#>

Set-StrictMode -Version 3

Write-Host "Calculating lots of complex information"

$runningTotal = 0
$runningTotal += [Math]::Pow(5 * 5 + 10, 2)

Write-Debug "Current value: $runningTotal"

Set-PsDebug -Trace 1
$dirCount = @(Get-ChildItem $env:WINDIR).Count

Set-PsDebug -Trace 2
$runningTotal -= 10
$runningTotal /= 2

Set-PsDebug -Step
$runningTotal *= 3
$runningTotal /= 2

$host.EnterNestedPrompt()

Set-PsDebug -off
```

当你试图弄清楚为什么脚本没有按预期工作时，调试会话可能如[示例 14-3](#ch14.xhtml#debugging_a_complex_script)所示。

### 示例 14-3. 调试复杂脚本

```
PS > $debugPreference = "Continue"
PS > Invoke-ComplexScript.ps1
Calculating lots of complex information
DEBUG: Current value: 1225
DEBUG:   17+ $dirCount = @(Get-ChildItem $env:WINDIR).Count
DEBUG:   17+ $dirCount = @(Get-ChildItem $env:WINDIR).Count
DEBUG:   19+ Set-PsDebug -Trace 2
DEBUG:   20+ $runningTotal -= 10
DEBUG:     ! SET $runningTotal = '1215'.
DEBUG:   21+ $runningTotal /= 2
DEBUG:     ! SET $runningTotal = '607.5'.
DEBUG:   23+ Set-PsDebug -Step

Continue with this operation?
  24+ $runningTotal *= 3
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend [?] Help
(default is "Y"):y
DEBUG:   24+ $runningTotal *= 3
DEBUG:    !  SET $runningTotal = '1822.5'.

Continue with this operation?
  25+ $runningTotal /= 2
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend [?] Help
(default is "Y"):y
DEBUG:   25+ $runningTotal /= 2
DEBUG:    !  SET $runningTotal = '911.25'.

Continue with this operation?
  27+ $host.EnterNestedPrompt()
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend [?] Help
(default is "Y"):y
DEBUG:   27+ $host.EnterNestedPrompt()
DEBUG:     ! CALL method 'System.Void EnterNestedPrompt()'
PS > $dirCount
296
PS > $dirCount + $runningTotal
1207.25
PS > exit

Continue with this operation?
  29+ Set-PsDebug -off
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend [?] Help
(default is "Y"):y
DEBUG:   29+ Set-PsDebug -off
```

总而言之，这些交互式调试功能一定能帮助你快速诊断并解决简单问题。对于更复杂的问题，PowerShell 的图形化调试器（在 Visual Studio Code 中）和 `*-PsBreakpoint` cmdlet 将提供帮助。
