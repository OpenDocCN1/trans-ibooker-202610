

## 活动 2：实现用于日期计算的类

在本活动中，我们将实现两个类：`Date` 和 `Days`，它们将使我们能够非常轻松地处理日期以及日期之间的时间差。让我们开始吧：

- 1. 从 `Lesson3/Activity02` 文件夹加载准备好的项目，并将项目的 Current Builder 配置为 `CMake Build (Portable)`。构建并配置启动器，然后运行单元测试。我们建议测试运行器的名称使用 `L3A2datetests`。该项目包含一些占位文件和一个失败的测试。
- 2. 在编辑器中打开 `date.hpp` 文件，并在基本的 `Date` 类中添加以下几行，以允许访问存储的值：
```
int Day() const {return m_day;}
int Month() const {return m_month;}
int Year() const {return m_year;}
```
- 3. 打开 `dateTests.cpp` 文件，并将以下代码添加到 `DateTest` 类中：
```
void VerifyDate(const Date& dt, int yearExp, int monthExp, int dayExp) const
{
    ASSERT_EQ(dayExp, dt.Day());
    ASSERT_EQ(monthExp, dt.Month());
    ASSERT_EQ(yearExp, dt.Year());
}
```
通常，随着测试的开发，你会对这个测试进行重构，但我们会提前把它提取出来。
- 4. 将现有测试中的 `ASSERT_FALSE()` 替换为以下测试：
```
Date dt;
VerifyDate(dt, 1970, 1, 1);
```
- 5. 重新构建并运行测试——它们现在应该全部通过。
- 6. 添加以下测试：
```
TEST_F(DateTest, Constructor1970Jan2)
{
    Date dt(2, 1, 1970);
    VerifyDate(dt, 1970, 1, 2);
}
```

```
int Date::CalcNumberLeapYearsFromEpoch(int year) const
{
    return (year-1)/YearsBetweenLeapYears
        - (EpochYear-1)/YearsBetweenLeapYears;
}
int Date::GetDayOfYear(int day, int month, int year) const
{
    return daysBeforeMonth[IsLeapYear(year)][month-1] + day;
}
bool Date::IsLeapYear(int year) const
{
    return (year%4)==0;  // 并非完整规则，但到2100年为止足够
}
date_t Date::ToDateT() const
{
    date_t value = GetDayOfYear(m_day, m_month, m_year) - 1;
    value += (m_year-EpochYear) * DaysPerCommonYear;
    date_t numberLeapYears = CalcNumberLeapYearsFromEpoch(m_year);
    value += numberLeapYears;
    return value;
}
}
```

16. 既然 `ToDateT()` 已经可以工作了，我们转向它的逆函数，即 `FromDateT()`。同样，我们一次一个地构建测试，以便在一系列日期上开发算法。以下是所使用的测试：

```
TEST_F(DateTest, FromDateT0Is1Jan1970)
{
    Date dt;
    dt.FromDateT(0);
    ASSERT_EQ(0, dt.ToDateT());
    VerifyDate(dt, 1970, 1, 1);
}
TEST_F(DateTest, FromDateT1Is2Jan1970)
{
    Date dt;
    dt.FromDateT(1);
    ASSERT_EQ(1, dt.ToDateT());
    VerifyDate(dt, 1970, 1, 2);
}
TEST_F(DateTest, FromDateT364Is31Dec1970)
{
```

```
Date dt;
    dt.FromDateT(364);
    ASSERT_EQ(364, dt.ToDateT());
    VerifyDate(dt, 1970, 12, 31);
}
TEST_F(DateTest, FromDateT365Is1Jan1971)
{
    Date dt;
    dt.FromDateT(365);
    ASSERT_EQ(365, dt.ToDateT());
    VerifyDate(dt, 1971, 1, 1);
}
TEST_F(DateTest, FromDateT1096Is1Jan1973)
{
    Date dt;
    dt.FromDateT(1096);
    ASSERT_EQ(1096, dt.ToDateT());
    VerifyDate(dt, 1973, 1, 1);
}
TEST_F(DateTest, FromDateT18136Is28Aug2019)
{
    Date dt;
    dt.FromDateT(18136);
    ASSERT_EQ(18136, dt.ToDateT());
    VerifyDate(dt, 2019, 8, 28);
}
```

17. 将以下声明添加到头文件中：

```
public:
    void FromDateT(date_t date);
private:
    int CalcMonthDayOfYearIsIn(int dayOfYear, bool IsLeapYear) const;
```

18. 使用以下实现，因为前面的测试是逐个添加的：

```
void Date::FromDateT(date_t date)
{
    int number_years = date / DaysPerCommonYear;
    date = date - number_years * DaysPerCommonYear;
    m_year = EpochYear + number_years;
    date_t numberLeapYears = CalcNumberLeapYearsFromEpoch(m_year);
    date -= numberLeapYears;
```

