4. To create test code, we'll create our first .cpp file, that is, TestCases.cpp. This will contain test cases for our checkValidIp function. Write the following code inside it:

```
#include"CheckIp.h"
#include<gtest/gtest.h>
using namespace std;
const char * testIp;
TEST(CheckIp, testNull){
    testIp=NULL;
    ASSERT_FALSE(checkValidIp(testIp));
}
TEST(CheckIp, BadLength){
    testIp = "232.13.1231.1321.123";
    ASSERT_FALSE(checkValidIp(testIp));
}
```

In the second line of the preceding code, we're including the gtest.h file. We're also calling test cases using the TEST function, which takes two parameters: the first is the testsuite name and the second is the testcase name. For our case, we have created TestSuite CheckIp. In the TEST block, you will see that we have the Google test define an assert called ASSERT_FALSE that will check if the condition is false. If it's not, it will fail the test case and show the same in the results.

# Note

Generally, for a Google test case and test suite, you can group them in a common namespace and call the RUN_ALL_TESTS macro, which runs all the test cases attached to test binary. For each test case, it calls the SetUp function to initialize (like constructor in class), then it calls the actual test case, and finally, it calls the TearDown function (like destructor in class). It is not necessary to write the SetUp and TearDown functions unless you have to initialize something for the test cases.

5. Now, to run the test cases, we will create the main .cpp file for the test cases and call the RUN_ALL_TESTS macro. Alternatively, we can create an executable by linking the Google Test library that invokes RUN_ALL_TESTS. For our case, we will do the latter. Open the terminal and run the following command to create a test run binary:

```
g++ -c CheckIp.cpp
```

This will include the object file of CheckIp.cpp since the CheckValidIp function is defined in it.