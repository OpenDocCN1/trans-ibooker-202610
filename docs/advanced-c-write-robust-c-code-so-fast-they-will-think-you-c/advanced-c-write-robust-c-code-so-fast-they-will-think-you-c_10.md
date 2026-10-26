## 第 2A 章 - 不允许鸭子 – 类型和推导

### 活动 1：图形处理

在此活动中，我们将实现两个类（**Point3d** 和 **Matrix3d**），以及乘法运算符，以便我们可以平移、缩放和旋转点。我们还将实现一些创建变换所需矩阵的辅助方法。按照以下步骤实现此活动：

1. 从 **Lesson2A/Activity01** 文件夹加载准备好的项目并将项目的 Current Builder 配置为 **CMake Build (Portable)**。构建并配置启动器并运行单元测试（测试失败）。建议用于测试运行器的名称为 **L2AA1graphicstests**。

> **CMake 配置**
> 
> 按照 *练习 1，声明变量和探索大小* 的第 9 步将项目配置为 CMake 项目。

2. 添加对 **Point3d** 类的测试，以验证默认构造函数是否创建了 **原点 [0, 0, 0, 1]**。

3. 打开 **point3dTests.cpp** 文件并在顶部添加以下行。

4. 用以下测试替换失败的现有测试：

```
TEST_F(Point3dTest, DefaultConstructorIsOrigin)
{
    Point3d pt;
    float expected[4] = {0,0,0,1};

    for(size_t i=0 ; i < 4 ; i++)
    {
        ASSERT_NEAR(expected[i], pt(i), Epsilon) << "cell [" << i << "]";
    }
}
```

此测试要求我们编写一个访问运算符。





13. 现在是时候通过将验证循环移动到 Point3dTest 类中的模板函数来重构测试用例了。在该类中添加以下模板：

```
template<size_t size>
void VerifyPoint(Point3d& pt, float (&expected)[size])
{
    for(size_t i=0 ; i< size ; i++)
    {
        ASSERT_NEAR(expected[i], pt(i), Epsilon) << "cell [" << i << "]";
    }
}
```

14. 这意味着最后一个测试可以重写如下：

```
TEST_F(Point3dTest, InitListConstructor4)
{
    Point3d pt {5.2, 3.5, 6.7, 2.0};
    float expected[4] = {5.2,3.5,6.7,2.0};
    VerifyPoint(pt, expected);
}
```

与生产代码一样，保持测试的可读性也很重要。

15. 接下来，通过以下测试添加对相等和不相等运算符的支持：

```
TEST_F(Point3dTest, EqualityOperatorEqual)
{
    Point3d pt1 {1,3,5};
    Point3d pt2 {1,3,5};
    ASSERT_EQ(pt1, pt2);
}
```

```
TEST_F(Point3dTest, EqualityOperatorNotEqual)
{
    Point3d pt1 {1,2,3};
    Point3d pt2 {1,2,4};
    ASSERT_NE(pt1, pt2);
}
```

29. 将此定义添加到 **matrix3d.cpp**：

```
#include "matrix3d.hpp"

Matrix3d::Matrix3d()
{
    for (int i{0} ; i< 4 ; i++)
        for (int j{0} ; j< 4 ; j++)
            m_data[i][j] = (i==j);
}
```

测试现在可以构建并通过。

30. 稍微重构代码以使其更具可读性。将头文件修改为如下所示：

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

31. 更新 **matrix3d.cpp** 文件以使用常量：

```
Matrix3d::Matrix3d()
{
    for (int i{0} ; i< NumberRows ; i++)
        for (int j{0} ; j< NumberColumns ; j++)
            m_data[i][j] = (i==j);
}
```

40. 将实现添加到 matrix3d.cpp 文件：

```
Matrix3d& Matrix3d::operator*=(const Matrix3d& rhs)
{
    Matrix3d temp;

    for(int i=0 ; i<NumberRows ; i++)
        for(int j=0 ; j<NumberColumns ; j++)
        {
            temp.m_data[i][j] = 0;
            for (int k=0 ; k<NumberRows ; k++)
                temp.m_data[i][j] += m_data[i][k] * rhs.m_data[k][j];
        }

    *this = temp;
    return *this;
}
```

41. 构建并运行测试 – 再次，它们应该通过。

42. 通过在 Matrix3dTest 类中声明它，向测试类引入第二个辅助函数：

```
void VerifyMatrixIsIdentity(Matrix3d& mat);
```

然后，定义它以便我们可以使用它：

```
void Matrix3dTest::VerifyMatrixIsIdentity(Matrix3d& mat)
{
for( int row{0} ; row<4 ; row++)
    for( int col{0} ; col<4 ; col++)
    {
        int expected = (row==col) ? 1 : 0;
        ASSERT_FLOAT_EQ(expected, mat(row,col))
            << "cell[" << row << "][" << col << "]";
    }
}
```

43. 更新一个测试以使用它：

```
TEST_F(Matrix3dTest, DefaultConstructorIsIdentity)
{
    Matrix3d mat;
```

