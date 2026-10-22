In “Guideline 19: Use Strategy to Isolate How Things are Done”, I have introduced you to the Strategy design pattern and in “Guideline 21: Use Commands to Isolate What Things are Done”, I have introduced you to the Command design pattern. I have demonstrated that these two design patterns are essential decoupling tools in your daily tool box. However, in “Guideline 22: Prefer Value Semantics to Reference Semantics” I have given you the idea that it is preferable to use value semantics instead of reference semantics. And this of course raises the question: how can you apply that wisdom for the Strategy and Command design patterns? Well, here is one possible value semantics solution: draw on the abstracting power of std::function.

# Introduction to std::function

In case you have not yet heard about std::function, allow me to introduce you. std::function represents an abstraction for a callable (e.g. a function pointer, function object, or lambda). The only requirement is that the callable satisfies a specific function type, which is passed as the only template parameter to a std::function. The following code gives an impression:

```
#include <cstdlib>
#include <functional>

void foo( int i )
{
    std::cout << "foo: " << i << '\n';
}

int main()
{
    // Create a default std::function instance. Calling it results
    // in a std::bad_function_call exception
    std::function<void(int)> f{};  ①

    f = []( int i ){  // Assigning a callable to 'f'  ②
        std::cout << "lambda: " << i << '\n';
    }
```