```
```cpp
else if (mid_element == P && mid_element.is_vaccinated() == true)
    return false;
else if (mid_element > P)
    std::advance(last, -mid_element_index);
if (mid_element < P)
    std::advance(first, mid_element_index);

// Student not found in the sequence and therefore should be
vaccinated
    if (range_length == 1)
        return true;
}
}
```
```

6. Finally, the driver code is implemented as follows:

```
```cpp
int main()
{
    // Generate a Student to search
    auto p = generate_random_Student(1000);

    search_test(1000, p);
    search_test(10000, p);
    search_test(100000, p);

    return 0;
}
```
```

> > **Note**
> >
> > Since we are randomizing values in step 3, your output may vary from the expected output shown for this activity.