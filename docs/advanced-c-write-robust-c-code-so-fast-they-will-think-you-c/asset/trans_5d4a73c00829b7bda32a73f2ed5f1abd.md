
```
bool fillCurrencyMap();
bool parseRecordFile();
bool checkRecord(vector<string> &);
void displayCurrencyMap();
ostream& operator<<(ostream &, const record &);
void displayRecords();
#endif
```

4. 创建一个名为 Util.cpp 的文件，并定义所有工具函数。编写以下代码来定义 trim() 函数：

```
#include<CommonHeader.h>

// Utility function to remove spaces and tabs from start of string and end of string..
string trim (string &str) { // remove space and tab from string.
    string res("");
    if ((str.find(' ') != string::npos) || (str.find(' ') != string::npos)){
// if space or tab found..
        size_t begin, end;
        if ((begin = str.find_first_not_of(" \t")) != string::npos){ // if string is not empty..
            end = str.find_last_not_of(" \t");
            if ( end >= begin )
                res = str.substr(begin, end - begin + 1);
        }
    }else{
        res = str; // No space or tab found..
    }
    str = res;
    return res;
}
```

5. 编写以下代码来定义 isAllNumbers()、isDigit() 和 parseLine() 函数：

```
// Utility function to check if string contains only digits ( 0-9) and only single '.'
// eg . 1121.23 , .113, 121. are valid, but 231.14.143 is not valid.
bool isAllNumbers(const string &str){ // make sure, it only contains digit and only single '.' if any
    return ( all_of(str.begin(), str.end(), [](char c) { return (
    isdigit(c) || (c == '.')); })
        && (count(str.begin(), str.end(), '.') <= 1) );
}
```

```
//Utility function to check if string contains only digits (0-9).. 
bool isDigit(const string &str){
    return ( all_of(str.begin(), str.end(), [](char c) { return
isdigit(c); }));
}
```

```
// Utility function, where single line of file <infile> is parsed using delimiter.
// And store the tokens in vector of string.
void parseLine(ifstream &infile, vector<string> & vec, char delimiter){
    string line, token;

    getline(infile, line);
    istringstream ss(line);
    vec.clear();
    while(getline(ss, token, delimiter)) // break line using delimiter
        vec.push_back(token);  // store tokens in vector of string
}
```

6. 编写以下代码来定义 parseCurrencyParameters() 和 check-Record() 函数：

```
// Utility function to check if vector string of 2 strings contain correct
// currency and conversion ratio. currency should be 3 characters,
conversion ratio
// should be in decimal number format.
bool parseCurrencyParameters( vector<string> & vec){
    trim(vec[0]);  trim(vec[1]);
    return ( (!vec[0].empty()) && (vec[0].size() == 3) && (!vec[1].
empty()) && (isAllNumbers(vec[1])) );
}
```

```
// Utility function, to check if vector of string has correct format for records parsed from Record File.
// CustomerId, OrderId, ProductId, Quantity should be in integer format
// TotalPrice Regional and USD should be in decimal number format
// Currecny should be present in map.
bool checkRecord(vector<string> &split){
    // Trim all string in vector
    for (auto &s : split)
        trim(s);
```

```
flag = false;
} else if ( split[0].find(parameter) == string::npos ){
    error = "In " + fileName + " file, first line doesnt contain
header ";
    flag = false;
}
}
    return flag;
}
```

8. 编写以下代码来定义 parseConfig() 函数：

```
// Function to parse Config file. Each line will have '<name> = <value>' format
// Store CurrencyConversion file and Record File parameters correctly.
bool parseConfig() {
    ifstream coffle;
    string error;
    if (!checkFile(confFile, configFile, "CONFIGURATION_FILE", '=', error))
{
        cerr << "ERROR: " << error << endl;
        return false;
    }

    bool flag = true;
    vector<string> split;
    while (confFile.good()){
        parseLine(confFile, split, '=');
        if ( split.size() == 2 ){
            string name = trim(split[0]);
            string value = trim(split[1]);

            if ( name == "currencyFile" )
                currencyFile = value;
            else if ( name == "recordFile")
                recordFile = value;
        }
    }

    if ( currencyFile.empty() || recordFile.empty() ){
        cerr << "ERROR : currencyfile or recordfile not set correctly." <<
endl;
        flag = false;
```

