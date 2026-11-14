
## Mocking with gmock

在前面的代码中，可以看到我们包含了 `gmock` 头文件，这是创建模拟类所必需的。此外，`MockDB` 类继承自 `ConnectDatabase` 类。`MOCK_METHOD1(getResult, string(string));` 这一行表明我们将模拟 `getResult` 接口。因此，在单元测试期间，我们可以直接使用期望的结果调用 `getResult` 函数，而无需创建 `ConnectDatabase` 类并对数据库运行真实查询。这里需要注意的一个重要点是：我们需要模拟的函数必须使用 `MOCK_METHOD[N]` 宏定义，其中 N 是接口将接受的参数数量。在我们的例子中，`getResult` 接口接受一个参数，因此使用 `MOCK_METHOD1` 宏进行模拟。

## 步骤 6：创建 Misc.cpp

创建一个名为 `Misc.cpp` 的文件并添加以下代码：

```cpp
#include"Misc.h"

#include <unistd.h>

string ConnectDatabase::getResult(string query){

     // dummy func, need to implement..

     // assuming query sent to DB is success and

     // will return some dummy string

     return string("DB returned success");

}

string WebServerConnect::getRequest(string req){

     // dummy func, need to implement..

     // assume no req string is sent to webserver. .

     // its returns the result returned from server.

     return string("Webserver returned success");

}

void App::connectDB(string dbname, string user, string passwd){

     if ( DB )

         delete DB;

     DB = new ConnectDatabase(dbname, user, passwd);

}

void App::connectDB(ConnectDatabase *db){

     if ( DB )

         delete DB;

     DB = db;

}

void App::connectWeb(string webname, string user, string passwd){

     if ( WB )

         delete WB;

     WB = new WebServerConnect(webname, user, passwd);

}

string App::getDBResult(string query){

     return DB->getResult(query);

}

string App::getWebResult(string query) {

     return WB->getRequest(query);

}

void App::run(){

     if ( (DB == NULL) || (WB == NULL) )

         return ;

     while( true ){

         // read some request to be run on web and get result. ..

         cout << getWebResult("dummy request to webserver") << endl;;

         sleep(5);

     }

}

bool App::checkValidIp(string ip){

     if(ip.empty()){

         cout << "ERROR : IP passed is NULL " << endl;

         return false;

     }

     if(ip.size() > 15){

         cout << "ERROR : IP size is greater than 15" <<endl;

         return false;

     }

     //check if last character in ip is not '.' as that is not captured in tokenizing

     if (ip[ip.size()-1] == '.'){

         cout <<"ERROR : Incorrect token at end" << endl;

         return false;

     }

     istringstream istrstr(ip);

     vector<string> tokens;

     string token;

     regex expression("[^0-9]");

     smatch m;

     while( getline(istrstr, token, '.') ){

         if ( token.empty() ){

             cout << "ERROR : Got empty token " << endl;

             return false;

         }

         if ( token.find(' ') != string::npos){

             cout << "ERROR : Space character in token " << endl;

             return false;

         }

         if ( regex_search(token, m, expression) ){

             cout << "ERROR : NonDigit character in token " << endl;

             return false;

         }

         int val = atoi(token.c_str());

         if ( val < 0 || val > 255 ){

             cout << "ERROR : Invalid Digit in token " << endl;

             return false;

         }

         tokens.push_back(token);

     }

     if ( tokens.size() != 4 ){

         cout << " ERROR : Incorect IP tokens used" << endl;

         return false;

     }

     cout << "strlen: " << ip.size() << endl;

     return true;

}
```

在前面的代码中，我们创建了一个最小接口和虚拟参数使其能够运行，以便理解实际功能。我们为 `getResult()` 和 `getRequest()` 函数开发了基本功能，其中数据库查询和 `WebServer` 查询返回默认字符串。这里，`App::run()` 函数假设数据库连接和 Web 服务器连接都已执行，现在可以定期执行 Web 查询。每次查询结束时，它将默认返回 `Webserver returned success` 字符串。

## 步骤 7：创建 RunApp.cpp

现在，创建一个名为 `RunApp.cpp` 的文件并在 main 函数中编写以下代码：

```cpp
#include"Misc.h"

int main(){

     App app;

     app.connectDB("dbname","dbuser", "dbpasswd");

     string ip = app.getDBResult("dummy");

     // DB query to get Webserver IP

     // Similarly some miscellaneous activities to get configuratio information

     // Like querying DB to get correct username/passwd to connect to WebServer..

     // After getting IP from DB, check if the IP is valid..

     //app.checkValidIp(ip);

     // Now conect to webserver with parameters extracted from DB.

     app.connectWeb("webname","user", "passwd");

     // Now run the App, like sending some request to webserver,

     // getting result and doing activity with received data.

     app.run();

     return 0;

}
```

正如前面的代码所示，创建了 `App` 类实例。使用该实例，我们借助虚拟参数 `dbname`、`dbuser` 和 `dbpasswd` 连接到数据库。然后，我们查询数据库以获取 IP 地址和其他配置参数。我们注释掉了 `app.checkValidIp(ip)` 行，因为我们假设从数据库获取的 IP 地址需要验证。此外，该函数需要进行单元测试。使用 `connectWeb()` 函数，我们可以通过传递虚拟参数 `webname`、`user` 和 `passwd` 连接到 Web 服务器。最后，我们调用 `run()` 函数，它将迭代运行，从而查询 Web 服务器并给出默认输出。

## 步骤 8：构建二进制文件

保存所有文件并打开终端。为了获得执行项目所需的基本功能，我们将构建二进制文件并执行它以查看结果。在终端中运行以下命令：

```bash
g++ Misc.cpp RunApp.cpp -o RunApp
```

前面的命令将在当前文件夹中创建一个名为 `RunApp` 的二进制文件。

## 步骤 9：运行可执行文件

现在，编写以下命令来运行可执行文件：

```bash
./RunApp
```

前面的命令在终端中生成以下输出：

![Figure 7.13: Running the app](image/C14583_07_13.jpg)

**Figure 7.13: Running the app**

正如前面的屏幕截图所示，二进制文件及时显示输出 `Webserver returned success`。到目前为止，我们的应用程序运行良好，因为它假设所有接口都按预期工作。但在使其准备好进行 `QA` 之前，我们仍然必须测试一些功能，例如验证 `IP address`、`DB connectivity`，检查 `username` 和 `password` 是否采用正确格式（如果有）等。

## 步骤 10：开始单元测试

使用相同的基础设施，开始对每个功能进行单元测试。在我们的练习中，我们将假设 `DB connectivity` 已经完成并已被查询以获取 `IP address`。之后，我们可以开始对 `IP address` 的有效性进行单元测试。因此，在我们的测试用例中，`Database` 类需要被模拟，并且 `getDBResult` 函数必须返回 `IP address`。这个 `IP address` 稍后将被传递给 `checkValidIP` 函数，我们将在其中对其进行测试。为了实现这一点，创建一个名为 `TestApp.cpp` 的类，我们将在其中调用 `checkValidIP` 函数：
