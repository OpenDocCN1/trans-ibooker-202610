# I/O Manipulators for Whitespace Processing

In the Standard Library, there are functions for processing whitespaces. The std::ws function from the <istream> header only works with input streams and discards leading whitespaces. The std::skipws and std::noskipws functions from the <ios> header are used to control reading and writing leading whitespaces. They work for both input and output streams. When the std::skipws flag is set, the stream ignores whitespaces in front of the input of the character sequence. By default, the std::skipws flag is set. Let's consider an example of using these I/O manipulators. First, we will read the input with default formatting and output what we have read. Next, we will clear our strings and read data with the std::noskipws flag:

```
std::string name;

std::string surname;

std::istringstream("Peppy Ping") >> name >> surname;

std::cout << "Your name: " << name << ", your surname: " << surname <<
std::endl;

name.clear();

surname.clear();

std::istringstream("Peppy Ping") >> std::noskipws >> name >> surname;
std::cout << "Your name: " << name << ", your surname: " << surname <<
std::endl;
```

After building and running this example, we will get the following output:

```
Your name: Peppy, your surname: Ping

Your name: Peppy, your surname:
```

As you can see from the preceding output, if we set the std::noskipws flag, we will read whitespaces as well.

In the <iomanip> header, an unusual manipulator for this header has been declared: std::quoted. When this function is applied to the input, it wraps a given string in quotes with escaping characters. If the input string already contains escaped quotes, it reads them as well. In order to understand this, let's consider a small example. We will initialize a source string with some text without quotes and another string will initialize with text with escaped quotes. Next, we will read them by using std::ostringstream without a flag is set and provide the output via std::cout. Take a look at the following example: