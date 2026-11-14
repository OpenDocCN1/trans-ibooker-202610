
# Invoke-SqlCommand

```powershell
##############################################################################
##
## Invoke-SqlCommand
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

Return the results of a SQL query or operation

.EXAMPLE

Invoke-SqlCommand.ps1 -Sql "SELECT TOP 10 * FROM Orders"
Invokes a command using Windows authentication

.EXAMPLE

PS > $cred = Get-Credential
PS > Invoke-SqlCommand.ps1 -Sql "SELECT TOP 10 * FROM Orders" -Cred $cred
Invokes a command using SQL Authentication

.EXAMPLE

PS > $server = "MYSERVER"
PS > $database = "Master"
PS > $sql = "UPDATE Orders SET EmployeeID = 6 WHERE OrderID = 10248"
PS > Invoke-SqlCommand $server $database $sql
Invokes a command that performs an update

.EXAMPLE

PS > $sql = "EXEC SalesByCategory 'Beverages'"
PS > Invoke-SqlCommand -Sql $sql
Invokes a stored procedure

.EXAMPLE

PS > Invoke-SqlCommand (Resolve-Path access_test.mdb) -Sql "SELECT * FROM Users"
Access an Access database

.EXAMPLE

PS > Invoke-SqlCommand (Resolve-Path xls_test.xls) -Sql 'SELECT * FROM [Sheet1$]'
Access an Excel file

#>

param(
    ## The data source to use in the connection
    [string] $DataSource = ".\SQLEXPRESS",

    ## The database within the data source
    [string] $Database = "Northwind",

    ## The SQL statement(s) to invoke against the database
    [Parameter(Mandatory = $true)]
    [string[]] $SqlCommand,

    ## The timeout, in seconds, to wait for the query to complete
    [int] $Timeout = 60,

    ## The credential to use in the connection, if any.
    $Credential
)


Set-StrictMode -Version 3

## Prepare the authentication information. By default, we pick
## Windows authentication
$authentication = "Integrated Security=SSPI;"

## If the user supplies a credential, then they want SQL
## authentication
if($credential)
{
    $credential = Get-Credential $credential
    $plainCred = $credential.GetNetworkCredential()
    $authentication =
        ("uid={0};pwd={1};" -f $plainCred.Username,$plainCred.Password)
}

## Prepare the connection string out of the information they
## provide
$connectionString = "Provider=sqloledb; " +
                    "Data Source=$dataSource; " +
                    "Initial Catalog=$database; " +
                    "$authentication; "

## If they specify an Access database or Excel file as the connection
## source, modify the connection string to connect to that data source
if($dataSource -match '\.xls$|\.mdb$')
{
    $connectionString = "Provider=Microsoft.Jet.OLEDB.4.0; " +
        "Data Source=$dataSource; "

    if($dataSource -match '\.xls$')
    {
        $connectionString += 'Extended Properties="Excel 8.0;"; '

        ## Generate an error if they didn't specify the sheet name properly
        if($sqlCommand -notmatch '\[.+\$\]')
        {
            $error = 'Sheet names should be surrounded by square brackets, ' +
                'and have a dollar sign at the end: [Sheet1$]'
            Write-Error $error
            return
        }
    }
}

## Connect to the data source and open it
$connection = New-Object System.Data.OleDb.OleDbConnection $connectionString
$connection.Open()

foreach($commandString in $sqlCommand)
{
    $command = New-Object Data.OleDb.OleDbCommand $commandString,$connection
    $command.CommandTimeout = $timeout

    ## Fetch the results, and close the connection
    $adapter = New-Object System.Data.OleDb.OleDbDataAdapter $command
    $dataset = New-Object System.Data.DataSet
    [void] $adapter.Fill($dataSet)

    ## Return all of the rows from their query
    $dataSet.Tables | Select-Object -Expand Rows
}

$connection.Close()
```

有关运行脚本的详细信息，请参阅[食谱 1.2](#ch01.xhtml#run_programs_scripts_and_existing_tools)。

## 另请参阅

[食谱 1.2，“运行程序、脚本和现有工具”](#ch01.xhtml#run_programs_scripts_and_existing_tools)

# 17.3 访问 Windows 性能计数器

## 问题

你想要从 PowerShell 访问系统性能计数器信息。

## 解决方案

若要检索特定性能计数器的信息，请使用 `Get-Counter` cmdlet，如[示例 17-2](#ch17.xhtml#accessing_performance_counter_data_throu)所示。

### 示例 17-2. 通过 Get-Counter cmdlet 访问性能计数器数据

```
PS > $counter = Get-Counter "\System\System Up Time"
PS > $uptime = $counter.CounterSamples[0].CookedValue
PS > New-TimeSpan -Seconds $uptime


Days              : 8
Hours             : 1
Minutes           : 38
Seconds           : 58
Milliseconds      : 0
Ticks             : 6971380000000
TotalDays         : 8.06872685185185
TotalHours        : 193.649444444444
TotalMinutes      : 11618.9666666667
TotalSeconds      : 697138
TotalMilliseconds : 697138000
```

或者，WMI 的 `Win32_Perf*` 类集支持许多最常见的性能计数器：

```
Get-CimInstance Win32_PerfFormattedData_Tcpip_NetworkInterface
```

## 讨论

`Get-Counter` cmdlet 为所有 Windows 性能计数器提供了便捷的访问。如果不带参数，它会汇总系统活动：

```
PS > Get-Counter -Continuous

Timestamp                 CounterSamples
---------                 --------------
1/9/2010 7:26:49 PM       \\...\network interface(ethernet
                          adapter)\bytes total/sec :
                          102739.3921377

                          \\...\processor(_total)\% processor
                          time :
                          35.6164383561644

                          \\...\memory\% committed bytes in use
                          :
                          29.4531607006855

                          \\...\memory\cache faults/sec :
                          98.1952324093294

                          \\...\physicaldisk(_total)\% disk time
                           :
                          144.227945205479

                          \\...\physicaldisk(_total)\current disk
                          queue length :
                          0
(...)
```

当你提供特定计数器的路径时，`Get-Counter` cmdlet 仅检索该路径的样本。如果需要，`-Computer` 参数允许你以特定远程计算机为目标：

```
PS > $computer = $ENV:Computername
PS > Get-Counter -Computer $computer "processor(_total)\% processor time"

Timestamp                 CounterSamples
---------                 --------------
1/9/2010 7:31:58 PM       \\...\processor(_total)\% processor time :
                          15.8710351576814
```

如果你不知道所需性能计数器的路径，可以使用 `-ListSet` 参数来搜索计数器或计数器集。要查看所有计数器集，请使用 `*` 作为参数值：

```
PS > Get-Counter -List * | Format-List CounterSetName,Description


CounterSetName : TBS counters
Description    : Performance counters for the TPM Base Services component.

CounterSetName : WSMan Quota Statistics
Description    : Displays quota usage and violation information for WS-
                 Management processes.

CounterSetName : Netlogon
Description    : Counters for measuring the performance of Netlogon.

(...)
```

如果你想查找特定计数器，请使用 `Where-Object` cmdlet 根据 `Description` 或 `Paths` 属性进行比较：
