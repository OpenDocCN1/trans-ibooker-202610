```
```cpp
{
    auto it = container.find(element);
    if(it == container.end())
        std::cout << element << " not found" << std::endl;
    else
        std::cout << element << " found with value=" << it->second <<
    std::endl;
}
```
```

4. Now, write the **main** function so that we can use **unordered_set** and **unordered_map**, and then perform various operations on it. We shall find, insert, and erase the elements:

```
```cpp
int main()
{
    std::cout << "Set example: " << std::endl;
    std::unordered_set<int> set1 = {1, 2, 3, 4, 5};
    std::cout << "Initial set1: ";
    print(set1);

    set1.insert(2);
    std::cout << "After inserting 2: ";
    print(set1);

    set1.insert(10);
    set1.insert(351);
    std::cout << "After inserting 10 and 351: ";
    print(set1);

    find(set1, 4);
    find(set1, 100);
    set1.erase(2);
    std::cout << "Erased 2 from set1" << std::endl;
    find(set1, 2);

    std::cout << "Map example: " << std::endl;
    std::unordered_map<int, int> squareMap;

    squareMap.insert({2, 4});
    squareMap[3] = 9;
    std::cout << "After inserting squares of 2 and 3: ";
```
```