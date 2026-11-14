
## 17. 为 `isAllNumbers` 函数编写以下测试用例：

```cpp
// Test Cases for isAllNumbers function..

TEST(isNumber, alphabets_present) {
    string str = "11.qwe13";
    ASSERT_FALSE(isAllNumbers(str));
}

TEST(isNumber, special_character_present) {
    string str = "34.^%3";
    ASSERT_FALSE(isAllNumbers(str));
}

TEST(isNumber, correct_number) {
    string str = "54.765";
    ASSERT_TRUE(isAllNumbers(str));
}

TEST(isNumber, decimal_begin) {
    string str = ".624";
    ASSERT_TRUE(isAllNumbers(str));
}

TEST(isNumber, decimal_end) {
    string str = "53.";
    ASSERT_TRUE(isAllNumbers(str));
}
```

## 18. 为 `isDigit` 函数编写以下测试用例：

```cpp
// Test Cases for isDigit function...

TEST(isDigit, alphabet_present) {
    string str = "527A";
    ASSERT_FALSE(isDigit(str));
}

TEST(isDigit, decimal_present) {
    string str = "21.55";
    ASSERT_FALSE(isDigit(str));
}

TEST(isDigit, correct_digit) {
    string str = "9769";
    ASSERT_TRUE(isDigit(str));
}
```

## 19. 为 `parseCurrencyParameters` 函数编写以下测试用例：

```cpp
// Test Cases for parseCurrencyParameters function

TEST(CurrencyParameters, extra_currency_characters) {
    vector<string> vec {"ASAA", "34.22"};
    ASSERT_FALSE(parseCurrencyParameters(vec));
}

TEST(CurrencyParameters, correct_parameters) {
    vector<string> vec {"INR", "1.44"};
    ASSERT_TRUE(parseCurrencyParameters(vec));
}
```

## 20. 为 `checkFile` 函数编写以下测试用例：

```cpp
// Test Cases for checkFile function...

TEST(checkFile, no_file_present) {
    string fileName = "./NoFile";
    ifstream infile;
    string parameter("nothing");
    char delimit = ';';
    string err;
    ASSERT_FALSE(checkFile(infile, fileName, parameter, delimit, err));
}

TEST(checkFile, empty_file) {
    string fileName = "./emptyFile";
    ifstream infile;
    string parameter("nothing");
    char delimit = ';';
    string err;
    ASSERT_FALSE(checkFile(infile, fileName, parameter, delimit, err));
}

TEST(checkFile, no_header) {
    string fileName = "./noHeaderFile";
    ifstream infile;
    string parameter("header");
    char delimit = '|';
    string err;
    ASSERT_FALSE(checkFile(infile, fileName, parameter, delimit, err));
}

TEST(checkFile, incorrect_header) {
    string fileName = "./correctHeaderFile";
    ifstream infile;
    string parameter("header");
    char delimit = '|';
    string err;
    ASSERT_FALSE(checkFile(infile, fileName, parameter, delimit, err));
}

TEST(checkFile, correct_file) {
    string fileName = "./correctHeaderFile";
    ifstream infile;
    string parameter("Currency");
    char delimit = '|';
    string err;
    ASSERT_TRUE(checkFile(infile, fileName, parameter, delimit, err));
}
```

### 注意

