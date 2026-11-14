
# PowerShell Loop Statements

## for Statement

When PowerShell executes a `for` statement, it first executes the expression given by `initialization`. It next evaluates `condition`. If `condition` evaluates to `$true`, PowerShell executes the given statement block. It then executes the expression given by `increment`. PowerShell continues to execute the statement block and `increment` statement as long as `condition` evaluates to `$true`.

For example:

```
for($counter = 0; $counter -lt 10; $counter++)
{
    Write-Host "Processing item $counter"
}
```

The `break` and `continue` statements (discussed later in this appendix) can specify the `loop_label` of any enclosing looping statement as their target.

## foreach Statement

```
:loop_label foreach(variable in expression)
{
   statement block
}
```

When PowerShell executes a `foreach` statement, it executes the pipeline given by `expression`—for example, `Get-Process | Where-Object {$_.Handles -gt 500}` or `1..10`. For each item produced by the expression, it assigns that item to the variable specified by `variable` and then executes the given statement block. For example:

```
$handleSum = 0
foreach($process in Get-Process |
    Where-Object { $_.Handles -gt 500 })
{
   $handleSum += $process.Handles
}
$handleSum
```

In addition to the `foreach` statement, you can also use the `foreach` method on collections directly:

```
$handleSum = 0
(Get-Process).foreach( { $handleSum += $_.Handles } )
```

The `break` and `continue` statements (discussed later in this appendix) can specify the `loop_label` of any enclosing looping statement as their target. In addition to the `foreach` statement, PowerShell also offers the `ForEach-Object` cmdlet with similar capabilities. For more information, see [Recipe 4.4](#ch04.xhtml#repeat_operations_with_loops).

## while Statement

```
:loop_label while(condition)
{
   statement block
}
```

When PowerShell executes a `while` statement, it first evaluates the expression given by `condition`. If this expression evaluates to `$true`, PowerShell executes the given statement block. PowerShell continues to execute the statement block as long as `condition` evaluates to `$true`. For example:

```
$command = "";
while($command -notmatch "quit")
{
   $command = Read-Host "Enter your command"
}
```

The `break` and `continue` statements (discussed later in this appendix) can specify the `loop_label` of any enclosing looping statement as their target.

## do … while Statement / do … until Statement

```
:loop_label do
{
   statement block
} while(condition)
```

or

```
:loop_label do
{
   statement block
} until(condition)
```

When PowerShell executes a `do … while` or `do … until` statement, it first executes the given statement block. In a `do … while` statement, PowerShell continues to execute the statement block as long as `condition` evaluates to `$true`. In a `do … until` statement, PowerShell continues to execute the statement as long as `condition` evaluates to `$false`. For example:

```
$validResponses = "Yes","No"
$response = ""
do
{
   $response = Read-Host "Yes or No?"
} while($validResponses -notcontains $response)
"Got it."

$response = ""
do
{
   $response = Read-Host "Yes or No?"
} until($validResponses -contains $response)
"Got it."
```

The `break` and `continue` statements (discussed later in this appendix) can specify the `loop_label` of any enclosing looping statement as their target.

## Flow Control Statements

PowerShell supports two statements to help you control flow within loops: `break` and `continue`.

### break

The `break` statement halts execution of the current loop. PowerShell then resumes execution at the end of the current looping statement, as though the looping statement had completed naturally. For example:

```
for($counter = 0; $counter -lt 5; $counter++)
{
    for($counter2 = 0; $counter2 -lt 5; $counter2++)
    {
        if($counter2 -eq 2)
        {
            break
        }

        Write-Host "Processing item $counter,$counter2"
    }
}
```

produces the output (notice the second column never reaches the value `2`):

```
Processing item 0,0
Processing item 0,1
Processing item 1,0
Processing item 1,1
Processing item 2,0
Processing item 2,1
Processing item 3,0
Processing item 3,1
Processing item 4,0
Processing item 4,1
```

If you specify a label with the `break` statement—for example, `break outer_loop`—PowerShell halts the execution of that loop instead. For example:

```
:outer_loop for($counter = 0; $counter -lt 5; $counter++)
{
    for($counter2 = 0; $counter2 -lt 5; $counter2++)
    {
        if($counter2 -eq 2)
        {
            break outer_loop
        }

        Write-Host "Processing item $counter,$counter2"
    }
}
```

produces the output:

```
Processing item 0,0
Processing item 0,1
```

### continue

The `continue` statement skips execution of the rest of the current statement block. PowerShell then continues with the next iteration of the current looping statement, as though the statement block had completed naturally. For example:

```
for($counter = 0; $counter -lt 5; $counter++)
{
    for($counter2 = 0; $counter2 -lt 5; $counter2++)
    {
        if($counter2 -eq 2)
        {
            continue
        }

        Write-Host "Processing item $counter,$counter2"
    }
}
```

produces the output:

```
Processing item 0,0
Processing item 0,1
Processing item 0,3
Processing item 0,4
Processing item 1,0
Processing item 1,1
Processing item 1,3
Processing item 1,4
Processing item 2,0
Processing item 2,1
Processing item 2,3
Processing item 2,4
Processing item 3,0
Processing item 3,1
Processing item 3,3
Processing item 3,4
Processing item 4,0
Processing item 4,1
Processing item 4,3
Processing item 4,4
```

If you specify a label with the `continue` statement—for example, `continue outer_loop`—PowerShell continues with the next iteration of that loop instead.

For example:

```
:outer_loop for($counter = 0; $counter -lt 5; $counter++)
{
    for($counter2 = 0; $counter2 -lt 5; $counter2++)
    {
        if($counter2 -eq 2)
        {
            continue outer_loop
        }

        Write-Host "Processing item $counter,$counter2"
    }
}
```

produces the output:

```
Processing item 0,0
Processing item 0,1
Processing item 1,0
Processing item 1,1
Processing item 2,0
Processing item 2,1
Processing item 3,0
Processing item 3,1
Processing item 4,0
Processing item 4,1
```

## Classes

```
## A class called "Example" that inherits from "BaseClass"
## and implements the "ImplementedInterface" interface
class Example : BaseClass, ImplementedInterface
{
    ## Default constructor, which also invokes the constructor
    ## from the base class.
    Example() : base()
    {
        [Example]::lastInstantiated = Get-Date
    }

    ## Constructor with parameters
    Example([string] $Name)
    {
        $this.Name = $Name
        [Example]::lastInstantiated = Get-Date
    }

    ## A publicly visible property with validation attributes
    [ValidateLength(2,20)]
    [string] $Name

    ## A property that is hidden from default views
    static hidden [DateTime] $lastInstantiated

    ## A publicly visible method that returns a value
    [string] ToString()
    {
        ## Return statement is required. Implicit / pipeline output
        ## is not treated as output like it is with functions.
        return $this.ToString( [Int32]::MaxValue )
    }

    ## A publicly visible method that returns a value
    [string] ToString([int] $MaxLength)
    {
        $output = "Name = $($this.Name);" +
            "LastInstantiated = $([Example]::lastInstantiated)"
        $outputLength = [Math]::Min($MaxLength, $output.Length)
        return $output.Substring(0, $outputLength)
    }

}
```

### Base classes and interfaces
