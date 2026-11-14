
# Clipboard Commands

One benefit to the `Get-Clipboard` and `Set-Clipboard` commands is that they automatically adapt their behavior to integrate with the operating system platform you are running PowerShell on. On Linux, PowerShell uses `xclip` if available. On macOS, PowerShell uses `pbcopy`. A great way to use the `Get-Clipboard` and `Set-Clipboard` cmdlets is to help with repetitive ad hoc content manipulation. For example, you can copy some content from a document into the clipboard, and then run a bit of PowerShell to get the content, change it, and replace the clipboard content with the new value. Then, you just paste the modified content back into the document. For more information about replacing text in strings, see [Recipe 5.8](#ch05.xhtml#replace_text_in_a_string).

### See Also

[Recipe 5.8, “Replace Text in a String”](#ch05.xhtml#replace_text_in_a_string)

# 8.4 Generate a Random Number or Object

## Problem

You want to generate a random number or pick a random element from a set of objects.

## Solution

Call the `Get-Random` cmdlet to generate a random positive integer:

```
Get-Random
```

Use the `-Minimum` and `-Maximum` parameters to generate a number between `Minimum` and up to (but not including) `Maximum`:

```
Get-Random -Minimum 1 -Maximum 21
```

Use simple pipeline input to pick a random element from a list:

```
PS > $suits = "Hearts","Clubs","Spades","Diamonds"
PS > $faces = (2..10)+"A","J","Q","K"
PS > $cards = foreach($suit in $suits) {
     foreach($face in $faces) { "$face of $suit" } }
PS > $cards | Get-Random
A of Spades
PS > $cards | Get-Random
2 of Clubs
```

## Discussion

The `Get-Random` cmdlet solves the problems usually associated with picking random numbers or random elements from a collection: `scaling` and `seeding`.

Most random number generators only generate numbers between 0 and 1. If you need a number from a different range, you have to go through a separate scaling step to map those numbers to the appropriate range. Although not terribly difficult, it’s a usability hurdle that requires more than trivial knowledge to do properly.

Ensuring that the random number generator picks `good` random numbers is a different problem entirely. Most general-purpose random number generators use a mathematical equation to generate their values. These are called `pseudo-random number generators`, or `PRNGs`. They make new values by incorporating the number they generated just before that—a feedback process that guarantees evenly distributed sequences of numbers. Maintaining this internal state is critical, as restarting from a specific point will always generate the same number, which is not very random at all!

To create their first value, these generators need a random number `seed` that they usually derive from the system time.

So unless you reuse the same random number generator, this last point usually leads to the downfall of realistically random numbers. When you generate them quickly, you create new random number generators that are likely to have the same seed. This tends to create runs of duplicate random numbers:

```
PS > 1..10 | ForEach-Object { (New-Object System.Random).Next(1, 21) }
20
7
7
15
15
11
11
18
18
18
```

The `Get-Random` cmdlet saves you from this issue in two ways. Early versions of PowerShell’s `Get-Random` cmdlet implemented a PRNG. The first way that it saved you from this issue was by internally maintaining a random number generator and its state to vastly improve randomness:

```
PS > 1..10 | ForEach-Object { Get-Random -Min 1 -Max 21 }
20
18
7
12
16
10
9
13
16
14
```

However, even as good as this `pseudo`-randomness was, administrators who didn’t realize it wasn’t `truly` random also used this cmdlet to generate passwords and other sensitive things. That is dangerous: if the only two things that went into the generation of a password were the time it was generated and the well-known formula that the random number generator used, that password isn’t very secure.

Despite that, assuming that you could use the `Get-Random` cmdlet to generate random passwords is realistically an assumption that anybody should be allowed to make. So, the second way that PowerShell saves you from this issue is by using a `cryptographic` random number generator. Numbers that `Get-Random` generates are suitable for use in passwords, cryptographic keys, and more.

For scenarios where you want reproducible results, you can use the `-SetSeed` parameter of the `Get-Random` cmdlet to supply a seed directly for testing purposes.

For more information about working with classes from the .NET Framework, see [Recipe 3.8](#ch03.xhtml#work_with_net_objects).

## See Also

[Recipe 3.8, “Work with .NET Objects”](#ch03.xhtml#work_with_net_objects)

# 8.5 Convert Time Between Time Zones

## Problem

You want to determine what a specific time in one time zone is when represented in another.

## Solution

Use the `TimeZoneInfo` class from the .NET Framework:

```
$targetTime = [DateTime] "11/05/2022 9:00 AM"
$targetTimeZone = [TimeZoneInfo]::GetSystemTimeZones() |
    Where-Object Id -match Israel
[TimeZoneInfo]::ConvertTime($targetTime, $targetTimeZone)
```

```
Saturday, November 5, 2022 6:00:00 PM
```

If the time you specify is not your own time zone:

```
$targetTime = [DateTime] "11/05/2022 9:00 AM"

$sourceTimeZone = [TimeZoneInfo]::GetSystemTimeZones() |
    Where-Object Id -match India
$targetTimeZone = [TimeZoneInfo]::GetSystemTimeZones() |
    Where-Object Id -match Israel
[TimeZoneInfo]::ConvertTime($targetTime, $sourceTimeZone, $targetTimeZone)
```

```
Saturday, November 5, 2022 5:30:00 AM
```

## Discussion

When working with people from around the world, keeping track of time zone differences can be overwhelming. The observation of daylight saving time is inconsistent around the world, and mental math when the international date line is involved is enough to make your head hurt in the best of times.

Fortunately, the `TimeZoneInfo` class from the .NET Framework can help with these challenges. It understands 140 different representations of time zones, their Coordinated Universal Time (UTC) offset, calendar changes, time adjustments, and more.

## See Also

[Recipe 3.8, “Work with .NET Objects”](#ch03.xhtml#work_with_net_objects)

# 8.6 Program: Search the Windows Start Menu

When working at the command line, you might want to launch a program that’s normally found only on your Start menu. While you could certainly click through the Start menu to find it, you could also search the Start menu with a script, as shown in [Example 8-2](#ch08.xhtml#search-startmenups1).

## Example 8-2. Search-StartMenu.ps1
