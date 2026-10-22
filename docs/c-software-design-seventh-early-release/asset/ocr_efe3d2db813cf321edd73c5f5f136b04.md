“But wait a second: you just said that if the Database is an implementation detail, there is no architecture, right?” Yes, I said that. And there is nothing we can do as it is: the Database Singleton does not represent any abstraction and does not enable us to deal with dependencies at all. Singleton is just not a design pattern. So in order to remove the dependencies on the Database class, in order to make the architecture work, we will have to design for change and testability by means of introducing an abstraction and using a real design pattern. In order to achieve that, let us take a look at an example with a good way to deal with global aspects, which uses Singletons from the C++ standard library.

# Inverting the Dependencies on a Singleton

I am returning to a true El Dorado of design patterns, which I have used several times by now to demonstrate different design patterns: the C++17 polymorphic memory resources.

```
#include <array>
#include <cstddef>
#include <cstdlib>
#include <memory_resource>
#include <string>
#include <vector>
// ...

int main()
{
    std::array<std::byte,1000> raw;  // Note: not initialized!

    std::pmr::monotonic_buffer_resource
        buffer{ raw.data(), raw.size(),
    std::pmr::null_memory_resource() };

    std::pmr::vector<std::pmr::string> strings{ &buffer };

    // ...

    return EXIT_SUCCESS;
}
```