```
void connectDB(string, string, string);
void connectDB(ConnectDatabase *db);
void connectWeb(string, string, string);
void run();
};
```

In the preceding code, the app will first query the database and get the **IP address**. It then connects to the web server with the necessary information and query it to get the required information.

+   5. Create a class named **MockMisc.h** and add the following code:

```
#include"Misc.h"
#include<gtest/gtest.h>
#include<gmock/gmock.h>

class MockDB : public ConnectDatabase {
    public :
        MockDB() {}
        virtual ~MockDB(){}

        MOCK_METHOD1(getResult, string( string) );
};
```

In the preceding code, you can see that we have included the **gmock** header file, which is needed to create a mock class. Also, the **MockDB** class is inherited from the **ConnectDatabase** class. The **MOCK_METHOD1(getResult, string(string))**; line states that we are going to mock the **getResult** interface. So, during unit testing, we can call the **getResult** function directly with the desired result without creating the **ConnectDatabase** class and running a real query to the database. One important point that needs to be noted here is that the function we need to mock must be defined with the **MOCK_METHOD[N]** macro, where N is the number of parameters the interface will take. In our case, the **getResult** interface takes one parameter. Thus, it is mocked with the **MOCK_METHOD1** macro.

+   6. Create a file named **Misc.cpp** and add the following code:

```
#include"Misc.h"
#include <unistd.h>

string ConnectDatabase::getResult(string query){
    // dummy func, need to implement..
    // assuming query sent to DB is success and
    // will return some dummy string
    return string("DB returned success");
```