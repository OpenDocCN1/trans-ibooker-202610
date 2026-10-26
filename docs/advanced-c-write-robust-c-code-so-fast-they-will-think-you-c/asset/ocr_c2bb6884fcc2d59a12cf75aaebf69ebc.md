+   3. Now, let's write some code into the **ANewClass** class and access it from the **CxxTemplate** class that we already had. Open **ANewClass.cpp** and change the beginning of the file to match the following, and then save the file:

```
```cpp
#include "ANewClass.h"
#include <iostream>

void ANewClass::run() {
    std::cout << "Hello from ANewClass." << std::endl;
}
```
```

You will see that Eclipse warns us with a **Member declaration not found** message:

![](bbox=[0.219, 0.327, 0.715, 0.563])

Figure 1.17: Analyzer warning

This error is generated since we need to add this to our **ANewClass.h** file as well. Such warnings are made possible by analyzers in IDEs and are quite useful as they help you fix your code as you are typing, without running the compiler.

+   4. Open the **ANewClass.h** file, add the following code, and save the file:

```
```cpp
public:
    void run(); // we added this line
    ANewClass();
```
```

You should see that the error in the **.cpp** file went away. If it did not go away, it may be because you may have forgotten to save one of the files. You should make it a habit to press Ctrl + S to save the current file, or *Shift + Ctrl + S* to save all the files that you edited.