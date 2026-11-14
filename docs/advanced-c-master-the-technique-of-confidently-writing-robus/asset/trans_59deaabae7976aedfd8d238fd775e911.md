

# 调试栈实现

因此，我们已经缩小了可能存在问题的代码范围。我们可以运行调试器定位到 `displayStackStats` 并进入 `displayStackStats` 内部，以查明导致 `Stack` 内部值被修改的原因。以下是相关截图，其中用户需要从头开始启动调试器：

![Figure 7.20: Debugger display – IV](image/C14583_07_20.jpg)

## Figure 7.20: 调试器显示 – IV

重新从头启动调试器并运行到第 11 行调用 `displayStackStats` 的执行点后，我们需要单步进入。流程是进入 `displayStackStats` 函数的开头。此外，我们需要执行下一条语句。由于函数中的初始检查很明确，它们不会改变 `head` 的值，我们可以按 `Enter` 执行后续步骤。当我们怀疑后续步骤可能会改变我们正在查找的变量值时，我们需要单步进入。这在前面的快照中用红色高亮显示。后续执行来到 `line 97`，即 `displayStackStats` 函数的最后一行。

输入 `s` 后，执行移动到析构函数栈并在第 81 行调用 `clean` 函数。该 `clean` 命令删除 `tmp` 变量，其值与 `head` 相同。该函数清空了 `stack`，这原本是不应该运行的。本应仅调用并执行 `displayStackStats` 函数，最终返回 `main` 函数。但析构函数可能会被调用，因为函数中的局部变量在函数完成后超出作用域。这里的局部变量是在第 92 行作为参数传递给 `displayStackStats` 函数的变量。因此，当调用 `displayStackStats` 函数时，会创建 `main` 函数中 `si` 变量的一个局部副本。当该变量超出作用域时，它调用了 `Stack` 的析构函数。现在，`si` 变量的指针已被复制到临时变量，并错误地在最后删除了指针。这并非开发者本意。因此，在代码执行结束时，报告了双重释放错误。当 `si` 变量超出作用域时，它必须调用 `Stack` 析构函数，因为它将尝试再次释放相同的内存。要解决此问题，很明显 `displayStackStats` 函数必须通过引用传递参数来调用。为此，我们必须更新 `Stack.h` 文件中 `displayStackStats` 函数的代码：

```cpp
template<typename T>

void displayStackStats(Stack<T> &st){

    cout << endl << "------------------------------" << endl;

    cout << "Showing Stack basic Stats ...  " << endl;

    cout << "Stack is empty : " << (st.is_empty() ? "true" : "false") << endl;

    cout << "Stack size :" << st.size() << endl;

    cout << "--------------------------------" << endl << endl;

}
```

现在，当我们保存并编译 `Main.cpp` 文件时，将生成二进制文件：

`./Main`

上述命令在终端中生成以下输出：

![Figure 7.21: Debugger display – IV](image/C14583_07_21.jpg)

## Figure 7.21: 调试器显示 – IV

从前面的截图中，我们可以看到栈顶显示了预期的 `57` 和 `426` 值。`displayStackStats` 函数也为 `int` 和 `char` `Stack` 显示了正确的信息。最后，我们使用调试器定位了错误并修复了它。在下一节中，我们将完成一个活动，我们将开发用于解析文件的函数，并编写测试用例来检查函数的准确性。

# 活动 1：使用测试用例检查函数的准确性并理解测试驱动开发 (TDD)

在本活动中，我们将开发函数以便解析文件，然后编写测试用例来检查我们开发的函数的正确性。

一家大型零售组织的 IT 团队希望作为其对账工作的一部分，通过在数据库中存储 `Product` 详细信息和 `Customer` 详细信息来跟踪产品销售。销售部门将定期以简单的文本格式向 IT 团队提供此数据。作为开发人员，您需要确保完成数据的基本合理性检查，并且在公司将记录存入数据库之前，所有记录都已被正确解析。销售部门将提供两个文本文件，分别包含所有销售交易的 `Customer` 信息和 `Currency` 信息。您需要编写解析函数来处理这些文件。这两个文件是 `RecordFile.txt` 和 `CurrencyConversion.txt`。`RecordFile.txt` 文件包含客户详细信息、他们购买的产品以及区域货币和外币的总价。`CurrencyConversion.txt` 文件包含两个字段，即 `Currency` 和 `ConversionRatio`。

环境设置所需的所有必要信息都保存在配置文件中。这也将保存文件名，以及其他参数（如 `DB`、`RESTAPI` 等）和变量值，在一个名为 `parse.conf` 的文件中。它将每行包含两个字段，由 `=` 分隔符分隔。第一行将是一个标题，声明 `CONFIGURATION_FILE`。对于 Record 文件，变量名是 `recordFile`，同样对于 Currency 文件，变量名是 `currencyFile`。

## CurrencyConversion.txt 的测试条件

以下是我们将编写的测试条件，用于检查解析 `CurrencyConversion.txt` 文件的函数的准确性：

*   第一行应为标题行，且其第一个字段应包含 `Currency` 字符串。
*   `Currency` 字段应由三个字符组成。例如：`USD`、`GBP` 有效。
*   `ConversionRatio` 字段应由浮点数组成。例如，`1.2`、`0.06` 有效。
*   每行应恰好有两个字段。
*   记录使用的分隔符是 `|`。

## RecordFile.txt 的测试条件

以下是我们将编写的测试条件，用于检查解析 `RecordFile.txt` 文件的函数的准确性：

*   第一行应包含标题行，且其第一个字段应包含 `Customer Id` 字符串。
*   `Customer Id`、`Order Id`、`Product Id` 和 `Quantity` 应均为整数值。例如，`12312`、`4531134` 有效。
*   `TotalPrice (Regional Currency)` 和 `TotalPrice (USD)` 应为浮点值。例如，`2433.34`、`3434.11` 有效。
*   `RegionalCurrency` 字段的值应存在于 `CurrencyConversion.txt` 文件或 `std::map` 中。
*   每行应恰好有九个字段，如文件 `HEADER` 信息中所定义。
*   记录的分隔符是 `|`。

## 实现步骤

请遵循以下步骤实现此活动：

1.  解析 `parse.conf` 配置文件，其中包含项目运行的环境变量。
2.  从步骤 1 中正确设置 `recordFile` 和 `currencyFile` 变量。
3.  使用从配置文件检索到的这些变量，在满足所有条件的情况下解析 Currency 文件。如果条件不满足，则返回适当的错误消息。
4.  在满足所有条件的情况下解析 Record 文件。如果不满足，则返回错误消息。
5.  创建一个名为 `CommonHeader.h` 的头文件，并声明所有工具函数，即 `isAllNumbers()`、`isDigit()`、`parseLine()`、`checkFile()`、`parseConfig()`、`parseCurrencyParameters()`、`fillCurrencyMap()`、`parseRecordFile()`、`checkRecord()`、`displayCurrencyMap()` 和 `displayRecords()`。
6.  创建一个名为 `Util.cpp` 的文件，并定义所有工具函数。

