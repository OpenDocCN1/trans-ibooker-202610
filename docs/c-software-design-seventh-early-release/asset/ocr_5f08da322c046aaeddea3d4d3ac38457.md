its defense, std::string was not part of the original STL. Thus it was not designed alongside the STL containers (std::vector, std::list, std::set, ...) and only later adapted to the STL design. That explains why it is different than the other STL containers and does not completely share their beautiful design goal.

# The Problem of Free Functions: Expectations on the Behavior

Apparently, free functions are remarkably powerful and seriously important for generic programming. They play a vital role in the design of the STL and the design of the C++ Standard Library as a whole, which builds on the power of this abstraction mechanism.¹³ However, all of this power can only work, if a set of overload functions adheres to a set of rules and certain expectations. It can only work, if it adheres to the LSP.

For instance, let us imagine that you have written your own Widget type, and want to provide a custom swap() operation for that:

```
//---- <Widget.h> ----------------

struct Widget
{
    int i;
    int j;
};

void swap( Widget& w1, Widget& w2 )
{
    using std::swap;
    swap( w1.i, w2.i );
}
```

Your Widget only needs to be a simple wrapper for int values, called i and j. You provide the according swap() function as an accompanying free function. And you implement swap() by swapping only the i value, not the j value. Further imagine, that your Widget type is used by some