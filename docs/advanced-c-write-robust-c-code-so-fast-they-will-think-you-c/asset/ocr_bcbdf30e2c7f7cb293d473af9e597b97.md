4. Next, edit the SumFunc.cpp file as illustrated here:

```
#include "SumFunc.h"
#include <iostream>

int sum(int a, int b) {
    return a + b;
}
```

In this file, we include header files and provide the body of our function, which adds and returns two given integers.

5. Edit the CMakeFiles.txt file so that its add_executable section reflects the following code:

```
add_executable(CxxTemplate
    src/CxxTemplate.cpp
    src/ANewClass.cpp
    src/SumFunc.cpp
)
```

Here, we added the src/SumFunc.cpp file to the list of executable source files so that it is linked into the executable.

6. Make the following changes in CxxTemplate.cpp:

```
#include "CxxTemplate.h"
#include "ANewClass.h"
#include "SumFunc.h" //add this line
```

...

```
CxxApplication::CxxApplication( int argc, char *argv[] ) {
    std::cout << "Hello CMake." << std::endl;
    ANewClass anew;
    anew.run();
    std::cout << sum(3, 4) << std::endl; // add this line
}
```

# Note

The complete code of this file can be found here: https://github.com/TrainingBy-Packt/Advanced-CPlusPlus/blob/master/Lesson1/Activity01/src/CxxTemplate.cpp.