
## Activity 1: Adding a Source-Header File Pair

```cpp
CxxApplication::CxxApplication( int argc, char *argv[] ) {

  std::cout << "Hello CMake." << std::endl;

  ANewClass anew;

  anew.run();

  std::cout << sum(3, 4) << std::endl; // add this line

}
```

### Note

The complete code of this file can be found here: [https://github.com/TrainingByPackt/Advanced-CPlusPlus/blob/master/Lesson1/Activity01/src/CxxTemplate.cpp](https://github.com/TrainingByPackt/Advanced-CPlusPlus/blob/master/Lesson1/Activity01/src/CxxTemplate.cpp).

Here, we added a line in which we call the `sum` function with `3` and `4` and print the result to the console.

7. Build and run the project (`Project` | `Build All` | `Run` | `Run`). The output you see should be as follows:

![Figure 1.57: The output](image/C14583_01_57.jpg)

*Figure 1.57: The output*

With this activity, you practiced adding a new source-header file pair to your project. These file pairs are a very common pattern in C++ development. They can host global functions such as the ones we had in this activity. More commonly, they host classes and their definitions. Throughout your development effort, you will be adding many more source-header file pairs to your application. Therefore, it is important to get used to adding them and not dragging your feet, which would result in large monolithic files that are difficult to maintain and test.

## Activity 2: Adding a New Class and Its Test

In this activity, we will add a new class that simulates `1D` linear motion. The class will have `double` fields for `position` and `velocity`. It will also have an `advanceTimeBy()` method, which receives a `double` `dt` parameter, which modifies `position` based on the value of `velocity`. Use `EXPECT_DOUBLE_EQ` instead of `EXPECT_EQ` for `double` values. In this activity, we will add a new class and its test to the project. Follow these steps to perform this activity:

1. Open the Eclipse IDE with our existing project. To create a new class, right-click the `src` folder in the `Project Explorer` pane and select `New` | `Class`. Type `LinearMotion1D` as the name and create the class.

2. Open the `LinearMotion1D.h` file that we created in the previous step. Add the `position` and `velocity` `double` fields into it. Also, add the forward reference to the `advanceTimeBy` method, which takes a `double` `dt` variable as a parameter. The constructors and destructors were already in the class. The following is the end result of these changes in `LinearMotion1D.h`:

```cpp
#ifndef SRC_LINEARMOTION1D_H_
#define SRC_LINEARMOTION1D_H_

class LinearMotion1D {

public:

  double position;

  double velocity;

  void advanceTimeBy(double dt);

  LinearMotion1D();

  virtual ~LinearMotion1D();

};

#endif /* SRC_LINEARMOTION1D_H_ */
```

3. Now open `LinearMotion1D.cpp` and add the implementation for the `advanceTimeBy` method. Our `velocity` is a field in our class and the time difference is a parameter to this method. A change in `position` is equal to the `velocity` multiplied by the time change, so we calculate the result and add it to the `position` variable. We also use the existing constructor code to initialize `position` and `velocity` to 0. The following is the end result of these changes in `LinearMotion1D.cpp`:

```cpp
#include "LinearMotion1D.h"

void LinearMotion1D::advanceTimeBy(double dt) {

  position += velocity * dt;

}

LinearMotion1D::LinearMotion1D() {

  position = 0;

  velocity = 0;

}

LinearMotion1D::~LinearMotion1D() {

}
```

4. Create a test for this class. Right-click the `tests` folder and select `New` | `Source File`. Type `LinearMotion1DTest.cpp` as the name and create it.

5. Now open `LinearMotion1DTest.cpp`. Create two tests for motions in two different directions, left and right. For each of them, create a `LinearMotion1D` object, initialize its `position` and `velocity`, and call `advanceTimeBy` to actually have the motion happen. Then, check whether it moved to the same location that we expected. The following is the end result of these changes in `LinearMotion1DTest.cpp`:

```cpp
#include "gtest/gtest.h"

#include "../src/LinearMotion1D.h"

namespace {

class LinearMotion1DTest: public ::testing::Test {};

TEST_F(LinearMotion1DTest, CanMoveRight) {

  LinearMotion1D l;

  l.position = 10;

  l.velocity = 2;

  l.advanceTimeBy(3);

  EXPECT_DOUBLE_EQ(16, l.position);

}

TEST_F(LinearMotion1DTest, CanMoveLeft) {

  LinearMotion1D l;

  l.position = 10;

  l.velocity = -2;

  l.advanceTimeBy(3);

  EXPECT_DOUBLE_EQ(4, l.position);

}

}
```

6. Now modify our CMake configuration files so that these source files that we generated are also used. For the `LinearMotion1D` class, add its `.cpp` file as an executable so that it is compiled and linked together with the other source files. Here is what the `add_executable` section of `CMakeLists.txt` becomes:

```cmake
add_executable(CxxTemplate

  src/CxxTemplate.cpp

  src/ANewClass.cpp

  src/SumFunc.cpp

  src/LinearMotion1D.cpp # added

)
```

7. For the tests that we just created, edit `tests/CMakeLists.txt`. There, we need to add both the test source file, `LinearMotion1DTest.cpp`, and the source file of the class that it uses, `LinearMotion1D.cpp`. Since they are in different directories, access them as `../src/LinearMotion1D.cpp`. Here is what the `add_executable` section of `tests/CMakeLists.txt` becomes:

```cmake
add_executable(tests

  CanTest.cpp

  SumFuncTest.cpp

  ../src/SumFunc.cpp

  LinearMotion1DTest.cpp # added

  ../src/LinearMotion1D.cpp # added

)
```

8. Build the project and run the tests. We will see that all the tests are successful:

![Figure 1.58: All tests are successful](image/C14583_01_58.jpg)

*Figure 1.58: All tests are successful*

With this activity, you performed the task of adding a new class and its test to the project. You created a class that simulates one-dimensional motion and you wrote unit tests to ensure that it is working properly.

## Activity 3: Making Code More Readable

In this activity, you will practice improving the quality of a given code. Follow these steps to implement this activity:

1. Open Eclipse CDT and create a class in a source-header file pair in Eclipse. To do this, right-click the `src` folder in `Project Explorer`. Select `New` | `Class` from the pop-up menu.

2. Type `SpeedCalculator` as the header filename and click `Finish`. It will create two files: `SpeedCalculator.h` and `SpeedCalculator.cpp`. We provided the code for both files above. Add the code that was provided for each file.

3. Now we need to add the class to the CMake project. Open the `CMakeLists.txt` file in the root of your project (outside the `src` folder) and make the following change in the file:

```cmake
  src/LinearMotion1D.cpp

  src/SpeedCalculator.cpp # add this line

)
```

4. Now select `File` | `Save All` to save all the files and build the project by selecting `Project` | `Build All`. Make sure there are no errors.

5. Create an instance of the `SpeedCalculator` class in our `main()` function and call its `run()` method. Open `CxxTemplate.cpp` and include our new class, then edit the `main` function by adding the following code:

```cpp
#include "SpeedCalculator.h"

int main( int argc, char *argv[] ) {

  cxxt::CxxApplication app( argc, argv );

  // add these three lines

  SpeedCalculator speedCalculator;

  speedCalculator.initializeData(10);

  speedCalculator.calculateAndPrintSpeedData();

  return 0;

}
```

6. To fix the style, simply use `Source` | `Format` and choose to format the entire file. Luckily, the variable names do not have any problems.
