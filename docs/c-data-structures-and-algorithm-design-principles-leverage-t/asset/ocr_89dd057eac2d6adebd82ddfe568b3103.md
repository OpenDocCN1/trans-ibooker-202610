9. Let's write a main function to use the preceding functions:

```
int main()
{
    singly_ll sll = {1, 2, 3};
    sll.push_front(0);

    std::cout << "First list: ";
    for(auto i: sll)
        std::cout << i << " ";
    std::cout << std::endl;

    auto sll2 = sll;
    sll2.push_front(-1);
    std::cout << "Second list after copying from first list and inserting -1 in front: ";
    for(auto i: sll2)
        std::cout << i << ' ';  // Prints -1 0 1 2 3
    std::cout << std::endl;

    std::cout << "First list after copying - deep copy: ";
    for(auto i: sll)
        std::cout << i << ' ';  // Prints 0 1 2 3
    std::cout << std::endl;
}
```

10. Running this exercise should produce the following output:

```
First list: 0 1 2 3
Second list after copying from first list and inserting -1 in front: -1 0 1 2 3
First list after copying - deep copy: 0 1 2 3
```

As we can see in the preceding example, we are able to initialize our list using std::initializer_list. We can call the push, pop_front, and back functions. As we can see, sll2.pop_back only removed the element from sll2, and not sll. sll is still intact with all five elements. Hence, we can perform a deep copy as well.