前面函数中用作输入参数的 `NoFile`、`emptyFile`、`noHeaderFile` 和 `correctHeaderFile` 文件可以在此处找到：[https://github.com/TrainingByPackt/Advanced-CPlusPlus/tree/master/Lesson7/Activity01](https://github.com/TrainingByPackt/Advanced-CPlusPlus/tree/master/Lesson7/Activity01)。

## 21. 为 `parseConfig` 函数编写以下测试用例：

```cpp
// Test Cases for parseConfig function...

TEST(parseConfig, missing_currency_file) {
    setDefault();
    configFile = "./parseMissingCurrency.conf";
    ASSERT_FALSE(parseConfig());
}

TEST(parseConfig, missing_record_file) {
    setDefault();
    configFile = "./parseMissingRecord.conf";
    ASSERT_FALSE(parseConfig());
}

TEST(parseConfig, correct_config_file) {
    setDefault();
    configFile = "./parse.conf";
    ASSERT_TRUE(parseConfig());
}
```

### 注意

前面函数中用作输入参数的 `parseMissingCurrency.conf`、`parseMissingRecord.conf` 和 `parse.conf` 文件可以在此处找到：[https://github.com/TrainingByPackt/Advanced-CPlusPlus/tree/master/Lesson7/Activity01](https://github.com/TrainingByPackt/Advanced-CPlusPlus/tree/master/Lesson7/Activity01)。

## 22. 为 `fillCurrencyMap` 函数编写以下测试用例：

```cpp
// Test Cases for fillCurrencyMap function...

TEST(fillCurrencyMap, wrong_delimiter) {
    currencyFile = "./CurrencyWrongDelimiter.txt";
    ASSERT_FALSE(fillCurrencyMap());
}

TEST(fillCurrencyMap, extra_column) {
    currencyFile = "./CurrencyExtraColumn.txt";
    ASSERT_FALSE(fillCurrencyMap());
}

TEST(fillCurrencyMap, correct_file) {
    currencyFile = "./CurrencyConversion.txt";
    ASSERT_TRUE(fillCurrencyMap());
}
```

### 注意

前面函数中用作输入参数的 `CurrencyWrongDelimiter.txt`、`CurrencyExtraColumn.txt` 和 `CurrencyConversion.txt` 文件可以在此处找到：[https://github.com/TrainingByPackt/Advanced-CPlusPlus/tree/master/Lesson7/Activity01](https://github.com/TrainingByPackt/Advanced-CPlusPlus/tree/master/Lesson7/Activity01)。

## 23. 为 `parseRecordFile` 函数编写以下测试用例：

```cpp
// Test Cases for parseRecordFile function...

TEST(parseRecordFile, wrong_delimiter) {
    recordFile = "./RecordWrongDelimiter.txt";
    ASSERT_FALSE(parseRecordFile());
}

TEST(parseRecordFile, extra_column) {
    recordFile = "./RecordExtraColumn.txt";
    ASSERT_FALSE(parseRecordFile());
}

TEST(parseRecordFile, correct_file) {
    recordFile = "./RecordFile.txt";
    ASSERT_TRUE(parseRecordFile());
}
```

前面函数中用作输入参数的 `RecordWrongDelimiter.txt`、`RecordExtraColumn.txt` 和 `RecordFile.txt` 文件可以在此处找到：[https://github.com/TrainingByPackt/Advanced-CPlusPlus/tree/master/Lesson7/Activity01](https://github.com/TrainingByPackt/Advanced-CPlusPlus/tree/master/Lesson7/Activity01)。

## 24. 打开编译器。通过编写以下命令来编译并执行 `Util.cpp` 和 `ParseFileTestCases.cpp` 文件：

```bash
g++ -c -g -Wall ../Util.cpp -I../
g++ -c -g -Wall ParseFileTestCases.cpp
g++ -g -Wall Util.o ParseFileTestCases.o -lgtest -lgtest_main -pthread -o ParseFileTestCases
```

以下是对此的屏幕截图。你将看到所有命令都存储在 `Test.make` 脚本文件中。执行后，它将创建一个用于单元测试的二进制程序，名为 `ParseFileTestCases`。你还会注意到在 Project 中创建了一个名为 `unitTesting` 的目录。在此目录中，编写了所有与单元测试相关的代码，并创建了一个二进制文件。此外，该项目的依赖库 `Util.o` 也是通过编译 `Util.cpp` 文件中的项目而创建的：

![](image/C14583_07_27.jpg)
