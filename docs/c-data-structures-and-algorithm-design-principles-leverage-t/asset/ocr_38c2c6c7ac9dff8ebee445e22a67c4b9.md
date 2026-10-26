```
```cpp
{
    std::cout << "No position named " << manager << std::endl;
    return false;
}
if(managerNode->first && managerNode->second)
{
    std::cout << manager << " already has 2 subordinates." << std::endl;
    return false;
}
if(!managerNode->first)
    managerNode->first = new node{subordinate, NULL, NULL};
else
    managerNode->second = new node{subordinate, NULL, NULL};
return true;
}
};
```
```

As we can see, the function returns a Boolean, indicating whether we can insert the node successfully or not.

+   7. Now, let's use this code to create a tree in the **main** function:

```
```cpp
int main()
{
    auto tree = org_tree::create_org_structure("CEO");
    if(tree.addSubordinate("CEO", "Deputy Director"))
        std::cout << "Added Deputy Director in the tree." << std::endl;
    else
        std::cout << "Couldn't add Deputy Director in the tree" << std::endl;

    if(tree.addSubordinate("Deputy Director", "IT Head"))
        std::cout << "Added IT Head in the tree." << std::endl;
    else
        std::cout << "Couldn't add IT Head in the tree" << std::endl;
    if(tree.addSubordinate("Deputy Director", "Marketing Head"))
        std::cout << "Added Marketing Head in the tree." << std::endl;
    else
        std::cout << "Couldn't add Marketing Head in the tree" << std::endl;

    if(tree.addSubordinate("IT Head", "Security Head"))
```
```