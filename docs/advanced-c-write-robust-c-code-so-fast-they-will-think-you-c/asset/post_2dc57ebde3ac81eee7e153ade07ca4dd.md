
## Chapter 2B: No Ducks Allowed – Templates and Deduction

### Activity 1: Developing a Generic "contains" Template Function

In this activity, we will implement several helper classes that will be used to detect the `std::string` class case and the `std::set` case and then use them to tailor the contains function to the particular container. Follow these steps to implement this activity:

1. Load the prepared project from the `Lesson2B/Activity01` folder. Build and configure the launcher and run the unit tests (which fail the one dummy test). We recommend that the name used for the test runner is `L2BA1tests`.

2. Open the `containsTests.cpp` file and replace the existing test with the following:

```
TEST_F(containsTest, DetectNpos)
{
    ASSERT_TRUE(has_npos_v<std::string>);
    ASSERT_FALSE(has_npos_v<std::set<int>>);
    ASSERT_FALSE(has_npos_v<std::vector<int>>);
}
```

This test requires us to write a set of helper templates to detect if the container class supports a static member variable called `npos`.

3. Add the following code to the `contains.hpp` file:

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

The tests now run and pass.

4. Add the following tests to the `containsTests.cpp` file:

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

The tests now run and pass.

12. Build and run the application called `contains`. Create a new Run Configuration. If your implementation of the `contains` template is correct, then the program will display the following output:

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

Figure 2B.36: Output from the successful implementation of contains

In this activity, we used various templating techniques in conjunction with SFINAE to select the appropriate implementation of a `contains()` function based upon the capability of the containing class. We could have achieved the same result using a generic template function and some specialized templates, but we took the path less travelled and flexed our newly found template skills.

10. After recompiling, the next error from the compiler refers to the following inline function:

```
inline Matrix3d operator*(const Matrix3d& lhs, const Matrix3d& rhs)
{
    Matrix3d temp(lhs);  // <=== compiler error - ill-formed copy constructor
    temp *= rhs;
    return temp;
}
```

11. Whereas before, the default copy constructor was sufficient for our purposes, it just did a shallow copy of all the elements of the array and that was correct. We now have indirection to the data we need to copy and so we need to implement a deep copy constructor and copy assignment. We will also need to address the existing constructors. For now, just add the constructor declarations to the class (adjacent to the other constructors):

```
Matrix3d(const Matrix3d& rhs);
Matrix3d& operator=(const Matrix3d& rhs);
```

Attempting to build the tests will now show that we have resolved all the issues in the header file, and that we can move onto the implementation file.

12. Modify the two constructors to initialize `unique_ptr` as follows:

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

16. With these changes in place, we have fixed all the compiler errors, but now we have a linker error to deal with – the copy constructor that we only declared back in step 11.

17. In the `matrix3d.cpp` file add the following definitions:

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

18. The tests will now build and all of them will pass. The next step is to force a move constructor. Locate the `createTranslationMatrix()` method in `matrix3d.cpp` and change the return statement as follows:

```
return std::move(matrix);
```

19. In `matrix3d.hpp` declare the move constructor.

```
Matrix3d(Matrix3d&& rhs);
```

20. Rebuild the tests. Now, we get an error related to the move constructor not being present.

21. Add the implementation of the constructor into `matrix3d.cpp` and rebuild the tests.

```
Matrix3d::Matrix3d(Matrix3d&& rhs)
{
    //std::cerr << "Matrix3d::Matrix3d(Matrix3d&& rhs)\n";
    std::swap(m_data, rhs.m_data);
}
```

22. Rebuild and run the tests. They all pass again.

23. Just to confirm that the move constructor is being called, add `#include <iostream>` to `matrix3d.cpp`, remove the comment from the output line in the move constructor, and re-run the test. It will report an error after the tests have completed because we sent it to the standard error channel (`cerr`). After the check, make the line a comment again.

> **Note**
> Just a quick note about the move constructor – we did not explicitly initialize `m_data` like we did for the other constructors. This means that it will be initialized as empty and then swapped with the parameter that is passed in, which is a temporary and so it is acceptable for it to not hold an array after the transaction – it removes one allocation and deallocation of memory.

24. Now let's convert the `Point3d` class so that it can use heap allocated memory. In the `point3d.hpp` file, add an `#include <memory>` line so that we have access to the `unique_ptr<>` template.

25. Next, change the type of the declaration for `m_data` to be like so:

```
std::unique_ptr<float[]> m_data;
```

26. The compiler now tells us that we have a problem with the insertion operator (`<<`) in `point3d.hpp` because we can't use a ranged-for on `unique_ptr`: Replace the implementation with the following:

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

27. Open `point3d.cpp` and modify the default constructor to initialize the `unique_ptr` and change the initialization loop since a ranged for cannot be used on the `unique_ptr`:

```
Point3d::Point3d() : m_data{new float[NumberRows]}
{
    for (int i{0}; i < NumberRows - 1; i++) {
        m_data[i] = 0;
    }
    m_data[NumberRows - 1] = 1;
}
```

28. Modify the other constructor by initializing the `unique_ptr`:

```
Point3d::Point3d(std::initializer_list<float> list)
    : m_data{new float[NumberRows]}
```

29. Now all the tests run and pass, like they did previously.

30. Now, if we run the original application, `L3graphics`, then the output will be identical to the original, but the implementation uses RAII to allocate and manage the memory that's used for the matrices and points.

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

Figure 3.52: Activity 1 output after successful conversion to use RAII
