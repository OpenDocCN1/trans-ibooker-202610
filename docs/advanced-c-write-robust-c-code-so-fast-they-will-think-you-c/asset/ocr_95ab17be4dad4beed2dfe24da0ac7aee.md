```
{
    const char* sep = "";
    std::cout << prefix << " = [";
    for(auto n : values)
    {
        std::cout << sep << n;
        sep = ", ";
    }
    std::cout << "]\n";
}
```

```
int main()
{
    std::cout << "\n\n------ Functors with STL ------\n";
    std::vector<int> values{1,2,3,4,5};

    PrintVector("Before transform", values);
    std::transform(values.begin(), values.end(), values.begin(), Scaler(3));
    PrintVector("After transform", values);

    std::cout << "Complete.\n";
    return 0;
}
```

If we run this program, the output produced will look like the following:

```
------ Functors with STL ------
Before transform = [1, 2, 3, 4, 5]
After transform = [3, 6, 9, 12, 15]
Complete.
```

Figure 4.11: Program output showing Scaler transformed vectors