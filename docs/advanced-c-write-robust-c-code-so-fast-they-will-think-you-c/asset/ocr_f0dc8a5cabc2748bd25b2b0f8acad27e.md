29. Add this definition to **matrix3d.cpp**:

```
#include "matrix3d.hpp"

Matrix3d::Matrix3d()
{
    for (int i{0} ; i< 4 ; i++)
        for (int j{0} ; j< 4 ; j++)
            m_data[i][j] = (i==j);
}
```

The tests now build and pass.

30. Refactor the code slightly to make it more readable. Modify the header to read like so:

```
#include <cstddef>  // Required for size_t definition

class Matrix3d
{
public:
    static constexpr size_t NumberRows{4};
    static constexpr size_t NumberColumns{4};

    Matrix3d();

    float operator()(const int row, const int column) const
    {
    return m_data[row][column];
    }

private:
    float m_data[NumberRows][NumberColumns];
};
```

31. Update the **matrix3d.cpp** file to use the constants:

```
Matrix3d::Matrix3d()
{
    for (int i{0} ; i< NumberRows ; i++)
        for (int j{0} ; j< NumberColumns ; j++)
            m_data[i][j] = (i==j);
}
```