```
pt(row) = sum;
}
    return pt;
}
```

49. Build and run the tests. They should all be passing again.

50. At the top of matrix3dtests.cpp, add the include file:

```
#include <cmath>
```

51. Start adding the transformation matrix factory methods. Using the following tests, we will develop the various factory methods (the tests should be added one at a time):

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