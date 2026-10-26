Everything after **[captures]** is the same as for a normal function definition, except that the name is missing. Lambdas are a convenient way of defining a short method (just a few lines) at the location it will be used. The lambda is often passed as an argument and will typically not be re-used. It should also be noted that a lambda can be assigned to a variable (usually declared with auto).

We can re-write the previous program where we used a **Scaler** class to use a lambda to achieve the same outcome:

```
#include <iostream>

#include <vector>

#include <algorithm>

void PrintVector(const char* prefix, std::vector<int>& values)
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

int main()
{
    std::cout << "\n\n------ Lambdas with STL ------\n";
    std::vector<int> values{1,2,3,4,5};

    PrintVector("Before transform", values);
    std::transform(values.begin(), values.end(), values.begin(),
    [] (int n) {return 5*n;}
    );
```