
# 活动 1：将 Point3d 转换为使用堆分配内存

关于移动构造函数的一个简短说明——我们没有像其他构造函数那样显式初始化 `m_data`。这意味着它将被初始化为空，然后与传入的参数（一个临时对象）进行交换，因此事务结束后它不持有数组也是可以接受的——这消除了一次内存分配和释放。

24. 现在让我们转换 `Point3d` 类，使其能够使用堆分配内存。在 `point3d.hpp` 文件中，添加一行 `#include <memory>`，以便我们可以访问 `unique_ptr<>` 模板。

25. 接下来，将 `m_data` 的声明类型更改为如下所示：

```
std::unique_ptr<float[]> m_data;
```

26. 编译器现在会告诉我们，`point3d.hpp` 中的插入运算符（`<<`）存在问题，因为我们不能在 `unique_ptr` 上使用范围 for 循环。将实现替换为以下内容：

```
inline std::ostream&
operator<<(std::ostream& os, const Point3d& pt)
{
    const char* sep = "[ ";
    for(int i{0} ; i < Point3d::NumberRows ; i++)
    {
        os << sep << pt.m_data[i];
        sep = ", ";
    }
    os << " ]";
    return os;
}
```

27. 打开 `point3d.cpp` 并修改默认构造函数以初始化 `unique_ptr`，并更改初始化循环，因为不能在 `unique_ptr` 上使用范围 for 循环：

```
Point3d::Point3d() : m_data{new float[NumberRows]}
{
    for(int i{0} ; i < NumberRows-1 ; i++) {
        m_data[i] = 0;
    }
    m_data[NumberRows-1] = 1;
}
```

28. 通过初始化 `unique_ptr` 来修改另一个构造函数：

```
Point3d::Point3d(std::initializer_list<float> list)
        : m_data{new float[NumberRows]}
```

29. 现在所有测试都能运行并通过，就像之前一样。

30. 现在，如果我们运行原始应用程序 `L3graphics`，输出将与原始输出完全相同，但实现使用 RAII 来分配和管理用于矩阵和点的内存。

![成功转换为使用 RAII 后的活动 1 输出](image/C14583_03_52.jpg)

*图 3.52：成功转换为使用 RAII 后的活动 1 输出*

# 活动 2：实现日期计算的类

在本活动中，我们将实现两个类 `Date` 和 `Days`，它们将使我们非常容易地处理日期以及它们之间的时间差。让我们开始吧：

1. 从 `Lesson3/Activity02` 文件夹加载准备好的项目，并将项目的当前构建器配置为 **CMake Build (Portable)**。构建并配置启动器，然后运行单元测试。我们建议测试运行器使用的名称为 `L3A2datetests`。该项目包含虚拟文件和一个失败的测试。

2. 在编辑器中打开 `date.hpp` 文件，并在基本 `Date` 类中添加以下行，以允许访问存储的值：

```
int Day()   const {return m_day;}
int Month() const {return m_month;}
int Year()  const {return m_year;}
```

3. 打开 `dateTests.cpp` 文件，并将以下代码添加到 `DateTest` 类中：

```
void VerifyDate(const Date& dt, int yearExp, int monthExp, int dayExp) const
{
    ASSERT_EQ(dayExp, dt.Day());
    ASSERT_EQ(monthExp, dt.Month());
    ASSERT_EQ(yearExp, dt.Year());
}
```

通常，随着测试的开发，你会重构这个测试，但我们将提前把它提取出来。

4. 将现有测试中的 `ASSERT_FALSE()` 替换为以下测试：

```
Date dt;
VerifyDate(dt, 1970, 1, 1);
```

5. 重新构建并运行测试——它们现在应该都能通过。

6. 添加以下测试：

```
TEST_F(DateTest, Constructor1970Jan2)
{
    Date dt(2, 1, 1970);
    VerifyDate(dt, 1970, 1, 2);
}
```

7. 为了使该测试通过，我们需要向 `Date` 类添加以下两个构造函数：

```
Date() = default;
Date(int day, int month, int year) :
        m_year{year}, m_month{month}, m_day{day}
{
}
```

8. 我们现在需要引入转换到/从 `date_t` 类型的函数。在我们的命名空间内，向 `date.hpp` 文件添加以下别名：

```
using date_t = int64_t;
```

9. 向 `Date` 类添加以下方法的声明：

```
date_t ToDateT() const;
```

10. 然后，添加以下测试：

```
TEST_F(DateTest, ToDateTDefaultIsZero)
{
    Date dt;
    ASSERT_EQ(0, dt.ToDateT());
}
```

11. 因为我们正在进行 **TDD**，所以我们添加该方法的最小实现以通过测试。

```
date_t Date::ToDateT() const
{
    return 0;
}
```

12. 现在，我们添加下一个测试：

```
TEST_F(DateTest, ToDateT1970Jan2Is1)
{
    Date dt(2, 1, 1970);
    ASSERT_EQ(1, dt.ToDateT());
}
```

13. 我们继续添加一个又一个测试，同时不断改进 `ToDateT()` 中的算法，首先处理 **1970** 年的日期，然后是 **1971 年 1 月 1 日**，接着是 **1973** 年的一个日期，这意味着我们跨越了一个闰年，依此类推。用于开发 `ToDateT()` 方法的完整测试集如下：

```
TEST_F(DateTest, ToDateT1970Dec31Is364)
{
    Date dt(31, 12, 1970);
    ASSERT_EQ(364, dt.ToDateT());
}

TEST_F(DateTest, ToDateT1971Jan1Is365)
{
    Date dt(1, 1, 1971);
    ASSERT_EQ(365, dt.ToDateT());
}

TEST_F(DateTest, ToDateT1973Jan1Is1096)
{
    Date dt(1, 1, 1973);
    ASSERT_EQ(365*3+1, dt.ToDateT());
}

TEST_F(DateTest, ToDateT2019Aug28Is18136)
{
    Date dt(28, 8, 2019);
    ASSERT_EQ(18136, dt.ToDateT());
}
```

14. 为了通过所有这些测试，我们向 `Date` 类的声明中添加以下项目：

```
public:
    static constexpr int EpochYear = 1970;
    static constexpr int DaysPerCommonYear = 365;
    static constexpr int YearsBetweenLeapYears = 4;

private:
    int GetDayOfYear(int day, int month, int year) const;
    bool IsLeapYear(int year) const;
    int CalcNumberLeapYearsFromEpoch(int year) const;
```

15. `date.cpp` 中 `ToDateT()` 和支持方法的实现如下：

```
namespace {

int daysBeforeMonth[2][12] =
{
    { 0, 31, 59, 90, 120, 151, 181, 212, 243, 273, 204, 334}, // Common Year
    { 0, 31, 50, 91, 121, 152, 182, 213, 244, 274, 205, 335}  // Leap Year
};

}

namespace acpp::date
{

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
    return (year%4)==0;   // Not full story, but good enough to 2100
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

16. 现在 `ToDateT()` 已经工作，我们转向它的逆操作，即 `FromDateT()`。同样，我们一次添加一个测试，以在一系列日期上开发算法。使用了以下测试：

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
```
