
# Debugging the Stack Implementation

So, we have narrowed down the code where there may be an issue. We can run the debugger to point `displayStackStats` and move inside `displayStackStats` to find the reasons that caused the values to be altered inside the `Stack`. The following is a screenshot of the same wherein the user needs to start the debugger from the beginning:

![Figure 7.20: Debugger display – IV](image/C14583_07_20.jpg)

## Figure 7.20: Debugger Display – IV

After restarting the debugger from the beginning and coming to the execution point at line 11 where `displayStackStats` is called, we need to step in. The flow is to go to the start of the `displayStackStats` function. Also, we need to execute the next statement. Since the initial checks in the function are clear, they don't alter the value of the `head` and we can press `Enter` to execute the next steps. When we suspect that the next steps can alter the value of the variable that we're looking for, we need to step in. This is done in the preceding snapshot, highlighted in red. The latter execution comes to `line 97`, that is, the last line for the `displayStackStats` function.

After typing in `s`, the execution moves to the destructor stack and calls the `clean` function at line 81. This `clean` command deletes the `tmp` variable, which has the same value as that of the `head`. The function empties the `stack`, which was not expected to run. It was only the `displayStackStats` function that was supposed to be called and executed to finally return to the `main` function. But the destructor may be called due to the local variable in the function that goes out of scope after the function is completed. Here, the local variable was the variable that was declared as a parameter to the `displayStackStats` function at `line 92`. So, a local copy of the `si` variable from the `main` function was created when the `displayStackStats` function was called. This variable called the destructor of `Stack` when it went out of scope. Now, the pointers from the `si` variable have been copied to the temporary variable and deleted the pointer at the end by mistake. This was not intended by the developer. Thus, at the end of the execution of the code, a double free error was stated. The `si` variable must call the `Stack` destructor when it goes out of scope as it will try to free the same memory again. To resolve this issue, it is clear that the `displayStackStats` function must be called with the passing parameter as a reference. For that, we must update the code of the `displayStackStats` function in the `Stack.h` file:

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

Now, when we save and compile the `Main.cpp` file, the binary will be generated:

`./Main`

The preceding command generates the following output in the Terminal:

![Figure 7.21: Debugger display – IV](image/C14583_07_21.jpg)

## Figure 7.21: Debugger Display – IV

From the preceding screenshot, we can see that the expected values of `57` and `426` are shown at the top of the stack. The `displayStackStats` function also displays the correct information for `int` and the `char` `Stack`. Finally, we located the error using the debugger and fixed it. In the next section, we'll solve an activity wherein we'll be developing functions for parsing files and writing test cases to check the accuracy of the functions.

# Activity 1: Checking the Accuracy of the Functions Using Test Cases and Understanding Test-Driven Development (TDD)

In this activity, we'll be developing functions so that we can parse files and then write test cases to check the correctness of the functions we've developed.

An IT team in a big retail organization wants to keep track of product sales by storing `Product` details and `Customer` details in its database as part of its reconciliation. On a periodic basis, the sales department will provide this data to the IT team in a simple text format. As the developer, you need to ensure that the basic sanity checking of data is done and that all the records are parsed correctly before the company stores records in the database. The sales department will provide two text files that hold `Customer` information and `Currency` information for all the sales transactions. You need to write parsing functions to process these files. The two files are `RecordFile.txt` and `CurrencyConversion.txt`. The `RecordFile.txt` file contains details of the customer, the products they have purchased, and the total price in regional currency and foreign currency. The `CurrencyConversion.txt` file contains two fields, namely `Currency` and `ConversionRatio`.

All the necessary information for the environment's setup for this project is kept in the Configuration file. This will also hold filenames, along with other parameters (such as `DB`, `RESTAPI`, and so on) and variable values in a file called `parse.conf`. It will contain two fields per line separated by the `=` delimiter. The first line will be a header that states `CONFIGURATION_FILE`. For the Record file, the variable name is `recordFile` and similarly for the Currency file, the variable name is `currencyFile`.

## Test Conditions for CurrencyConversion.txt

The following are the test conditions we'll be writing to check the accuracy of the function that's used to parse the `CurrencyConversion.txt` file:

*   The first line should be the header line and its first field should contain the `Currency` string.
*   The `Currency` field should be made up of three characters. For example: `USD`, `GBP` is valid.
*   The `ConversionRatio` field should be made of floating-point numbers. For example, `1.2`, `0.06` is valid.
*   There should be exactly two fields for each line.
*   The delimiter that's used for the record is `|`.

## Test Conditions for RecordFile.txt

The following are test conditions we'll be writing to check the accuracy of the function that's used to parse the `RecordFile.txt` file:

*   The first line should contain the header line and its first field should contain the `Customer Id` string.
*   The `Customer Id`, `Order Id`, `Product Id`, and `Quantity` should all be integer values. For example, `12312`, `4531134` is valid.
*   `TotalPrice (Regional Currency)` and `TotalPrice (USD)` should be floating-point values. For example, `2433.34`, `3434.11` is valid.
*   The `RegionalCurrency` field should have its value present in the `CurrencyConversion.txt` file or `std::map`.
*   There should be exactly nine fields in each line, as defined in the `HEADER` information of the file.
*   The delimiter for the record is `|`.

## Implementation Steps

Follow these steps to implement this activity:

1.  Parse the `parse.conf` configuration file, which includes the environment variables for the project to run.
2.  Set the `recordFile` and `currencyFile` variables correctly from Step 1.
3.  Using these variables we retrieved from the Config file, parse the Currency file with all conditions met. If the condition is not met, return the appropriate error message.
4.  Parse the Record file with all the conditions we've met. If not, then return an error message.
5.  Create a header file named `CommonHeader.h` and declare all the utility functions, that is, `isAllNumbers()`, `isDigit()`, `parseLine()`, `checkFile()`, `parseConfig()`, `parseCurrencyParameters()`, `fillCurrencyMap()`, `parseRecordFile()`, `checkRecord()`, `displayCurrencyMap()`, and `displayRecords()`.
6.  Create a file named `Util.cpp` and define all the utility functions.
