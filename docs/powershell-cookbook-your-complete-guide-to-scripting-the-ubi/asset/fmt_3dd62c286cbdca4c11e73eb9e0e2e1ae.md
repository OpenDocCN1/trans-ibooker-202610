
## Send-TcpRequest

From *PowerShell Cookbook* (O'Reilly) by Lee Holmes (<http://www.leeholmes.com/guide>)

### SYNOPSIS

Send a TCP request to a remote computer, and return the response. If you do not supply input to this script (via either the pipeline, or the `-InputObject` parameter), the script operates in interactive mode.

### EXAMPLE

```powershell
$http = @"
  GET / HTTP/1.1
  Host:bing.com
  `n`n
"@

$http | Send-TcpRequest bing.com 80
```

### Parameters

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

### Script Body

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

For more information about running scripts, see [Recipe 1.2](#ch01.xhtml#run_programs_scripts_and_existing_tools).

## See Also

- [Recipe 1.2, “Run Programs, Scripts, and Existing Tools”](#ch01.xhtml#run_programs_scripts_and_existing_tools)
- [Recipe 5.15, “Convert Text Streams to Objects”](#ch05.xhtml#convert_text_streams_to_objects)

# Chapter 13. User Interaction

## 13.0 Introduction

Although most scripts are designed to run automatically, you’ll frequently find it useful to have your scripts interact with the user.

> **Note**
>
> The best way to get input from your user is through the arguments and parameters to your script or function. This lets your users run your script without having to be there as it runs!

If your script greatly benefits from (or requires) an interactive experience, PowerShell offers a range of possibilities. This might be simply waiting for a keypress, prompting for input, or displaying a richer choice-based prompt.

User input isn’t the only aspect of interaction, though. In addition to its input facilities, PowerShell supports output as well—from displaying simple text strings to much more detailed progress reporting and interaction with UI frameworks.

## 13.1 Read a Line of User Input

### Problem

You want to use input from the user in your script.

### Solution

To obtain user input, use the `Read-Host` cmdlet:

```powershell
PS > $directory = Read-Host "Enter a directory name"
Enter a directory name: C:\MyDirectory
PS > $directory
C:\MyDirectory
```

### Discussion

The `Read-Host` cmdlet reads a single line of input from the user. If the input contains sensitive data, the cmdlet supports an `-AsSecureString` parameter to read this input as a `SecureString`.

If the user input represents a date, time, or number, be aware that most cultures represent these data types differently. For more information about writing culture-aware scripts, see [Recipe 13.6](#ch13.xhtml#write_culture-aware_scripts).

For more information about the `Read-Host` cmdlet, type `Get-Help Read-Host`. For an example of reading user input through a graphical prompt, see the `Read-InputBox` script included in this book’s code examples. For more information about obtaining these examples, see [“Using Code Examples”](#preface01.xhtml#code_examples).

### See Also
