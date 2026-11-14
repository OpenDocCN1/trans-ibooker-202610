
# 解析配置文件与记录文件

## 8. 定义 `parseConfig()` 函数

编写以下代码来定义 `parseConfig()` 函数：

```cpp
// Function to parse Config file. Each line will have '<name> = <value> format
// Store CurrencyConversion file and Record File parameters correctly.

bool parseConfig() {
    ifstream coffle;
    string error;
    if (!checkFile(confFile, configFile, "CONFIGURATION_FILE", '=', error)) {
        cerr << "ERROR: " << error << endl;
        return false;
    }
    bool flag = true;
    vector<string> split;
    while (confFile.good()) {
        parseLine(confFile, split, '=');
        if (split.size() == 2) {
            string name = trim(split[0]);
            string value = trim(split[1]);
            if (name == "currencyFile")
                currencyFile = value;
            else if (name == "recordFile")
                recordFile = value;
        }
    }
    if (currencyFile.empty() || recordFile.empty()) {
        cerr << "ERROR : currencyfile or recordfile not set correctly." << endl;
        flag = false;
    }
    return flag;
}
```

## 9. 定义 `fillCurrencyMap()` 函数

编写以下代码来定义 `fillCurrencyMap()` 函数：

```cpp
// Function to parse CurrencyConversion file and store values in Map.

bool fillCurrencyMap() {
    ifstream currFile;
    string error;
    if (!checkFile(currFile, currencyFile, "Currency", '|', error)) {
        cerr << "ERROR: " << error << endl;
        return false;
    }
    bool flag = true;
    vector<string> split;
    while (currFile.good()) {
        parseLine(currFile, split, '|');
        if (split.size() == 2) {
            if (parseCurrencyParameters(split)) {
                currencyMap[split[0]] = static_cast<float>(atof(split[1].c_str())); // make sure currency is valid.
            } else {
                cerr << "ERROR: Processing Currency Conversion file for Currency: " << split[0] << endl;
                flag = false;
                break;
            }
        } else if (!split.empty()) {
            cerr << "ERROR: Processing Currency Conversion , got incorrect parameters for Currency: " << split[0] << endl;
            flag = false;
            break;
        }
    }
    return flag;
}
```

## 10. 定义 `parseRecordFile()` 函数

编写以下代码来定义 `parseRecordFile()` 函数：

```cpp
// Function to parse Record File ..

bool parseRecordFile() {
    ifstream recFile;
    string error;
    if (!checkFile(recFile, recordFile, "Customer Id", '|', error)) {
        cerr << "ERROR: " << error << endl;
        return false;
    }
    bool flag = true;
    vector<string> split;
    while (recFile.good()) {
        parseLine(recFile, split, '|');
        if (split.size() == 9) {
            if (checkRecord(split)) {
                vecRecord.push_back(split); // Construct struct record and save it in vector...
            } else {
                cerr << "ERROR : Parsing Record, for Customer Id: " << split[0] << endl;
                flag = false;
                break;
            }
        } else if (!split.empty()) {
            cerr << "ERROR: Processing Record, for Customer Id: " << split[0] << endl;
            flag = false;
            break;
        }
    }
    return flag;
}
```

## 11. 定义 `displayCurrencyMap()` 函数与 `operator<<` 重载

编写以下代码来定义 `displayCurrencyMap()` 函数：

```cpp
void displayCurrencyMap() {
    cout << "Currency MAP :" << endl;
    for (auto p : currencyMap)
        cout << p.first << "  :  " << p.second << endl;
    cout << endl;
}

ostream& operator<<(ostream& os, const record& rec) {
    os << rec.customerId << "|" << rec.firstName << "|" << rec.lastName << "|"
       << rec.orderId << "|" << rec.productId << "|" << rec.quantity << "|"
       << fixed << setprecision(2) << rec.totalPriceRegional << "|" << rec.currency << "|"
       << fixed << setprecision(2) << rec.totalPriceUsd << endl;
    return os;
}
```

## 12. 定义 `displayRecords()` 函数

编写以下代码来定义 `displayRecords()` 函数：

```cpp
void displayRecords() {
    cout << " Displaying records with '|' delimiter" << endl;
    for (auto rec : vecRecord) {
        cout << rec;
    }
    cout << endl;
}
```

## 13. 创建 `ParseFiles.cpp` 并调用解析函数

创建名为 `ParseFiles.cpp` 的文件，并调用 `parseConfig()`、`fillCurrencyMap()` 和 `parseRecordFile()` 函数：

```cpp
#include <CommonHeader.h>

// Global variables ...

string configFile = "./parse.conf";
string recordFile;
string currencyFile;
map<string, float>  currencyMap;
vector<record>      vecRecord;

int main() {
    // Read Config file to set global configuration variables.
    if (!parseConfig()) {
        cerr << "Error parsing Config File " << endl;
        return false;
    }
    // Read Currency file and fill map
    if (!fillCurrencyMap()) {
        cerr << "Error setting CurrencyConversion Map " << endl;
        return false;
    }
    if (!parseRecordFile()) {
        cerr << "Error parsing Records File " << endl;
        return false;
    }
    displayCurrencyMap();
    displayRecords();
    return 0;
}
```

## 14. 编译并执行

打开编译器，通过编写以下命令编译并执行 `Util.cpp` 和 `ParseFiles.cpp` 文件：

```bash
g++ -c -g -I. -Wall Util.cpp
g++ -g -I. -Wall Util.o ParseFiles.cpp -o ParseFiles
```

两个文件的二进制文件将会生成。

在下面的截图中，你将看到这两条命令存储在 `build.sh` 脚本中并被执行。运行此脚本后，你将看到最新的 `Util.o` 和 `ParseFiles` 文件已生成：

![Figure 7.25: New files generated](image/C14583_07_25.jpg)

###### Figure 7.25: New files generated

## 15. 运行 `ParseFiles` 可执行文件

运行 `ParseFiles` 可执行文件后，我们将收到以下输出：

![Figure 7.26: New files generated](image/C14583_07_26.jpg)

###### Figure 7.26: New files generated

## 16. 创建测试用例文件 `ParseFileTestCases.cpp`

创建名为 `ParseFileTestCases.cpp` 的文件，并为工具函数编写测试用例。为 `trim` 函数编写以下测试用例：

```cpp
#include <gtest/gtest.h>
#include "../CommonHeader.h"

using namespace std;

// Global variables ...

string configFile = "./parse.conf";
string recordFile;
string currencyFile;
map<string, float>  currencyMap;
vector<record>      vecRecord;

void setDefault() {
    configFile = "./parse.conf";
    recordFile.clear();
    currencyFile.clear();
    currencyMap.clear();
    vecRecord.clear();
}

// Test Cases for trim function ...

TEST(trim, empty) {
    string str = "    ";
    EXPECT_EQ(trim(str), string());
}

TEST(trim, start_space) {
    string str = "    adas";
    EXPECT_EQ(trim(str), string("adas"));
}
```
