

## 使用模拟对象进行单元测试

当开发人员进行单元测试时，可能会遇到某些场景：在具体动作发生后会调用特定接口。例如，正如我们在前面的场景中讨论的那样，假设项目的设计是在执行前，从数据库获取所有配置信息。它查询数据库以获取特定参数，例如 Web 服务器的 **IP 地址**、**用户** 和 **密码**。然后它尝试连接到 Web 服务器（可能有另一个模块处理网络相关任务）或开始处理实际项目所需的项目。之前，我们测试了 IP 地址的有效性。现在，我们将更进一步。假设 IP 地址是从数据库获取的，并且我们有一个处理连接 **DB** 和查询 **IP 地址** 的工具类。

现在，要测试 IP 地址的有效性，我们需要假设数据库连接已建立。这意味着应用程序可以正确查询数据库并获取查询结果，其中之一是 **IP 地址**。只有在那时，我们才能测试 IP 地址的有效性。为了执行此类测试，我们必须假设所有必要的活动都已完成，并且我们已经获得了一个用于测试的 **IP 地址**。这时模拟对象就派上用场了，它的行为就像真实对象一样。它为单元测试提供了便利，使应用程序误以为 IP 地址已经从数据库获取，但实际上我们是模拟的。要创建模拟对象，我们需要从它需要模仿的类继承。在下一节中，我们将通过一个练习来更好地理解模拟对象。

### 练习 5：创建模拟对象

在本练习中，我们将通过假设所有接口都按预期工作来创建模拟对象。使用这些对象，我们将测试一些功能，例如验证 **IP 地址**、检查数据库连接性，以及检查 **用户名** 和 **密码** 格式是否正确。所有测试通过后，我们将确认应用程序并使其准备好进行 **QA**。执行以下步骤以实现此练习：

#### 1. 创建一个名为 `Misc.h` 的头文件并包含必要的库：

```cpp
#include <iostream>
#include <string>
#include <sstream>
#include <vector>
#include <iterator>
#include <regex>

using namespace std;
```

#### 2. 创建一个名为 `ConnectDatabase` 的类，用于连接数据库并返回查询结果。在类中，声明 `Dbname`、`user` 和 `passwd` 变量。此外，声明一个构造函数和两个虚函数。在这两个虚函数中，第一个必须是析构函数，第二个必须是 `getResult()` 函数，它返回来自数据库的查询结果。添加以下代码以实现此功能：

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

#### 3. 创建另一个名为 `WebServerConnect` 的类。在 `class` 内部声明三个 `string` 变量，分别为 `Webserver`、`uname` 和 `passwd`。创建构造函数和两个虚函数。在这两个虚函数中，第一个必须是析构函数，第二个必须是 `getRequest()` 函数。添加以下代码以实现此功能：

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

##### 注意

**虚函数** 是必需的，因为我们将从前面的类创建 **模拟类** 并调用这些函数。

#### 4. 创建一个名为 `App` 的类。创建构造函数和析构函数并调用所有函数。添加以下代码以实现此功能：

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

在前面的代码中，应用程序将首先查询数据库并获取 **IP 地址**。然后使用必要的信息连接到 Web 服务器并查询以获取所需信息。

#### 5. 创建一个名为 `MockMisc.h` 的类并添加以下代码：

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

### IP 地址验证代码示例

以下代码片段演示了 IP 地址验证逻辑：

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

### 运行测试二进制文件

打开终端并输入以下命令以运行二进制文件：

```bash
./TestRun
```

所有测试用例均已执行，如下截图所示：

![图 7.12：测试用例运行输出](image/C14583_07_12.jpg)

**图 7.12：测试用例运行输出**

前面的截图显示 **CheckIp** 测试套件中有 **10** 个测试用例，并且所有测试用例均运行正常。在下一节中，我们将学习使用模拟对象进行单元测试。