```
cout << endl;
}

ostream& operator<<(ostream& os, const record &rec){
    os << rec.customerId << "|" << rec.firstName << "|" << rec.lastName <<
    "|"
        << rec.orderId << "|" << rec.productId << "|" << rec.quantity <<
    "|"
        << fixed << setprecision(2) << rec.totalPriceRegional << "|" << rec.
    currency << "|"
        << fixed << setprecision(2) << rec.totalPriceUsd << endl;
    return os;
}
```

12. 编写以下代码来定义 displayRecords() 函数：

```
void displayRecords(){
    cout << " Displaying records with '|' delimiter" << endl;
    for (auto rec : vecRecord){
        cout << rec;
    }
    cout << endl;
}
```

13. 创建一个名为 ParseFiles.cpp 的文件，并调用 parseConfig()、fillCurrencyMap() 和 parseRecordFile() 函数：

```
#include <CommonHeader.h>
// Global variables ...
string configFile = "./parse.conf";
string recordFile;
string currencyFile;
map<string, float> currencyMap;
vector<record>      vecRecord;
int main(){
    // Read Config file to set global configuration variables.
    if (!parseConfig()){
        cerr << "Error parsing Config File " << endl;
        return false;
    }
    // Read Currency file and fill map
    if (!fillCurrencyMap()){
        cerr << "Error setting CurrencyConversion Map " << endl;
        return false;
```

```
}
if (!parseRecordFile()){
    cerr << "Error parsing Records File " << endl;
    return false;
}
    displayCurrencyMap();
displayRecords();
return 0;
}
```

14. 打开编译器。通过编写以下命令来编译和执行 Util.cpp 和 ParseFiles.cpp 文件：

```
g++ -c -g -I. -Wall Util.cpp
g++ -g -I. -Wall Util.o ParseFiles.cpp -o ParseFiles
```

两个文件都会生成对应的二进制文件。

在下面的截图中，你将看到这两个命令被存储在 build.sh 脚本中并执行。运行此脚本后，你将看到最新的 Util.o 和 ParseFiles 文件已生成：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_737_0.png)

图 7.25：生成的新文件

15. 运行 ParseFiles 可执行文件后，我们将收到以下输出：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_738_0.png)

图 7.26：生成的新文件

16. 创建一个名为 ParseFileTestCases.cpp 的文件，并为工具函数编写测试用例。编写以下针对 trim 函数的测试用例：

```
#include<gtest/gtest.h>
#include"../CommonHeader.h"

using namespace std;

// Global variables ...
string configFile = "./parse.conf";
string recordFile;
string currencyFile;
map<string, float>  currencyMap;
vector<record>      vecRecord;

void setDefault(){
    configFile = "./parse.conf";
    recordFile.clear();
    currencyFile.clear();
    currencyMap.clear();
    vecRecord.clear();
}
```

```
TEST(CurrencyParameters, correct_parameters){
    vector<string> vec {"INR","1.44"};
    ASSERT_TRUE(parseCurrencyParameters(vec));
}
```

20. 编写以下针对 checkFile 函数的测试用例：

```
//Test Cases for checkFile function...
TEST(checkFile, no_file_present){
    string fileName = "./NoFile";
    ifstream infile;
    string parameter("nothing");
    char delimit = ',';
    string err;
    ASSERT_FALSE(checkFile(infile, fileName, parameter, delimit, err));
}
TEST(checkFile, empty_file){
    string fileName = "./emptyFile";
    ifstream infile;
    string parameter("nothing");
    char delimit = ',';
    string err;
    ASSERT_FALSE(checkFile(infile, fileName, parameter, delimit, err));
}
TEST(checkFile, no_header){
    string fileName = "./noHeaderFile";
    ifstream infile;
    string parameter("header");
    char delimit = '|';
    string err;
    ASSERT_FALSE(checkFile(infile, fileName, parameter, delimit, err));
}
TEST(checkFile, incorrect_header){
    string fileName = "./correctHeaderFile";
    ifstream infile;
    string parameter("header");
    char delimit = '|';
    string err;
    ASSERT_FALSE(checkFile(infile, fileName, parameter, delimit, err));
}
TEST(checkFile, correct_file){
    string fileName = "./correctHeaderFile";
    ifstream infile;
```
