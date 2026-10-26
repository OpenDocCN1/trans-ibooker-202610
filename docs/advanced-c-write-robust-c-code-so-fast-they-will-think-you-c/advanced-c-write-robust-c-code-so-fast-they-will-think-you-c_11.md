## 第 2B 章：禁止鸭子 – 模板与推导

### 活动 1：开发一个通用的“contains”模板函数

在本活动中，我们将实现几个辅助类，用于检测 `std::string` 类的情况和 `std::set` 的情况，然后使用它们针对特定容器定制 contains 函数。按以下步骤完成此活动：

1. 从 `Lesson2B/Activity01` 文件夹中加载准备好的项目。构建并配置启动器，然后运行单元测试（其中那个虚拟测试会失败）。我们建议测试运行器使用的名称是 `L2BA1tests`。

2. 打开 `containsTests.cpp` 文件，将现有测试替换为以下内容：

```
TEST_F(containsTest, DetectNpos)
{
    ASSERT_TRUE(has_npos_v<std::string>);
    ASSERT_FALSE(has_npos_v<std::set<int>>);
    ASSERT_FALSE(has_npos_v<std::vector<int>>);
}
```

该测试要求我们编写一组辅助模板来检测容器类是否支持名为 `npos` 的静态成员变量。

3. 向 `contains.hpp` 文件添加以下代码：

```
template <class T>
auto test_npos(int) -> decltype((void)T::npos, std::true_type{});

template <class T>
auto test_npos(long) -> std::false_type;

template <class T>
struct has_npos : decltype(test_npos<T>(0)) {};

template< class T >
inline constexpr bool has_npos_v = has_npos<T>::value;
```

现在测试可以运行并通过。

4. 向 `containsTests.cpp` 文件添加以下测试：

```
TEST_F(containsTest, DetectFind)
{
    ASSERT_TRUE((has_find_v<std::string, char>));
    ASSERT_TRUE((has_find_v<std::set<int>, int>));
}
```

```
if constexpr(has_npos_v<C>)
{
    return C::npos != c.find(key);
}
else
if constexpr(has_find_v<C, T>)
{
    return std::end(c) != c.find(key);
}
else
{
    return std::end(c) != std::find(begin(c), end(c), key);
}
```

现在测试可以运行并通过。

12. 构建并运行名为 `contains` 的应用程序。创建一个新的运行配置。如果你对 `contains` 模板的实现是正确的，程序将显示以下输出：

```
----- Activity 1 -----
Set contains 5: true
Set contains 15: false
Vector contains 5: true
Vector contains 15: false
String contains 'times': true
String contains 'Light': false
String contains 'm': true
String contains 'z': false

Complete.
```

图 2B.36：contains 成功实现的输出

在本活动中，我们结合 SFINAE 使用了各种模板技术，基于容器类的能力来选择 `contains()` 函数的适当实现。我们本可以使用一个通用模板函数和一些特化模板来达到同样的结果，但我们选择了一条少有人走的路，并施展了我们新学到的模板技能。

10. 重新编译后，编译器报告的下一个错误指向以下内联函数：

```
inline Matrix3d operator*(const Matrix3d& lhs, const Matrix3d& rhs)
{
    Matrix3d temp(lhs);  // <=== compiler error - ill-formed copy constructor
    temp *= rhs;
    return temp;
}
```

11. 之前，默认拷贝构造函数就足以满足我们的需求，它只对数组的所有元素进行浅拷贝，而且这是正确的。现在我们有了对需要拷贝的数据的间接访问，因此需要实现深拷贝构造函数和拷贝赋值操作符。我们还需要处理现有的构造函数。目前，只需将构造函数声明添加到类中（紧挨着其他构造函数）：

```
Matrix3d(const Matrix3d& rhs);
Matrix3d& operator=(const Matrix3d& rhs);
```

尝试构建测试将表明我们已经解决了头文件中的所有问题，现在可以转向实现文件了。

12. 修改两个构造函数，按如下方式初始化 `unique_ptr`：

```
Matrix3d::Matrix3d() : m_data{new float[NumberRows * NumberColumns]}
{
    for (int i{0}; i < NumberRows; i++)
        for (int j{0}; j < NumberColumns; j++)
            m_data[i * NumberColumns + j] = (i == j);
}
```

```
Matrix3d::Matrix3d(std::initializer_list<std::initializer_list<float>> list)
    : m_data{new float[NumberRows * NumberColumns]}
{
    int i{0};
    for (auto it1 = list.begin(); i < NumberRows; ++it1, ++i)
    {
        int j{0};
        for (auto it2 = it1->begin(); j < NumberColumns; ++it2, ++j)
            m_data[i * NumberColumns + j] = *it2;
    }
}
```

16. 完成这些修改后，我们已经修复了所有编译器错误，但现在要处理一个链接器错误——就是我们在步骤 11 中仅仅声明了的拷贝构造函数。

17. 在 `matrix3d.cpp` 文件中添加以下定义：

```
Matrix3d::Matrix3d(const Matrix3d& rhs) :
    m_data{new float[NumberRows * NumberColumns]}
{
    *this = rhs;
}
```

```
Matrix3d& Matrix3d::operator=(const Matrix3d& rhs)
{
    for (int i = 0; i < NumberRows * NumberColumns; i++)
        m_data[i] = rhs.m_data[i];
    return *this;
}
```

18. 测试现在将构建成功，并且全部通过。下一步是强制使用移动构造函数。在 `matrix3d.cpp` 中找到 `createTranslationMatrix()` 方法，并按如下方式修改返回语句：

```
return std::move(matrix);
```

19. 在 `matrix3d.hpp` 中声明移动构造函数。

```
Matrix3d(Matrix3d&& rhs);
```

20. 重新构建测试。现在，我们会得到一个与移动构造函数不存在相关的错误。

21. 将移动构造函数的实现添加到 `matrix3d.cpp` 中，然后重新构建测试。

```
Matrix3d::Matrix3d(Matrix3d&& rhs)
{
    //std::cerr << "Matrix3d::Matrix3d(Matrix3d&& rhs)\n";
    std::swap(m_data, rhs.m_data);
}
```

22. 重新构建并运行测试。它们将再次全部通过。

23. 为了确认移动构造函数确实被调用了，在 `matrix3d.cpp` 中添加 `#include <iostream>`，取消移动构造函数中输出行的注释，然后重新运行测试。测试完成后会报告一个错误，因为我们将其发送到了标准错误通道（`cerr`）。检查完毕后，将那一行重新注释掉。

> **注意**
> 关于移动构造函数有一点需要说明——我们没有像其他构造函数那样显式初始化 `m_data`。这意味着它将被初始化为空，然后与传入的参数进行交换。该参数是一个临时对象，因此在交换操作之后它不持有数组是可以接受的——这样就省去了一次内存分配和释放。

24. 现在让我们转换 `Point3d` 类，使其能够使用堆分配的内存。在 `point3d.hpp` 文件中添加 `#include <memory>` 行，以便我们可以使用 `unique_ptr<>` 模板。

25. 接下来，将 `m_data` 的声明类型改为如下形式：

```
std::unique_ptr<float[]> m_data;
```

26. 编译器现在告诉我们，`point3d.hpp` 中的插入运算符（`<<`）有问题，因为我们不能在 `unique_ptr` 上使用基于范围的 for 循环：将实现替换为以下内容：

```
inline std::ostream&
operator<<(std::ostream& os, const Point3d& pt)
{
    const char* sep = "[ ";
    for (int i{0}; i < Point3d::NumberRows; i++)
    {
        os << sep << pt.m_data[i];
        sep = ", ";
    }
    os << " ]";
    return os;
}
```

27. 打开 `point3d.cpp`，修改默认构造函数以初始化 `unique_ptr`，并修改初始化循环，因为不能对 `unique_ptr` 使用基于范围的 for 循环：

```
Point3d::Point3d() : m_data{new float[NumberRows]}
{
    for (int i{0}; i < NumberRows - 1; i++) {
        m_data[i] = 0;
    }
    m_data[NumberRows - 1] = 1;
}
```

28. 修改另一个构造函数，初始化 `unique_ptr`：

```
Point3d::Point3d(std::initializer_list<float> list)
    : m_data{new float[NumberRows]}
```

29. 现在所有测试都运行并通过，和之前一样。

30. 现在，如果我们运行原始应用程序 `L3graphics`，输出将与原始版本一致，但该实现使用 RAII 来分配和管理矩阵与点所使用的内存。

```
------ Activity 1 ------
The point [ 1, 1, 1, 1 ]
...rotated around X by 90 degrees
...rotated around Y by 90 degrees
...rotated around Z by 90 degrees
moves to [ 1, 1, -1, 1 ]
...scaled by x=2, y = 3, z = 4
moves to [ 2, 3, -4, 1 ]
...translated by dx=-2, dy = 1, dz = -3
moves to [ 0, 4, -7, 1 ]

Complete.
```

图 3.52：成功转换为使用 RAII 后的活动 1 输出





### 活动 2：实现用于日期计算的类

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
