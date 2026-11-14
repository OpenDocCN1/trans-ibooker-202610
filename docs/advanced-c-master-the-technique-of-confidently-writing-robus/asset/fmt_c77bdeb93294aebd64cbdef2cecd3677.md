
## Unit Testing Using Mock Objects

When a developer works on unit testing, there may be scenarios wherein certain interfaces are called after concrete actions have taken place. For example, as we discussed in the preceding scenarios, let's assume that the project is designed in such a way that, before its execution, it takes all the configuration information from the database. It queries the database to get specific parameters, for example, the web server's **IP address**, **user**, and **password**. It then tries to connect to a web server (maybe there is another module that handles network-related tasks) or starts acting on the items that are needed for an actual project. Previously, we worked on testing the validity of the IP address. Now, we'll go a step further. Let's assume that the IP address is fetched from the database and that we have a utility class that handles connecting to **DB** and querying **IP addresses**.

Now, to test the IP address' validity, we need to assume the database connectivity has been set up. This means that the application can query the database correctly and get the results of the query, one of which is an **IP address**. Only then can we test the IP address' validity. Now, in order to perform such testing, we must assume that all the necessary activities have been completed and that we've got an **IP address** to test. Here comes the mock object, which behaves like the real object. It provides the facility for unit testing so that the application is under the impression that the IP address has already been fetched from the database, but in reality, we have imitated it. To create a mock object, we need to inherit it from the class that it needs to imitate. In the next section, we'll walk through an exercise to get a better understanding of the mock object.

### Exercise 5: Creating Mock Objects

In this exercise, we'll create mock objects by assuming that all the interfaces are working as expected. Using these objects, we'll test a few functionalities such as validating the **IP address**, checking DB connectivity, and checking whether the **username** and **password** are in correct format. Once all the tests have passed, we'll confirm the application and make it ready for **QA**. Perform the following steps to implement this exercise:

#### 1. Create a header file named `Misc.h` and include the necessary libraries:

```cpp
#include <iostream>
#include <string>
#include <sstream>
#include <vector>
#include <iterator>
#include <regex>

using namespace std;
```

#### 2. Create a class named `ConnectDatabase` that will connect to the database and return the result of the query. Within the class, declare the `Dbname`, `user`, and `passwd` variables. Also, declare a constructor and two virtual functions. Out of these two virtual functions, the first one must be a destructor, and the second one must be the `getResult()` function, which returns the query result from the database. Add the following code to implement this:

```cpp
class ConnectDatabase {
    string DBname;
    string user;
    string passwd;

public:
    ConnectDatabase() {}
    ConnectDatabase(string _dbname, string _uname, string _passwd) :
        DBname(_dbname), user(_uname), passwd(_passwd) { }

    virtual ~ConnectDatabase() {}
    virtual string getResult(string query);
};
```

#### 3. Create another class named `WebServerConnect`. Declare three `string` variables inside the `class`, namely `Webserver`, `uname`, and `passwd`. Create constructor and two virtual functions. Out of these two virtual functions, the first one must be a destructor, and the second one must be the `getRequest()` function. Add the following code to implement this:

```cpp
class WebServerConnect {
    string Webserver;
    string uname;
    string passwd;

public:
    WebServerConnect(string _sname, string _uname, string _passwd) :
        Webserver(_sname), uname(_uname), passwd(_passwd) { }

    virtual ~WebServerConnect() {}
    virtual string getRequest(string req);
};
```

##### Note

**Virtual functions** are required since we are going to create a **Mock class** from the preceding class and call these functions.

#### 4. Create a class named `App`. Create the constructors, and destructors and call all the functions. Add the following code to implement this:

```cpp
class App {
    ConnectDatabase *DB;
    WebServerConnect *WB;

public:
    App(): DB(NULL), WB(NULL) {}
    ~App() {
        if (DB)  delete DB;
        if (WB)  delete WB;
    }

    bool checkValidIp(string ip);
    string getDBResult(string query);
    string getWebResult(string query);
    void connectDB(string, string, string);
    void connectDB(ConnectDatabase *db);
    void connectWeb(string, string, string);
    void run();
};
```

In the preceding code, the app will first query the database and get the **IP address**. It then connects to the web server with the necessary information and query it to get the required information.

#### 5. Create a class named `MockMisc.h` and add the following code:

```cpp
#include "Misc.h"
#include <gtest/gtest.h>
#include <gmock/gmock.h>

class MockDB : public ConnectDatabase {
public:
    MockDB() {}
    virtual ~MockDB() {}
    MOCK_METHOD1(getResult, string(string));
};
```

### IP Address Validation Code Example

The following code snippet demonstrates IP address validation logic:

```cpp
if (ip[strlen(ip)-1] == '.') {
    cout << "ERROR : Incorrect token at end" << endl;
    return false;
}

istringstream istrstr(ip);
vector<string> tokens;
string token;
regex expression("[^0-9]");
smatch m;

while (getline(istrstr, token, '.')) {
    if (token.empty()) {
        cout << "ERROR : Got empty token" << endl;
        return false;
    }
    if (token.find(' ') != string::npos) {
        cout << "ERROR : Space character in token" << endl;
        return false;
    }
    if (regex_search(token, m, expression)) {
        cout << "ERROR : NonDigit character in token" << endl;
        return false;
    }
    int val = atoi(token.c_str());
    if (val < 0 || val > 255) {
        cout << "ERROR : Invalid digit in token" << endl;
        return false;
    }
    tokens.push_back(token);
}

if (tokens.size() != 4) {
    cout << "ERROR : Incorrect IP tokens used" << endl;
    return false;
}

cout << "strlen: " << strlen(ip) << endl;
return true;
```

### Running the Test Binary

Open the terminal and write the following command to run the binary file:

```bash
./TestRun
```

All the test cases have been executed, as shown in the following screenshot:

![Figure 7.12: Output of test cases run](image/C14583_07_12.jpg)

**Figure 7.12: Output of test cases run**

The preceding screenshot shows that there are **10** test cases in the **CheckIp** test suite and that all the test cases ran fine. In the next section, we'll learn about unit testing using mock objects.