```
m_month = CalcMonthDayOfYearIsIn(date, IsLeapYear(m_year));
date -= daysBeforeMonth[IsLeapYear(m_year)][m_month-1];
m_day = date + 1;
}
int Date::CalcMonthDayOfYearIsIn(int dayOfYear, bool isLeapYear) const
{
    for(int i = 1 ; i < 12; i++)
    {
        if ( daysBeforeMonth[isLeapYear][i] > dayOfYear)
            return i;
    }
    return 12;
}
```

19. 现在我们已经准备好了辅助例程，可以实现 `Date` 类的真正功能了：两个日期之间的差值，以及通过添加天数来确定新日期。这两个操作都需要一个新类型（类）`Days`。

20. 将 `Days` 的以下实现添加到头文件中（位于 `Date` 的上方）：

```
class Days
{
public:
    Days() = default;
    Days(int days) : m_days{days}    {  }
    operator int() const
    {
        return m_days;
    }
private:
    int m_days{0};
};
```

21. 第一个运算符将是 `Days` 与 `Date` 的加法。在 `Date` 类的公共部分添加以下方法声明：

```
Date& operator+=(const Days& day);
```

22. 然后，将内联实现（在 `Date` 类外部）添加到头文件中：

```
inline Date operator+(const Date& lhs, const Days& rhs )
{
    Date tmp(lhs);
```

27. 现在构建失败了，所以我们需要通过将加法行改为强制转换为 `Days` 来修复测试，如下所示：

```
dt += static_cast<Days>(4);
```

所有测试应该再次通过。

28. 我们想要实现的最后一个功能是两个日期之间的差值。以下是用于验证实现的测试：

```
TEST_F(DateTest, DateDifferences27days)
{
    Date dt1(28, 8, 2019);
    Date dt2(1, 8, 2019);
    
    Days days = dt1 - dt2;
    ASSERT_EQ(27, (int)days);
}
```

```
TEST_F(DateTest, DateDifferences365days)
{
    Date dt1(28, 8, 2019);
    Date dt2(28, 8, 2018);
    
    Days days = dt1 - dt2;
    ASSERT_EQ(365, (int)days);
}
```

29. 在头文件中的 `Date` 类的公共部分添加以下函数声明：

```
Days operator-(const Date& rhs) const;
```

30. 在头文件中的 `Date` 类之后添加以下代码：

```
inline Days Date::operator-(const Date& rhs) const
{
    return Days(ToDateT() - rhs.ToDateT());
}
```

因为我们将 `Days` 构造函数设为显式，所以必须在 return 语句中调用它。完成所有这些更改后，所有测试都应该通过。

31. 将 **L3A2date** 配置为 **datetools** 二进制文件，并在编辑器中打开 `main.cpp`。从 **ACTIVITY2** 的定义中移除注释：

```
#define ACTIVITY2
```

32. 构建，然后运行示例应用程序。这将产生以下输出：

```
------ Activity 2 ------
There are 358 days between 1-Jan-2019 and 25-Dec-2019
There are 0.980172 years between 1-Jan-2019 and 25-Dec-2019

There are 364 days between 1-Jan-2019 and 31-Dec-2019
There are 0.996599 years between 1-Jan-2019 and 31-Dec-2019

There are 18255 days between 1-Jan-1970 and 25-Dec-2019
There are 49.9805 years between 1-Jan-1970 and 25-Dec-2019

There are 36525 days between 1-Jan-1970 and 1-Jan-2070
There are 100.002 years between 1-Jan-1970 and 1-Jan-2070

Complete.
```

图 3.53：成功的 Date 示例应用程序的输出

我们已经实现了 `Date` 和 `Days` 类的所有需求，并且全部通过单元测试交付。单元测试使我们能够逐步实现功能，从而构建出两个复杂的算法：**ToDateT** 和 **FromDateT**，它们构成了我们希望交付的功能的基础支撑。

## 第 4 章 - 关注点分离 - 软件架构、函数、可变参数模板

## 活动 1：实现一个多播事件处理器

- 1. 从 **Lesson4/Activity01** 文件夹加载准备好的项目，并将项目的 Current Builder 配置为 `CMake Build (Portable)`。构建项目，配置启动器并运行单元测试（这些测试会失败一个占位测试）。建议测试运行器的名称使用 `L4delegateTests`。
- 2. 在 **delegateTests.cpp** 中，将失败的占位测试替换为以下测试：

```
TEST_F(DelegateTest, BasicDelegate)
{
    Delegate delegate;
    ASSERT_NO_THROW(delegate.Notify(42));
}
```

- 3. 现在构建会失败，所以我们需要向 **Delegate** 添加一个新方法。由于这将会演变成一个模板，我们将在头文件中完成所有开发。在 **delegate.hpp** 中，添加以下定义：

```
class Delegate
{
public:
    Delegate() = default;
    void Notify(int value) const
    {
    }
};
```

测试现在可以运行并通过了。

- 4. 在现有测试中添加以下行：

```
ASSERT_NO_THROW(delegate(22));
```

