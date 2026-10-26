
Now, let's see how we can make this code simpler and easier to understand. The preceding code is doing these things: calculating the sum of the input elements, finding the smallest one, determining whether the top threshold was crossed, and multiplying each element by two.

- 7. Since all of these are done in the same loop, the algorithm is not very clear now. Fix that and have four separate loops:

```cpp
void arrayPlay(int *input, int inputLength) {

    // find the sum of the input
    int sum = 0;
    for (int i = 0; i < inputLength; ++i) {
        sum += input[i];
    }

    // find the smallest element
    int smallest = 0;
    for (int i = 0; i < inputLength; ++i) {
        if (i == 0 || smallest > input[i]) {
            smallest = input[i];
        }
    }

    // determine whether top threshold is crossed
    int topThreshold = 100;
    bool isTopThresholdCrossed = true;
    for (int i = 0; i < inputLength; ++i) {
        if (input[i] >= topThreshold) {
            isTopThresholdCrossed = true;
        }
    }

    // multiply each element by 2
    for (int i = 0; i < inputLength; ++i) {
        if (input[i] < 0) {
            input[i] *= 2;
        }
    }
}
```

Now the code is much clearer. While it's very easy to understand what each block is doing, we also added comments to make it even more clear. In this section, we gained a better understanding of how our code is converted to executables. Then, we discussed ways of identifying and resolving possible errors with our code. We finalized this with a discussion about how to write readable code that is less likely to have problems. In the next section, we will solve an activity wherein we will be making code more readable.

## Activity 3: Making Code More Readable

You may have code that is unreadable and contains bugs, either because you wrote it in a hurry, or you received it from someone else. You want to change the code to eliminate its bugs and to make it more readable. We have a piece of code that needs to be improved. Improve it step by step and resolve the issues using a debugger. Perform the following steps to implement this activity:

- 1. Below you will find the source for **SpeedCalculator.cpp** and **SpeedCalculator.h**. They contain the **SpeedCalculator** class. Add these two files to your project.
- 2. Create an instance of this class in your **main()** function and call its **run()** method.
- 3. Fix the style and naming problems in the code.
- 4. Simplify the code to make it more understandable.
- 5. Run the code and observe the problem at runtime.
- 6. Use the debugger to fix the problem.

Here's the code for **SpeedCalculator.cpp** and **SpeedCalculator.h** that you will add to your project. You will modify them as a part of this activity:

```cpp
// SpeedCalculator.h
#ifndef SRC_SPEEDCALCULATOR_H_
#define SRC_SPEEDCALCULATOR_H_

class SpeedCalculator {
private:
    int numEntries;
    double *positions;
    double *timesInSeconds;
    double *speeds;
public:
    void initializeData(int numEntries);
    void calculateAndPrintSpeedData();
};

#endif /* SRC_SPEEDCALCULATOR_H_ */

//SpeedCalculator.cpp
#include "SpeedCalculator.h"
#include <cstdlib>
#include <ctime>
#include <iostream>
#include <cassert>

void SpeedCalculator::initializeData(int numEntries) {
    this->numEntries = numEntries;
    positions = new double[numEntries];
    timesInSeconds = new double[numEntries];
    srand(time(NULL));
    timesInSeconds[0] = 0.0;
    positions[0] = 0.0;
    for (int i = 0; i < numEntries; ++i) {
        positions[i] = positions[i-1] + (rand()%500);
        timesInSeconds[i] = timesInSeconds[i-1] + ((rand()%10) + 1);
    }
}

void SpeedCalculator::calculateAndPrintSpeedData() {
    double maxSpeed = 0;
    double minSpeed = 0;
    double speedLimit = 100;
    double limitCrossDuration = 0;

    for (int i = 0; i < numEntries; ++i) {
        double dt = timesInSeconds[i+1] - timesInSeconds[i];
        assert (dt > 0);
        double speed = (positions[i+1] - positions[i]) / dt;
        if (maxSpeed < speed) {
            maxSpeed = speed;
        }
        if (minSpeed > speed) {
            minSpeed = speed;
        }
        if (speed > speedLimit) {
            limitCrossDuration += dt;
        }
        speeds[i] = speed;
    }

    std::cout << "Max speed: " << maxSpeed << std::endl;
    std::cout << "Min speed: " << minSpeed << std::endl;
    std::cout << "Total duration: " <<
timesInSeconds[numEntries - 1] - timesInSeconds[0] << " seconds" <<
std::endl;
    std::cout << "Crossed the speed limit for " << limitCrossDuration << " seconds" << std::endl;
    delete[] speeds;
}
```

Note

The solution for this activity can be found on page 626.

## Summary

In this chapter, we learned how to create C++ projects that are portable and maintainable. We first learned how to create CMake projects and how to import them to Eclipse CDT, giving us the choice to use the command line or an IDE. The rest of the chapter focused on eliminating various problems in our projects. First, we learned how to add unit tests to a project and how to use them to ensure that our code works as intended. We continued this with a discussion about the compilation and linking steps that our code goes through and observed the contents of object files to gain a better understanding of executable files. Then, we learned how to debug our code visually in the IDE to eliminate runtime errors. We finished this discussion with a number of rules of thumb that help create readable, understandable, and maintainable code. These methods will come in handy on your C++ journey. In the next chapter, we will learn more about C++'s type system and templates.

## Introduction

C++ is a strongly typed, statically typed language. The compiler uses type information related to the variables that are used and the context in which they are used to detect and prevent certain classes of programming errors. This means that every object has a type and that type does not change, ever. In contrast, dynamically typed languages such as Python and PHP defer this type checking until runtime (also known as late binding), and the type of a variable may change during the execution of the application. These languages use the duck test instead of the variable's type – that is, "if it walks and talks like a duck, then it must be a duck." Statically typed languages such as C++ rely on the type to determine whether a variable can be used for a given purpose, while dynamically typed languages rely on the presence of certain methods and properties to determine its suitability.

C++ was originally described as "C with classes". What does this mean? Basically, C provided a set of built-in fundamental types – int, float, char, and so on – along with pointers and arrays of these items. You aggregate these into data structures of related items using a struct. C++ extends this to classes so that you can define your own types completely with the operators that can be used to manipulate them, thereby making them first-class citizens in the language. Since its humble beginnings, C++ has evolved to be way more than just "C with classes", as it can now express the object-oriented paradigm (encapsulation, polymorphism, abstraction, and inheritance), the functional paradigm, and generic programming (templates).

In this book, we will focus on what it means for C++ to support the object-oriented paradigm. As your experience as a developer grows and you are exposed to languages such as Clojure, Haskell, Lisp, and other functional languages, they will help you to write robust C++ code. Dynamically typed languages such as Python, PHP, and Ruby have already influenced the way we write C++ code. With the arrival of C++17 came the introduction of the std::variant class – a class that holds whatever type that we choose (at compile time) and acts a lot like variables in dynamic languages.

In the previous chapter, we learned how to create C++ projects that are portable and maintainable using CMake. We learned how to incorporate unit tests in our projects to help write correct code and how to debug problems when they arise. We learned how the toolchain takes our code and runs it through a pipeline of programs to produce the executable files. We finished with some rules of thumb that help us to create readable, understandable, and maintainable code.

In this chapter, we will go on a whirlwind tour of the C++ type system, declaring and using our own types as we go.
