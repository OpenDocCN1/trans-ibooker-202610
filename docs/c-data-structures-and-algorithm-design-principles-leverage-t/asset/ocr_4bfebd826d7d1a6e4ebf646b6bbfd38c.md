5. Let's write the lookup function to see whether an element is present in the map:

```
bool find(uint value)
{
    int n = data.size();
    auto& entries = data[value % n];
    return std::find(entries.begin(), entries.end(), value) != entries.end();
}
```

As we can see, our lookup seems faster than conventional methods, but not as fast as it was earlier. This is because now, it is also dependent on the data, as well as the value of n. We'll come back to this point again after this exercise.

6. Let's implement a function to remove elements:

```
void erase(uint value)
{
    int n = data.size();
    auto& entries = data[value % n];
    auto iter = std::find(entries.begin(), entries.end(), value);

    if(iter != entries.end())
    {
    entries.erase(iter);
        std::cout << "Removed " << value << std::endl;
    }
}
};
```

7. Let's write the same main function as in the previous exercise and look at the difference:

```
int main()
{
    hash_map map(7);

    auto print = [&](int value)
    {
```