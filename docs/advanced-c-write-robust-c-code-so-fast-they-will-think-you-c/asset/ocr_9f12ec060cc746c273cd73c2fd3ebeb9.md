This looks better. Again, you may want to check which characters are being used to fill in empty spaces and which are actually spaces that you inserted between digits. Let's set the filling character to "*", for example. You will get the following output:

```
2.33* 3.45*
2.2** 4.55*
3.67* 3.02*
```

Now, you can see that the blank space is filled with stars. Now that we've considered where it can be useful to format width and fill with output, let's consider how we can do that with I/O manipulators. The std::setw and std::setfill functions are declared in the <iomanip> header. std::setw takes an integer value as a parameter and sets the width of the stream to exact n characters. There are a few cases where the width will be set to 0. They are as follows:

+   - When the shift operator is called with std::string or char
+   - When the std::put_money() function is called
+   - When the std::quoted() function is called

In the <ios> header, there are declared functions for changing the place where filling characters should be inserted: std::internal, std::left, and std::right. They are only used for output operations and only affect integer, floating-point, and monetary values.

Now, let's consider an example of using all of them together. Let's output positive, negative, floating-point, and hexadecimal values with a width of 10 and replace the filling character with "#":

```
std::cout << "Internal fill: " << std::endl
    << std::setfill('#')
    << std::internal
    << std::setw(10) << -2.38 << std::endl
    << std::setw(10) << 2.38 << std::endl
    << std::setw(10) << std::hex << std::showbase << 0x4b << std::endl;
```

```
std::cout << "Left fill: " << std::endl
    << std::left
    << std::setw(10) << -2.38 << std::endl
    << std::setw(10) << 2.38 << std::endl
```