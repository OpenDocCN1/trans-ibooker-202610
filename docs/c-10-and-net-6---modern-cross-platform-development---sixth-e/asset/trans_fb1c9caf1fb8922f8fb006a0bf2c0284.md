
## 使用日期和时间

在数字和文本之后，最受欢迎的数据类型就是日期和时间。两种主要类型如下：

- DateTime：表示某个固定时间点的组合日期和时间值。
- TimeSpan：表示一段时间的持续时间。

这两种类型经常一起使用。例如，如果你从一个 DateTime 值中减去另一个 DateTime 值，结果就是一个 TimeSpan。如果将一个 TimeSpan 加到一个 DateTime 上，结果就是一个 DateTime 值。

## 指定日期和时间值

创建日期和时间值的常见方法是通过指定日期和时间组件的单个值，如日和小时，如下表所述：

| 日期/时间参数 | 值范围 |
|---|---|
| year | 1 to 9999 |
| month | 1 to 12 |
| day | 1 to the number of days in that month |
| hour | 0 to 23 |
| minute | 0 to 59 |
| second | 0 to 59 |

另一种方法是提供一个字符串以供解析，但这可能会根据线程的默认文化而被误解。例如，在英国，日期指定为日/月/年，而在美国，日期指定为月/日/年。

让我们看看你可能想用日期和时间做什么：

1. 使用你喜欢的代码编辑器，在 Chapter08 解决方案/工作区中添加一个名为 WorkingWithTime 的新控制台应用。
2. 在 Visual Studio Code 中，将 WorkingWithTime 选择为活动的 OmniSharp 项目。
3. 在 Program.cs 中，删除现有语句，然后添加语句以初始化一些特殊的日期/时间值，如以下代码所示：

```
WriteLine("Earliest date/time value is: {0}",
    arg0: DateTime.MinValue);

WriteLine("UNIX epoch date/time value is: {0}",
    arg0: DateTime.UnixEpoch);

WriteLine("Date/time value Now is: {0}",
    arg0: DateTime.Now);

WriteLine("Date/time value Today is: {0}",
    arg0: DateTime.Today);
```

4. 运行代码并注意结果，如以下输出所示：

```
Earliest date/time value is: 01/01/0001 00:00:00
UNIX epoch date/time value is: 01/01/1970 00:00:00
Date/time value Now is: 23/04/2021 14:14:54
Date/time value Today is: 23/04/2021 00:00:00
```

5. 添加语句来定义 2021 年的圣诞节（如果这是过去的年份则使用未来的年份）并以各种方式显示它，如以下代码所示：

```
DateTime christmas = new(year: 2021, month: 12, day: 25);

WriteLine("Christmas: {0}",
    arg0: christmas); // default format

WriteLine("Christmas: {0:dddd, dd MMMM yyyy}",
    arg0: christmas); // custom format

WriteLine("Christmas is in month {0} of the year.",
    arg0: christmas.Month);

WriteLine("Christmas is day {0} of the year.",
    arg0: christmas.DayOfYear);

WriteLine("Christmas {0} is on a {1}.",
    arg0: christmas.Year,
    arg1: christmas.DayOfWeek);
```

6. 运行代码并注意结果，如以下输出所示：

```
Christmas: 25/12/2021 00:00:00
Christmas: Saturday, 25 December 2021
Christmas is in month 12 of the year.
Christmas is day 359 of the year.
Christmas 2021 is on a Saturday.
```

7. 添加语句对圣诞节执行加法和减法，如以下代码所示：

```
DateTime beforeXmas = christmas.Subtract(TimeSpan.FromDays(12));
DateTime afterXmas = christmas.AddDays(12);

WriteLine("12 days before Christmas is: {0}",
    arg0: beforeXmas);

WriteLine("12 days after Christmas is: {0}",
    arg0: afterXmas);

TimeSpan untilChristmas = christmas - DateTime.Now;

WriteLine("There are {0} days and {1} hours until Christmas.",
    arg0: untilChristmas.Days,
    arg1: untilChristmas.Hours);

WriteLine("There are {0:N0} hours until Christmas.",
    arg0: untilChristmas.TotalHours);
```

8. 运行代码并注意结果，如以下输出所示：

```
12 days before Christmas is: 13/12/2021 00:00:00
12 days after Christmas is: 06/01/2022 00:00:00
There are 245 days and 9 hours until Christmas.
There are 5,890 hours until Christmas.
```

9. 添加语句来定义圣诞节当天孩子们可能起床开礼物的时间，并以各种方式显示它，如以下代码所示：

