
## Send-TcpRequest

源自 *PowerShell Cookbook*（O'Reilly），作者 Lee Holmes (<http://www.leeholmes.com/guide>)

### 概要

向远程计算机发送 TCP 请求，并返回响应。如果不向此脚本提供输入（通过管道或 `-InputObject` 参数），则脚本将在交互模式下运行。

### 示例

```powershell
$http = @"
  GET / HTTP/1.1
  Host:bing.com
  `n`n
"@

$http | Send-TcpRequest bing.com 80
```

### 参数

```powershell
[CmdletBinding()]
param(
    ## The computer to connect to
    [Parameter()]
    [string] $ComputerName = "localhost",

    ## A switch to determine if you just want to test the connection
    [Parameter()]
    [switch] $Test,

    ## The port to use
    [Parameter()]
    [int] $Port = 80,

    ## A switch to determine if the connection should be made using SSL
    [Parameter()]
    [switch] $UseSSL,

    ## The input string to send to the remote host
    [Parameter(ValueFromPipeline)]
    [string] $InputObject,

    ## The delay, in milliseconds, to wait between commands
    [Parameter()]
    [int] $Delay = 100
)
```

### 脚本主体

```powershell
Set-StrictMode -Version 3

[string] $SCRIPT:output = ""

## Store the input into an array that we can scan over. If there was no input,
## then we will be in interactive mode.
$currentInput = $InputObject
if (-not $currentInput) {
    $currentInput = @($input)
}
$scriptedMode = ([bool] $currentInput) -or $Test

function Main {
    ## Open the socket, and connect to the computer on the specified port
    if (-not $scriptedMode) {
        Write-Host "Connecting to $ComputerName on port $Port"
    }

    try {
        $tcpClient = New-Object Net.Sockets.TcpClient($ComputerName, $Port)
    }
    catch {
        if ($Test) { $false }
        else { Write-Error "Could not connect to remote computer: $_" }
        return
    }

    ## If we're just testing the connection, we've made the connection
    ## successfully, so just return $true
    if ($Test) { $true; return }

    ## If this is interactive mode, supply the prompt
    if (-not $scriptedMode) {
        Write-Host "Connected.  Press ^D followed by [ENTER] to exit.`n"
    }

    $stream = $tcpClient.GetStream()

    ## If we wanted to use SSL, set up that portion of the connection
    if ($UseSSL) {
        try {
            $sslStream = New-Object System.Net.Security.SslStream $stream, $false
            $sslStream.AuthenticateAsClient($ComputerName)
            $stream = $sslStream
        }
        catch [System.IO.IOException] {
            ## Try again with explicit SSL (TLS)
            $tcpClient = New-Object System.Net.Sockets.TcpClient($ComputerName, $Port)
            $stream = $tcpClient.GetStream()

            $writer = New-Object System.IO.StreamWriter $stream

            $writer.WriteLine("EHLO")
            $writer.Flush()

            $writer.WriteLine("STARTTLS")
            $writer.Flush()
            $null = GetOutput

            $sslStream = New-Object System.Net.Security.SslStream $stream, $false
            $sslStream.AuthenticateAsClient($ComputerName)
            $stream = $sslStream
        }
    }

    $writer = New-Object System.IO.StreamWriter $stream

    while ($true) {
        ## Receive the output that has buffered so far
        $SCRIPT:output += GetOutput

        ## If we're in scripted mode, send the commands,
        ## receive the output, and exit.
        if ($scriptedMode) {
            foreach ($line in $currentInput) {
                $writer.WriteLine($line)
                $writer.Flush()
                Start-Sleep -m $Delay
                $SCRIPT:output += GetOutput
            }
            break
        }
        ## If we're in interactive mode, write the buffered
        ## output, and respond to input.
        else {
            if ($output) {
                foreach ($line in $output.Split("`n")) {
                    Write-Host $line
                }
                $SCRIPT:output = ""
            }

            ## Read the user's command, quitting if they hit ^D
            $command = Read-Host
            if ($command -eq ([char] 4)) { break; }

            ## Otherwise, write their command to the remote host
            $writer.WriteLine($command)
            $writer.Flush()
        }
    }

    ## Close the streams
    $writer.Close()
    $stream.Close()

    ## If we're in scripted mode, return the output
    if ($scriptedMode) {
        $output
    }
}

## Read output from a remote host
function GetOutput {
    ## Create a buffer to receive the response
    $buffer = New-Object System.Byte[] 1024
    $encoding = New-Object System.Text.AsciiEncoding

    $outputBuffer = ""
    $foundMore = $false

    ## Read all the data available from the stream, writing it to the
    ## output buffer when done.
    do {
        ## Allow data to buffer for a bit
        Start-Sleep -m 1000

        ## Read what data is available
        $foundMore = $false
        $stream.ReadTimeout = 1000

        do {
            try {
                $read = $stream.Read($buffer, 0, 1024)

                if ($read -gt 0) {
                    $foundMore = $true
                    $outputBuffer += ($encoding.GetString($buffer, 0, $read))
                }
            } catch { $foundMore = $false; $read = 0 }
        } while ($read -gt 0)
    } while ($foundMore)

    $outputBuffer
}

. Main
```

有关运行脚本的更多信息，请参阅 [Recipe 1.2](#ch01.xhtml#run_programs_scripts_and_existing_tools)。

## 另请参阅

- [Recipe 1.2，“运行程序、脚本和现有工具”](#ch01.xhtml#run_programs_scripts_and_existing_tools)
- [Recipe 5.15，“将文本流转换为对象”](#ch05.xhtml#convert_text_streams_to_objects)

# 第 13 章. 用户交互

## 13.0 简介

虽然大多数脚本设计为自动运行，但您经常会发现让脚本与用户交互很有用。

> **注意**
>
> 从用户获取输入的最佳方式是通过脚本或函数的参数。这允许用户在脚本运行时无需亲自在场即可运行脚本！

如果您的脚本极大地受益于（或需要）交互式体验，PowerShell 提供了多种可能性。这可能只是等待按键、提示输入，或显示更丰富的基于选择的提示。

不过，用户输入并不是交互的唯一方面。除了输入功能外，PowerShell 还支持输出——从显示简单的文本字符串到更详细的进度报告和与 UI 框架的交互。

## 13.1 读取一行用户输入

### 问题

您希望在脚本中使用来自用户的输入。

### 解决方案

要获取用户输入，请使用 `Read-Host` cmdlet：

```powershell
PS > $directory = Read-Host "Enter a directory name"
Enter a directory name: C:\MyDirectory
PS > $directory
C:\MyDirectory
```

### 讨论

`Read-Host` cmdlet 从用户读取单行输入。如果输入包含敏感数据，该 cmdlet 支持 `-AsSecureString` 参数以将此输入作为 `SecureString` 读取。

如果用户输入表示日期、时间或数字，请注意大多数区域性对这些数据类型的表示方式不同。有关编写区分区域性脚本的更多信息，请参阅 [Recipe 13.6](#ch13.xhtml#write_culture-aware_scripts)。

有关 `Read-Host` cmdlet 的更多信息，请键入 `Get-Help Read-Host`。有关通过图形提示读取用户输入的示例，请参阅本书代码示例中包含的 `Read-InputBox` 脚本。有关获取这些示例的更多信息，请参阅 [“使用代码示例”](#preface01.xhtml#code_examples)。

### 另请参阅
