
- **7.** Compile and execute the application. Now, go to the project folder and find where the `prices.txt` file is located. In the following screenshot, you can see the location of the created file in the project directory:

  | Name | Date modified | Size |
  | --- | --- | --- |
  | `.settings` | 01.09.2019 17:05 | |
  | `build` | 01.09.2019 17:02 | |
  | `src` | 01.09.2019 17:02 | |
  | `.cproject` | 01.09.2019 17:05 | 2 КБ |
  | `.project` | 01.09.2019 17:02 | 1 КБ |
  | `Makefile` | 01.09.2019 17:03 | 1 КБ |
  | `prices` | 02.09.2019 12:19 | 1 КБ |

  *Figure 6.5: Location of the created file*

- **8.** Open it in Notepad. In the following screenshot, you can see what the output to the file looks like:

  ![](img/6507d435e2edfa3c00c220d0ccfcaf94_427_0.png)

  *Figure 6.6: The result of the output of the user-defined type to the file*

- **9.** Now, let's read this data into the variable. Create an instance of the `std::ifstream` class. Open the file called `pricesFile`. Check if the stream has been successfully opened and print an error message if not:

  ```cpp
  std::ifstream inFile(pricesFile);
  if (inFile.fail())
  {
      std::cerr << "Failed to open file " << pricesFile << std::endl;
      return 1;
  }
  ```

- **10.** Create the local variables that will be used for the input from a file, namely `name`, `authorName`, `authorSurname`, `year`, and `price`. Their names are self-explanatory:

  ```cpp
  std::string name;
  std::string authorName;
  std::string authorSurname;
  int year;
  float price;
  ```

- **11.** Now, read the data from the file into variables in the order they are in the file:

  ```cpp
  inFile >> name >> authorName >> authorSurname >> year >> price;
  ```

- **12.** Create a `Book` instance called `book_002` and initialize it with those read values:

  ```cpp
  Book book_002(name, std::string(authorName + " " + authorSurname), year,
  price);
  ```

- **13.** To check whether the read operation was successfully executed, print the `book_002` variable to the console:

  ```cpp
  std::cout << "Book name: " << book_002.getName() << std::endl
          << "Author name: " << book_002.getAuthor() << std::endl
          << "Year: " << book_002.getYear() << std::endl
          << "Price: " << book_002.getPrice() << std::endl;
  ```

- **14.** Compile and execute the application again. In the console, you will see the following output:

  ![](img/6507d435e2edfa3c00c220d0ccfcaf94_428_0.png)

  *Figure 6.7: The result of executing Exercise 2*

As you can see, we wrote and read custom formatted data from a file without any difficulties. We created our own custom type, wrote it to the file using the `std::ofstream` class, and checked that everything was written successfully. Then, we read those data from a file into our custom variable using the `std::ifstream` class, output it to the console, and ensured that everything was read correctly. By doing this, we learned how to read and write data to a file using the I/O Standard Library. Now, let's move on and learn about the in-memory portion of the I/O library.

## String I/O Implementation

The I/O Standard Library allows input and output – not only to devices such as files but also to memory, in particular, to the `std::string` objects. In this case, the string can be a source for input operations as well as a sink for output operations. In the `<sstream>` header file, stream classes are declared that manage input and output to strings. They, like the file streams, also provide an interface that implements RAII – the string is opened for reading or writing upon the stream's creation and is closed on its destruction. They are represented in the Standard Library by the following classes: `basic_stringbuf`, which implements a raw string interface, `basic_istringstream` for input operations, `basic_ostringstream` for output operations, and `basic_stringstream` for both input and output operations. The Standard Library also provides typedefs for `char` and `wchar_t` types: `istringstream`, `ostringstream`, and `stringstream` and the same names with the `w` prefix for wide characters.

To create an object of the `std::istringstream` class, we should pass the initializer string as a constructor parameter or set it later using the `str()` function:

```cpp
std::string track("ABBA 1967 Vule");
std::istringstream iss(track);
```

Alternatively, we can do the following:

