6. Now, let's write a function to print all the mappings for logging:

```
```cpp
void printURLs() const
{
    for(const auto& entry: data)
    {
        std::cout << entry.first << " -> " << entry.second << std::endl;
    }
    std::cout << std::endl;
};
```
```

7. Now, write the **main** function so that we can use this service:

```
```cpp
int main()
{
    URLService service;

    if(service.registerURL("https://www.packtpub.com/eu/big-data-and-business-intelligence/machine-learning-r-third-edition", "https://ml-r-v3"))
    {
        std::cout << "Registered https://ml-r-v3" << std::endl;
    }
    else
    {
        std::cout << "Couldn't register https://ml-r-v3" << std::endl;
    }

    if(service.registerURL("https://www.packtpub.com/eu/virtualization-and-cloud/hands-aws-penetration-testing-kali-linux", "https://aws-test-kali"))
    {
        std::cout << "Registered https://aws-test-kali" << std::endl;
    }
    else
    {
        std::cout << "Couldn't register https://aws-test-kali" << std::endl;
    }
```
```