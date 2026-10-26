
12. Click on the Run button to rerun the tests – this time, we have added two more tests that fail – AddFractions and AddFractions2:

![](img/6507d435e2edfa3c00c220d0ccfcaf94_151_0.png)

Figure 2A.42: Additional tests to fail

13. Locate the operator+= function in the Fraction.cpp file.

14. Make the necessary changes to the function and click on the Run button to rerun the tests until the tests pass. Look at the equations that define its operation given earlier and see how operator*=( ) was implemented.

15. Open Fraction.hpp in the editor and change the line near the top of the file to read like so:

```
#define EXERCISE7_STEP 15
```

16. Click on the Run button to rerun the tests – this time, we have added two more tests that fail – SubtractFractions and SubtractFractions2.

17. Locate the operator-= function in the Fraction.cpp file.

18. Make the necessary changes to the function and click on the Run button to rerun the tests until the tests pass.

19. Open Fraction.hpp in the editor and change the line near the top of the file to read like so:

```
#define EXERCISE7_STEP 19
```

20. Click on the Run button to rerun the tests – this time, we have added two more tests that fail – DivideFractions and DivideFractions2.

21. Locate the **operator/=** function in the **Fraction.cpp** file.

22. Make the necessary changes to the function and click on the **Run** button to rerun the tests until the tests pass.

23. Open **Fraction.hpp** in the editor and change the line near the top of the file to read like so:

```
#define EXERCISE7_STEP 23
```

24. Click on the **Run** button to rerun the tests – this time, we have added one more test that fails – InsertOperator.

25. Locate the **operator<<** function in the Fraction.hpp file.

26. Make the necessary changes to the function and click on the **Run** button to rerun the tests until the tests pass.

27. From the **Launch Configuration**, select **Exercise7** and click on the **Run** button. This will produce the following output:

![](img/6507d435e2edfa3c00c220d0ccfcaf94_152_0.png)

Figure 2A.43: Functional Fraction class

This completes our implementation of the **Fraction** class for now. We will return to it again when we consider exceptions in Chapter 3, *The Distance Between Can and Should – Objects, Pointers, and Inheritance*, so that we can deal with illegal values in our fractions (denominators of 0).

## Function Overloading

C++ supports a feature known as function overloading, which is where two or more functions have the same name, but their argument list is different. The number of arguments can be the same, but at least one of the argument types has to be different. Alternatively, they may have a different number of arguments. So, the function prototype for the multiple functions are different. However, two functions cannot have the same function name, the same argument types, and a different return type. The following is an example of overloading:

```
std::ostream& print(std::ostream& os, int value) {
    os << value << " is an int\n";
    return os;
}

std::ostream& print(std::ostream& os, float value) {
    os << value << " is a single precision float\n";
    return os;
}

std::ostream& print(std::ostream& os, double value) {
    os << value << " is a double precision float \n";
    return os;
}

// The next function causes the compiler to generate an error
// as it only differs by return type.
void print(std::ostream& os, double value) {
    os << value << " is a double precision float!\n";
}
```

So far, the multiple constructors on **Fraction** and the overloaded arithmetic operators are all examples of overloaded functions that the compiler has to refer to when it meets one of these functions. Consider the following code:

```
int main(int argc, char** argv) {
    print(42);
}
```

When the compiler meets the line **print(42)**, it needs to work out which of the previously defined functions to call, so it performs the following process (grossly simplified):

![](img/6507d435e2edfa3c00c220d0ccfcaf94_154_0.png)

Figure 2A.44: Function overload resolution (simplified)

The C++ standard defines rules for how the compiler determines the best candidate function based on how it has to manipulate (that is, convert) the arguments to get a match. If no conversions are required, then that function is the best match.

## Classes, Structs, and Unions

When you define a class and do not specify an access modifier (public, protected, private), all the members will be private by default:

```
class Fraction
{
    Fraction() {};        // All of these are private
    int m_numerator;
    int m_denominator;
};
```

## Activity 1: Graphics Processing

In a modern computing environment, matrices are used everywhere to solve various problems – to solve simultaneous equations, to analyze electricity power grids or electric circuits, to perform manipulations on objects for graphics rendering, and to deliver implementations of machine learning. In the world of graphics, whether two-dimensional (2D) or three-dimensional (3D), all the operations that you would want to perform on your objects can be done with the help of matrix multiplication. Your team has been tasked with developing a representation of points, transformation matrices, and the operations that you might want to perform on them. Follow these steps to achieve this:

- 1. Load the prepared project from the **Lesson2A/Activity01** folder.
- 2. Create a class called **Point3d** that can be constructed by default to be the origin, or using an initializer list of three or four values (data is stored directly in the class).
- 3. Create a class called **Matrix3d** that can be constructed by default to an identity matrix, or using a nested initializer list to provide all the values (data is stored directly in the class).
- 4. On **Point3d**, overload **operator()** so that it takes the **(index)** argument in order to return the value at **x(0), y(1), z(2), and w(3)**.
- 5. On **Matrix3d**, overload **operator()** to take **(row, col)** arguments so that it returns the value.
- 6. Add the unit tests to verify all of the aforementioned features.
- 7. Add **operator*=(const Matrix3d&)** and **operator==(const Matrix3d&)** to the **Matrix3d** class, as well as the unit tests for them.
- 8. Add free functions for multiplying two **Matrix3d** objects and a **Matrix3d** object by a **Point3d** object with unit tests.
- 9. Add standalone methods for creating a matrix to translate, scale, and rotate (around the x, y, z axes) and their unit tests.

After implementing the preceding steps, the expected output is as follows:

![](img/6507d435e2edfa3c00c220d0ccfcaf94_157_0.png)

Figure 2A.45: Successfully running the activity program

For the purpose of this activity, we will not be worrying about the possibility of out of range on indices. We will talk about this in *Chapter 3, The Distance Between Can and Should – Objects, Pointers, and Inheritance*. An identity matrix is a square matrix (4 x 4, in our case) where all the values on the diagonal are set to one (1) and all the other values are zero (0).

When working with 3D graphics, we use an augmented matrix for the points (vertices) and for the transformations so that all the transformations (translation, scaling, rotation) can be achieved by using multiplication.

An **n × m** matrix is an array of n rows of m numbers. For example, a **2 x 3** matrix could look like this:

![](img/6507d435e2edfa3c00c220d0ccfcaf94_157_1.png)

Figure 2A.46: Matrix of 2x3

A vertex in three dimensions can be represented as a **three-tuple (x, y, z)**. However, we augment it with another ordinate, w (**=1 for a vertex, =0 for a direction**), making it a **four-tuple (x, y, z, 1)**. Rather than using a tuple, we place it in a **4 x 1** matrix, as follows:

![](img/6507d435e2edfa3c00c220d0ccfcaf94_157_2.png)

Figure 2A.47: 4x1 Matrix
