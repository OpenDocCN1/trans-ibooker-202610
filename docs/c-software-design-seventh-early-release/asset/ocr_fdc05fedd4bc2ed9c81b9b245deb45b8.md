The only functions we need for the computing activities are compute() (⑦) and undoLast() (⑧). The compute() function is passed a CalculatorCommand instance, immediately executes it to update the current value, (⑨), and stores it on the stack (⑩). The undoLast() function reverts the last executed command by popping it from the stack and calling undo().

The main() function combines all of the pieces:

```
//---- <Main.cpp> ----------------

#include <Calculator.h>
#include <Add.h>
#include <Subtract.h>
#include <cstdlib>

int main()
{
    Calculator calculator{};    ⑪

    auto op1 = std::make_unique<Add>( 3 );    ⑫
    auto op2 = std::make_unique<Add>( 7 );    ⑬
    auto op3 = std::make_unique<Subtract>( 4 );    ⑭
    auto op4 = std::make_unique<Subtract>( 2 );    ⑮

    calculator.compute( std::move(op1) );  // Computes 0 + 3,
    stores and returns 3
    calculator.compute( std::move(op2) );  // Computes 3 + 7,
    stores and returns 10
    calculator.compute( std::move(op3) );  // Computes 10 - 4,
    stores and returns 6
    calculator.compute( std::move(op4) );  // Computes 6 - 2,
    stores and returns 4

    calculator.undoLast();  // Reverts the last operation,
                            // stores and returns 6

    int const res = calculator.result();  // Get the final result:
    6

    // ...

    return EXIT_SUCCESS;
}
```