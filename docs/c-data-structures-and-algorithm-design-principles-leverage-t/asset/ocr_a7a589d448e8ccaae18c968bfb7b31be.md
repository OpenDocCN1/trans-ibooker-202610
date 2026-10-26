Now, let's see how we can remove the elements from the list:

```
std::forward_list<int> fwd_list = {1, 2, 3, 4, 5};
```

```
fwd_list.pop_front();
// list becomes {2, 3, 4, 5}
```

```
auto it = fwd_list.begin();
```

```
fwd_list.erase_after(it);
// list becomes {2, 4, 5}
```

```
fwd_list.erase_after(it, fwd_list.end());
// list becomes {2}
```

Let's explore what other operations we can do with forward_list in the following section.

# Other Operations on forward_list

Apart from the erase functions to delete elements based on its position determined by iterators, forward_list also provides the remove and remove_if functions to remove elements based on their values. The remove function takes a single parameter – the value of the elements to be removed. It removes all the elements that match the given element based on the equality operator defined for the type of the value. Without the equality operator, the compiler doesn't allow us to call that function and throws a compilation error. Since remove only deletes the elements based on the equality operator, it is not possible to use it for deletion based on other conditions, since we can't change the equality operator after defining it once. For a conditional removal, forward_list provides the remove_if function. It takes a predicate as a parameter, which is a function taking an element of the value type as a parameter, and a Boolean as the return value. So, all the elements for which the predicate returns true are removed from the list. With the latest C++ versions, we can easily specify the predicate with lambdas as well. The following exercise should help you to understand how to implement these functions.