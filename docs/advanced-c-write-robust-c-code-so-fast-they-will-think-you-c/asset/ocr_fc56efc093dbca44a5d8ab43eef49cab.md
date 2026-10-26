Then, you use it in a few places in your code:

```
```cpp
int res = max (5, 3);
```
```

```
```cpp
std::cout << (max (a, b));
```
```

When the preprocessor finishes its work, the code will be as follows:

```
```cpp
int res = 5 < 3 ? 3 : 5;
```
```

```
```cpp
std::cout << (a < b ? b : a);
```
```

As an expression, you can use any valid expression, such as a function call, inline function, value, and so on. If you need to write the expression in more than one line, use the back-slash operator, "\\". For example, the max definition we can write in two lines is as follows:

```
```cpp
#define max(a, b) \\
a < b ? b : a
```
```

> **Note**
> 
> The macro definitions came from the C language. It's better to use const variables or inline functions instead. However, there are still cases when it's more convenient to use macro definitions, for example, in loggers when you wish to define different logging levels.

Now, we know everything we need to complete the activity. So, let's sum up what we have learned in this chapter and let's improve the project that we wrote in *Chapter 5, The Philosophers' Dinner – Threads and Concurrency*. We'll be developing a thread-safe logger and integrating it into our project.