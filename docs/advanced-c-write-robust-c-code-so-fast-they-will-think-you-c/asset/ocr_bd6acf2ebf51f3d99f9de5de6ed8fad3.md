2. Next, declare a template **formattingPrint** function that has a template parameter called **FloatingPoint** and takes a parameter variable of this type. Next, store the previous precision in an auto variable by calling the **precision()** function in the **std::cout** object. Then, display the given number in different formats in the Terminal: with the point, without a point, and in the fixed, scientific, hexfloat, and defaultfloat formats. Next, in the for loop, from 0 to 22, display the given number with precision and the size of the loop counter. After the loop exits, set the precision back using the value we stored earlier:

```
```cpp
template< typename FloatingPoint >
void formattingPrint(const FloatingPoint number)
{
    auto precision = std::cout.precision();
    std::cout << "Default formatting with point: "
              << std::showpoint << number << std::endl
              << "Default formatting without point: "
              << std::noshowpoint << number << std::endl
              << "Fixed formatting: "
              << std::fixed << number << std::endl
              << "Scientific formatting: "
              << std::scientific << number << std::endl
              << "Hexfloat formatting: "
              << std::hexfloat << number << std::endl
              << "Defaultfloat formatting: "
              << std::defaultfloat << number << std::endl;

    for (int i = 0; i < 22; i++)
    {
        std::cout << "Precision: " << i
                  << ", number: " << std::setprecision(i)
                  << number << std::endl;
    }
    std::cout << std::setprecision(precision);
}
```
```