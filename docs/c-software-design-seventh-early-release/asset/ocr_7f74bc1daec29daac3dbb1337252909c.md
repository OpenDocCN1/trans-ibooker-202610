By means of the third argument, you are able to specify *what* task the algorithm is supposed to perform on all of the given elements. This can be any action, ranging from manipulating the elements to printing them, and it can be specified by something as simple as a function pointer to something as powerful as a lambda:

```
#include <algorithms>
#include <cstdlib>

void multBy10( int& i )
{
    i *= 10;
}

int main()
{
    std::vector<int> v{ 1, 2, 3, 4, 5 };

    // Multiplying all integers with 10
    std::for_each( begin(v), end(v), multBy10 );

    // Printing all integers
    std::for_each( begin(v), end(v), []( int& i ){
        std::cout << i << '\n';
    });

    return EXIT_SUCCESS;
}
```

# The Command Design Pattern Versus the Strategy Design Pattern

“Wait a second!”, I can hear you cry out. “Didn’t you just explain that the algorithms of the Standard Library are implemented by means of the *Strategy* design pattern? Isn’t this a complete contradiction of the previous statement?” Yes, you are correct. Just a few pages back, I did explain that the `std::partition()` and `std::sort()` algorithms are implemented by means of the *Strategy* design pattern. And therefore I admit that it appears as if I am now contradicting myself. However, I did not claim that *all* the algorithms are based on *Strategy*. So let me explain.