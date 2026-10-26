3. Enter the **main** function and define the integer variable that will be used for reading user input:

```
int integer;
```

4. Create an infinite while loop. Inside the loop, ask the user to enter a decimal value. Read the input as a decimal integer. Pass it to the **displayInBases** function. Next, ask the user to enter a hexadecimal value. Read the input as a hexadecimal integer. Pass it to the **displayInBases** function. Finally, ask the user to enter an octal value. Read the input as an octal integer. Pass it to the **displayInBases** function:

```
int main(int argc, char **argv)
{
    int integer;

    while(true)
    {
        std::cout << "Enter the decimal value: ";
        std::cin >> std::dec >> integer;
        displayInBases(integer, BASE::DECIMAL);

        std::cout << "Enter the hexadecimal value: ";
        std::cin >> std::hex >> integer;
        displayInBases(integer, BASE::HEXADECIMAL);

        std::cout << "Enter the octal value: ";
        std::cin >> std::oct >> integer;
        displayInBases(integer, BASE::OCTAL);
    }

    return 0;
}
```