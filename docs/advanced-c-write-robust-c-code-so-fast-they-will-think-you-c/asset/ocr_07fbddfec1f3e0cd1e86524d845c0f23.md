```
int age;
string address;
string Mobile_No;
};

#endif
```

In the preceding code, the struct person is defined and consists of the following attributes: name, age, address, and Mobile_No. It also has the version number 2. Now, version 1 is the old version and version 2 is the new one. The following is a screenshot of the two header files side by side:

![](bbox=[0.14, 0.32, 0.92, 0.61])

Figure 7.5: Library file with a different version

+   3. Create a file named StaticAssertionSample.cpp and add the following code:

```
#include<iostream>
#include"PersonLibrary.h"

void doSanityCheck(){
    static_assert(PERSON_LIB_VERSION > 1 , "PERSON LIBRARY VERSION not greater than 1");
    // Do any more sanity check before starting app ...
}

int main(){
    doSanityCheck();
    return 0;
}
```