```
DateTime kidsWakeUp = new(
    year: 2021, month: 12, day: 25,
    hour: 6, minute: 30, second: 0);

WriteLine("Kids wake up on Christmas: {0}",
    arg0: kidsWakeUp);

WriteLine("The kids woke me up at {0}",
    arg0: kidsWakeUp.ToShortTimeString());
```

10. 运行代码并注意结果，如以下输出所示：

```
Kids wake up on Christmas: 25/12/2021 06:30:00
The kids woke me up at 06:30
```

## 日期和时间的全球化

当前文化控制日期和时间的解析方式：

1. 在 Program.cs 顶部，导入 System.Globalization 命名空间。
2. 添加语句以显示用于显示日期和时间值的当前文化，然后解析美国独立日并以各种方式显示它，如以下代码所示：

```
WriteLine("Current culture is: {0}",
    arg0: CultureInfo.CurrentCulture.Name);

string textDate = "4 July 2021";
DateTime independenceDay = DateTime.Parse(textDate);

WriteLine("Text: {0}, DateTime: {1:d MMMM}",
    arg0: textDate,
    arg1: independenceDay);

textDate = "7/4/2021";
independenceDay = DateTime.Parse(textDate);

WriteLine("Text: {0}, DateTime: {1:d MMMM}",
    arg0: textDate,
    arg1: independenceDay);

independenceDay = DateTime.Parse(textDate,
    provider: CultureInfo.GetCultureInfo("en-US"));

WriteLine("Text: {0}, DateTime: {1:d MMMM}",
    arg0: textDate,
    arg1: independenceDay);
```

3. 运行代码并注意结果，如以下输出所示：

```
Current culture is: en-GB
Text: 4 July 2021, DateTime: 4 July
Text: 7/4/2021, DateTime: 7 April
Text: 7/4/2021, DateTime: 4 July
```

在我的电脑上，当前文化是英式英语。如果日期以 4 July 2021 给出，则无论当前文化是英式还是美式，都能正确解析。但如果日期以 7/4/2021 给出，则会被错误解析为 7 月 4 日。你可以通过在解析时指定正确的文化作为提供者来覆盖当前文化，如上第三个示例所示。

4. 添加语句从 2020 年循环到 2025 年，显示年份是否为闰年以及二月有多少天，然后显示圣诞节和独立日是否在夏令时期间，如以下代码所示：

```
for (int year = 2020; year < 2026; year++)
{
    Write($"{year} is a leap year: {DateTime.IsLeapYear(year)}. ");
    WriteLine("There are {0} days in February {1}.",
        arg0: DateTime.DaysInMonth(year: year, month: 2), arg1: year);
}

WriteLine("Is Christmas daylight saving time? {0}",
    arg0: christmas.IsDaylightSavingTime());

WriteLine("Is July 4th daylight saving time? {0}",
    arg0: independenceDay.IsDaylightSavingTime());
```

5. 运行代码并注意结果，如以下输出所示：

```
2020 is a leap year: True. There are 29 days in February 2020.
2021 is a leap year: False. There are 28 days in February 2021.
2022 is a leap year: False. There are 28 days in February 2022.
2023 is a leap year: False. There are 28 days in February 2023.
2024 is a leap year: True. There are 29 days in February 2024.
2025 is a leap year: False. There are 28 days in February 2025.
Is Christmas daylight saving time? False
Is July 4th daylight saving time? True
```

## 仅使用日期或时间

.NET 6 引入了一些新的类型，用于仅处理日期值或仅处理时间值，名为 DateOnly 和 TimeOnly。这些类型比使用带有零时间的 DateTime 值来存储仅日期值更好，因为它是类型安全的并且避免了误用。DateOnly 也能更好地映射到数据库列类型，例如 SQL Server 中的日期列。TimeOnly 适用于设置闹钟和安排定期会议或活动，并且它映射到 SQL Server 中的时间列。

让我们用它们来为英国女王计划一个派对：

1. 添加语句来定义女王的生日，以及她派对开始的时间，然后将这两个值组合起来制作一个日历条目，以便我们不会错过她的派对，如以下代码所示：

```
DateOnly queensBirthday = new(year: 2022, month: 4, day: 21);
WriteLine($"The Queen's next birthday is on {queensBirthday}.");

TimeOnly partyStarts = new(hour: 20, minute: 30);
WriteLine($"The Queen's party starts at {partyStarts}.");

DateTime calendarEntry = queensBirthday.ToDateTime(partyStarts);
WriteLine($"Add to your calendar: {calendarEntry}.");
```

2. 运行代码并注意结果，如以下输出所示：

```
The Queen's next birthday is on 21/04/2022.
The Queen's party starts at 20:30.
Add to your calendar: 21/04/2022 20:30:00.
```