```
pt(row) = sum;
}
    return pt;
}
```

49. 构建并运行测试。它们都应该再次通过。

50. 在 matrix3dtests.cpp 的顶部，添加包含文件：

```
#include <cmath>
```

51. 开始添加变换矩阵工厂方法。使用以下测试，我们将开发各种工厂方法（测试应一次添加一个）：

```
TEST_F(Matrix3dTest, CreateTranslateIsCorrect)
{
    Matrix3d mat = createTranslationMatrix(-0.5, 2.5, 10.0);
    Matrix3d expected {{1.0, 0.0, 0.0, -0.5},
                        {0.0, 1.0, 0.0, 2.5},
                        {0.0, 0.0, 1.0, 10.0},
                        {0.0, 0.0, 0.0, 1.0}
    };
    VerifyMatrixResult(expected, mat);
}
```

```
TEST_F(Matrix3dTest, CreateScaleIsCorrect)
{
    Matrix3d mat = createScaleMatrix(3.0, 2.5, 11.0);
    Matrix3d expected {{3.0, 0.0, 0.0, 0.0},
                        {0.0, 2.5, 0.0, 0.0},
                        {0.0, 0.0, 11.0, 0.0},
                        {0.0, 0.0, 0.0, 1.0}
    };
    VerifyMatrixResult(expected, mat);
}
```

```
TEST_F(Matrix3dTest, CreateRotateX90IsCorrect)
{
    Matrix3d mat = createRotationMatrixAboutX(90.0F);
    Matrix3d expected {{1.0, 0.0, 0.0, 0.0},
                        {0.0, 0.0, -1.0, 0.0},
```

```
matrix(2, 3) = dz;
    return matrix;
}

Matrix3d createScaleMatrix(float sx, float sy, float sz)
{
    Matrix3d matrix;
    matrix(0, 0) = sx;
    matrix(1, 1) = sy;
    matrix(2, 2) = sz;
    return matrix;
}

Matrix3d createRotationMatrixAboutX(float degrees)
{
    Matrix3d matrix;
    double pi(4.0F*atan(1.0F));
    double radians = degrees / 180.0 * pi;

    float cos_theta = static_cast<float>(cos(radians));
    float sin_theta = static_cast<float>(sin(radians));

    matrix(1, 1) =  cos_theta;
    matrix(2, 2) =  cos_theta;
    matrix(1, 2) = -sin_theta;
    matrix(2, 1) =  sin_theta;

    return matrix;
}

Matrix3d createRotationMatrixAboutY(float degrees)
{
    Matrix3d matrix;
    double pi(4.0F*atan(1.0F));
    double radians = degrees / 180.0 * pi;

    float cos_theta = static_cast<float>(cos(radians));
    float sin_theta = static_cast<float>(sin(radians));

    matrix(0, 0) =  cos_theta;
    matrix(2, 2) =  cos_theta;
    matrix(0, 2) = sin_theta;
    matrix(2, 0) = -sin_theta;

    return matrix;
}
```

```
Matrix3d createRotationMatrixAboutZ(float degrees)
{
    Matrix3d matrix;
    double pi{4.0F*atan(1.0F)};
    double radians = degrees / 180.0 * pi;
    float cos_theta = static_cast<float>(cos(radians));
    float sin_theta = static_cast<float>(sin(radians));

    matrix(0, 0) = cos_theta;
    matrix(1, 1) = cos_theta;
    matrix(0, 1) = -sin_theta;
    matrix(1, 0) = sin_theta;

    return matrix;
}
```

55. 为了使此代码能够编译并通过测试，我们需要在 Matrix3d 的声明中添加一个额外的访问器：

```
float& operator()(const int row, const int column)
{
    return m_data[row][column];
}
```

56. 再次构建并运行所有测试以显示它们都通过。

57. 在 point3d.hpp 中，添加对 <ostream> 的包含，并在 Point3d 类末尾添加以下友元声明：

```
friend std::ostream& operator<<(std::ostream& , const Point3d& );
```

58. 在类之后编写 operator 的内联实现：

```
inline std::ostream&
operator<<(std::ostream& os, const Point3d& pt)
{
    const char* sep = "[ ";
    for(auto value : pt.m_data)
    {
        os << sep << value;
        sep = ", ";
    }
    os << " ]";
    return os;
}
```

59. 打开 **main.cpp** 文件并从以下行中删除注释分隔符 //：

```
//#define ACTIVITY1
```

60. 构建并运行名为 **graphics** 的应用程序 – 你需要创建一个新的运行配置。如果你的 **Point3d** 和 **Matrix3d** 实现正确，则程序将显示以下输出：

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

Figure 2A.53: Successfully running the activity program

在本活动中，我们实现了两个类，它们构成了实现 3D 图形渲染所需的所有操作的基础。我们使用运算符重载来实现这一点，以便 Matrix3d 和 Point3d 可以像原生类型一样使用。这可以很容易地扩展以处理点向量，如果我们希望操作整个对象，则需要这样做。