```cpp
std::string track("ABBA 1967 Vule");
std::istringstream iss;
iss.str(track);
```

Next, to read values from the stream, use the right shift operator, `>>`, which is overloaded for all built-in types:

```cpp
std::string group;
std::string name;
int year;

iss >> group >> year >> name;
```

To create an object of the `std::ostringstream` class, we just declare a variable of its type:

```cpp
std::ostringstream oss;
```

Next, to write data to the string, use the left shift operator, `<<`, which is overloaded for all built-in types:

```cpp
std::string group("ABBA");
std::string name("Vule");
int year = 1967;
```

```cpp
oss << group << std::endl
    << name << std::endl
    << year << std::endl;
```

To get the resulting string, use the `str()` function:

```cpp
std::cout << oss.str();
```

The `std::stringstream` object works bidirectionally, so it has both a default constructor and a constructor that takes the string. We can create the default `std::stringstream` object by declaring the variable of this type and then use it for reading and writing:

```cpp
std::stringstream ss;
ss << "45";
int count;
ss >> count;
```

Also, we can create `std::stringstream` using the constructor with a string parameter. Then, we can use it for reading and writing as usual:

```cpp
std::string employee("Alex Ismailow 26");
std::stringstream ss(employee);
```

Alternatively, we can create a default `std::stringstream` object and initialize it by setting a string using the `str()` function:

```cpp
std::string employee("Charlz Buttler 26");
std::stringstream ss;
ss.str(employee);
```

Next, we can use the `ss` object for reading and writing:

```cpp
std::string name;
std::string surname;
int age;
```

```cpp
ss >> name >> surname >> age;
```

We can also apply open modes for these kinds of streams. Their functionality is similar to that of file streams but with a slight difference. `ios_base::binary` is irrelevant in the case of working with string streams and `ios_base::trunc` is ignored. Thus, we can open any of the string streams in four modes: `ios_base::app`, `ios_base::ate`, `ios_base::in`, and `ios_base::out`.

### Exercise 3: Creating a Function for Replacing Words in a String

In this exercise, we will implement a function that parses the given string and replaces the given word with another word. To complete this exercise, we create a callable class that takes three parameters: the original string, the word to be replaced, and the word that will be used for replacing. As a result, the new string should be returned. Perform the following steps to complete this exercise:

- **1.** Include the required headers: `<iostream>` for the output to a Terminal and `<sstream>` for I/O string support:

  ```cpp
  #include <sstream>
  #include <iostream>
  ```

- **2.** Implement the callable class with the name `Replacer`. It has only one function – an overloaded parentheses operator, `()`, that returns a string and takes three parameters: the original string, the word to be replaced, and the word to be used for replacing. The function declaration looks as follows:

  ```cpp
  std::string operator()(const std::string& originalString,
      const std::string& wordToBeReplaced,
      const std::string& wordReplaceBy);
  ```

- **3.** Next, create the `istringstream` object, that is, `iss`, and set the `originalString` variable as the source of input:

  ```cpp
  std::istringstream iss(originalString);
  ```

- **4.** Create the `ostringstream` object, that is, `oss`, that will hold the converted string:

  ```cpp
  std::ostringstream oss;
  ```

- **5.** Then, in the loop, while input is possible, read the next word into the `word` variable. Check if this word is equal to the `wordToBeReplaced` variable. If so, replace it with the `wordReplaceBy` variable and write it to the `oss` stream. If they are unequal, write the original word to the `oss` stream. After each word, add a whitespace character, since the `iss` stream truncates them. Finally, return the result. The complete class is as follows:

  ```cpp
  class Replacer
  {
  public:
      std::string operator()(const std::string& originalString,
                              const std::string& wordToBeReplaced,
                              const std::string& wordReplaceBy)
      {
          std::istringstream iss(originalString);
          std::ostringstream oss;

          std::string word;
          while (iss >> word)
          {
              if (0 == word.compare(wordToBeReplaced))
              {
                  oss << wordReplaceBy << " ";
              }
              else
              {
                  oss << word << " ";
              }
          }

          return oss.str();
      }
  };
  ```
