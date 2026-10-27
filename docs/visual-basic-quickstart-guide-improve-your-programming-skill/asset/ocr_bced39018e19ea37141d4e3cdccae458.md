The **DateAdd** function’s first parameter is a VB constant to represent the type of value being added to the third parameter. Here are the possible types and the constants used:

+   - **yyyy**: Year
+   - **q**: Quarter
+   - **m**: Month
+   - **y**: Day of year
+   - **d**: Day
+   - **w**: Weekday
+   - **ww**: Week
+   - **h**: Hour
+   - **n**: Minute
+   - **s**: Second

# Date and time component extraction

Using the respective properties or functions, you can extract specific components (year, month, day, hour, minute, or second) from a **Date** or **Time** value. In VB.NET, a property is available on a **DateTime** value. In VB6, VBScript, and VBA, there is a function with the same name. Here’s an example in VB.NET:

```
Dim curDate As Date = Date.Now
Dim curYear As Integer = curDate.Year
Dim curMonth As Integer = curDate.Month
Dim curDay As Integer = curDate.Day
Dim curHour As Integer = curDate.Hour
Dim curMinute As Integer = curDate.Minute
Dim curSecond As Integer = curDate.Second
```

The same example in earlier VB members would look like this:

```
Dim curDate As Date = Now()
Dim curYear As Integer = Year(curDate)
Dim curMonth As Integer = Month(curDate)
Dim curDay As Integer = Day(curDate)
Dim curHour As Integer = Hour(curDate)
Dim curMinute As Integer = Minute(curDate)
Dim curSecond As Integer = Second(curDate)
```

Next, we will look at formatting numeric values.

# Formatting numeric values

In VB, you can format data using various functions and methods to present it in the desired format. Here are some commonly used techniques for formatting data in Visual Basic: