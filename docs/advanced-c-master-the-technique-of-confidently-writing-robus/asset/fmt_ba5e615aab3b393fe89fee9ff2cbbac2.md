
# Activity 1: Converting Point3d to Use Heap Allocated Memory

Just a quick note about the move constructor – we did not explicitly initialize `m_data` like we did for the other constructors. This means that it will be initialized as empty and then swapped with the parameter that is passed in, which is a temporary and so it is acceptable for it to not hold an array after the transaction – it removes one allocation and deallocation of memory.

24. Now let's convert the `Point3d` class so that it can use heap allocated memory. In the `point3d.hpp` file, add an `#include <memory>` line so that we have access to the `unique_ptr<>` template.

25. Next, change the type of the declaration for `m_data` to be like so:

```
std::unique_ptr<float[]> m_data;
```

26. The compiler now tells us that we have a problem with the insertion operator (`<<`) in `point3d.hpp` because we can't use a ranged-for on `unique_ptr`. Replace the implementation with the following:

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

27. Open `point3d.cpp` and modify the default constructors to initialize the `unique_ptr` and change the initialization loop since a ranged for cannot be used on the `unique_ptr`:

```
Point3d::Point3d() : m_data{new float[NumberRows]}
{
    for(int i{0} ; i < NumberRows-1 ; i++) {
        m_data[i] = 0;
    }
    m_data[NumberRows-1] = 1;
}
```

28. Modify the other constructor by initializing the `unique_ptr`:

```
Point3d::Point3d(std::initializer_list<float> list)
        : m_data{new float[NumberRows]}
```

29. Now all the tests run and pass, like they did previously.

30. Now, if we run the original application, `L3graphics`, then the output will be identical to the original, but the implementation uses RAII to allocate and manage the memory that's used for the matrices and points.

![Activity 1 output after successful conversion to use RAII](image/C14583_03_52.jpg)

*Figure 3.52: Activity 1 output after successful conversion to use RAII*

# Activity 2: Implementing Classes for Date Calculations

In this activity, we will implement two classes, `Date` and `Days` that will make it very easy for us to work with dates and the time differences between them. Let's get started:

1. Load the prepared project from the `Lesson3/Activity02` folder and configure the Current Builder for the project to be **CMake Build (Portable)**. Build and configure the launcher and run the unit tests. We recommend that the name that's used for the tests runner is `L3A2datetests`. The project has dummy files and one failing test.

2. Open the `date.hpp` file in the editor and add the following lines inside the basic `Date` class to allow access to the stored values:

```
int Day()   const {return m_day;}
int Month() const {return m_month;}
int Year()  const {return m_year;}
```

3. Open the `dateTests.cpp` file and add the following code to the `DateTest` class:

```
void VerifyDate(const Date& dt, int yearExp, int monthExp, int dayExp) const
{
    ASSERT_EQ(dayExp, dt.Day());
    ASSERT_EQ(monthExp, dt.Month());
    ASSERT_EQ(yearExp, dt.Year());
}
```

Normally, you would refactor this test as the tests develop, but we will pull it out up front.

4. Replace `ASSERT_FALSE()` in the existing test with the following test:

```
Date dt;
VerifyDate(dt, 1970, 1, 1);
```

5. Rebuild and run the tests – they should now all pass.

6. Add the following test:

```
TEST_F(DateTest, Constructor1970Jan2)
{
    Date dt(2, 1, 1970);
    VerifyDate(dt, 1970, 1, 2);
}
```

7. To make this test we need to add the following two constructors to the `Date` class:

```
Date() = default;
Date(int day, int month, int year) :
        m_year{year}, m_month{month}, m_day{day}
{
}
```

8. We now need to introduce the functions to convert to/from the `date_t` type. Add the following alias to the `date.hpp` file inside our namespace:

```
using date_t = int64_t;
```

9. To the `Date` class, add the declaration of the following method:

```
date_t ToDateT() const;
```

10. Then, add the following test:

```
TEST_F(DateTest, ToDateTDefaultIsZero)
{
    Date dt;
    ASSERT_EQ(0, dt.ToDateT());
}
```

11. As we are doing **TDD**, we add the minimal implementation of the method to pass the test.

```
date_t Date::ToDateT() const
{
    return 0;
}
```

12. Now, we add the next test:

```
TEST_F(DateTest, ToDateT1970Jan2Is1)
{
    Date dt(2, 1, 1970);
    ASSERT_EQ(1, dt.ToDateT());
}
```

13. We continue to add one more test and then another, all the time refining the algorithm in `ToDateT()` firstly to deal with dates in **1970**, then **1-Jan-1971**, and then a date in **1973**, which means we span one leap year, and so on. The full set of tests that are used to develop the `ToDateT()` method are as follows:

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

14. To pass all of these tests, we add the following items to the declaration of the `Date` class:

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

15. The implementation of `ToDateT()` and the supporting methods in `date.cpp` is as follows:

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

16. Now that `ToDateT()` is working, we turn to its inverse, that is, `FromDateT()`. Again, we build up the tests one at a time to develop the algorithm over a range of dates. The following tests were used:

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
