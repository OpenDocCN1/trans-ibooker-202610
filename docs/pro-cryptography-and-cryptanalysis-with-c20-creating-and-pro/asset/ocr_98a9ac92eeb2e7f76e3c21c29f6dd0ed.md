that are displayed is based on the compiler used. For example, Visual Studio uses three and other compilers use two (they are implemented according to C99 instructions and standards).

The number of digits that represents floating point number and its precision are dependent on both the size and the specific value that is stored. The float values are represented with 6 and 9 digits as precision, with most values having a minimum of 7 important digits. The double values are represented with 15 and 18 digits as precision. Long double values are represented with at least a precision of 15 or 33 important digits, which are dependent on how the bytes are occupied.

The code in Listing 5-3 overrides the default precision that cout or std::cout displays by using the setprecision() function. The setprecision() function is defined within the iomanip header. See Figure 5-3 for the output.

# **Listing 5-3.** Default Precision

```
#include <iostream>
#include <iomanip>

using namespace std;

int main()
{
    std::cout << std::setprecision(16);
    std::cout << 3.333333333333333333333333333333333f << endl;
    std::cout << 3.333333333333333333333333333333333 << endl;

    return 0;
}
```

![](bbox=[0.266, 0.699, 0.772, 0.838])

# **Figure 5-3.** *Overriding the default precision*