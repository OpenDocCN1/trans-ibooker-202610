+   4. If there no 9 fields found from above step return with appropriate error message.
+   5. If there are 9 fields found, do corresponding checks for all fields listed in start of activity.
+   6. If step 5 does not pass, return with appropriate error message.
+   7. If step 5 is passed, store the record in vector of records.
+   8. After completely parsing Record file, all the records will be stored in vector of records.

While creating flow for parsing all the three files, we see few steps repeating for all 3 files such as:

Checking if file exists and is readable

Checking if file has a correct header information

Parsing the records with delimiter

Checking if field is Digit is common in Currency and Record file

Checking if field is Numeric is common in Currency and Record file

The above points will help in refactoring the code. Also, there will be one common function for parsing the fields with delimiter, that is trim function. So, when we parse records with delimiter we can get value with space or tab at begin or end and that may not be needed , so we need to trim it once when parse the record.

Now we know we have above steps that are common we can write individual function for them. To start with TDD, we first understand requirement for function and start writing unit test cases first to test these functionalities. Then we write function such that it will pass the unit test cases. If few test cases fail we iterate the steps of updating function and executing test cases, until all of them pass.

For our example, above we can write trim function,

Now we know in trim function we need to remove first and last extra spaces/tab. For example, if string contains " AA ", the trim should return "AA" remove all the spaces.

The trim function can return new string with expected value or it can update the same string which was passed to it.

```
string trim ( string & ) ;
```