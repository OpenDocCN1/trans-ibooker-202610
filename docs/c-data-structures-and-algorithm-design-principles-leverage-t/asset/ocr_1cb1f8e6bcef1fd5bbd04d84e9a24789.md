```
if(map.find(value))
    std::cout << value << " found in the hash map";
else
    std::cout << value << " NOT found in the hash map";
std::cout << std::endl;
};
```

8. Let's use the insert and erase functions on map:

```
map.insert(2);
map.insert(25);
map.insert(290);

map.insert(100);
map.insert(55);

print(100);
map.erase(2);
}
```

Here's the output of our program:

```
Inserted 2
Inserted 25
Inserted 290
Inserted 100
Inserted 55
100 found in the hash map
Removed 2
```

As we can see, the values are not overwritten because we can store any number of values in the list. Hence, our output is completely accurate and reliable.