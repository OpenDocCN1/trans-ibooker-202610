# Caveat Emptor

We have seen how ADL makes life easier for the programmer by including namespaces related to the function argument types. However, this lookup capability does not come without risk which, for the most part we can minimize. Consider the following sample code:

```
#include <iostream>

namespace mylib
{
void is_substring(std::string superstring, std::string substring)
{
    std::cout << "mylib::is_substring()\n";
}

void contains(std::string superstring, const char* substring) {
    is_substring(superstring, substring);
}
}

int main() {
    mylib::contains("Really long reference", "included");
}
```

When we compile and run the preceding program, we get the expected output:

```
$ ./adl_caveat.exe
mylib::is_substring()
$
```

Figure 3.36: ADL Sample program output