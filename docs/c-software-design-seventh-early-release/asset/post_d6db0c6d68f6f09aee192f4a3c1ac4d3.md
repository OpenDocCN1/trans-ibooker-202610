
> *Delegate to Services: Has-A Trumps Is-A.*
> —Andrew Hunt & David Thomas

This is a general take-away for many design patterns. I suggest you keep this insight close at hand, as it will prove very useful in understanding the design patterns that you will see in the remainder of this book, but also to improve the quality of your implementations.

## GUIDELINE 20: FAVOR COMPOSITION OVER INHERITANCE

- Understand that inheritance is often overused and sometimes even misused;
- Keep in mind that inheritance creates a tight coupling;
- Realize that many design patterns are enabled by composition, not by inheritance.

## Guideline 21: Use Commands to Isolate What Things are Done

Before we get started with this guideline, let us try an experiment: Open your preferred email client and write an email to me. Add the following content: “I love your book! It keeps me up all night and makes me forget all my troubles.” Ok, great. Now click on send. Good job! Give me a second to check my emails… No, it is not here yet… No, still not here… Let us try again: Click on resend… No, nothing. Mmh, I guess some server must be down. Or all of my *Commands* simply failed: The `WriteCommand`, the `SendCommand`, the `ResendCommand`… How unfortunate. But despite this failed experiment, you now have a pretty good idea of another GoF design pattern: the *Command* design pattern.

## The Command Design Pattern Explained

## THE COMMAND DESIGN PATTERN

Intent: “Encapsulate a request as an object, thereby letting you parameterize clients with different requests, queue or log requests, and support undoable operations.” (GoF)

The *Command* design pattern focuses on the abstraction and isolation of work packages that (most often) are executed once and (usually) immediately. For that purpose, it recognizes the existence of different kinds of work packages as *variation point* and introduces the according abstraction that allows the easy implementation of new kinds of work packages. Figure 5-7 shows the original UML formulation, taken from the GoF book:

![](img/7b1b8ee332752e5702cc436a0cba7c1c_261_0.png)

Figure 5-7. The UML representation of the Command design pattern.

In this OO-based form, the Command pattern introduces an abstraction in form of the Command base class. This enables anyone to implement a new kind of ConcreteCommand. That ConcreteCommand can do anything, even perform an action on some kind of Receiver. The effect of a command is triggered via the abstract base class by a particular kind of Invoker.

As a concrete example for the Command design pattern, let us consider the following implementation of a calculator. The first code snippet shows the implementation of a CalculatorCommand base class, which represents the abstraction of a mathematical operation on a given integer:

```
//---- <CalculatorCommand.h> ----------------

class CalculatorCommand
{
public:
    virtual ~CalculatorCommand() = default;

    virtual int execute( int i ) const = 0;  ①
    virtual int undo( int i ) const = 0;  ②
};
```

The CalculatorCommand class expects derived classes to implement both the pure virtual execute() function (①) and the pure virtual undo() function (②). The expectation for undo() is, that it implements the necessary actions to reverse the effect of the execute() function.

The Add and Subtract classes both represent possible commands for a calculator and therefore implement the CalculatorCommand base class:

```
//---- <Add.h> ----------------

#include <CalculatorCommand.h>

class Add : public CalculatorCommand
{
public:
    explicit Add( int operand ) : operand_(operand) {}

    int execute( int i ) const override  ③
    {
        return i + operand_;
    }
    int undo( int i ) const override  ④
    {
        return i - operand_;
    }
}
```

```
private:
    int operand_{};
};
```

```
//---- <Subtract.h> ----------------

#include <CalculatorCommand.h>

class Subtract : public CalculatorCommand
{
public:
    explicit Subtract( int operand ) : operand_(operand) {}

    int execute( int i ) const override  ⑤
    {
        return i - operand_;
    }
    int undo( int i ) const override  ⑥
    {
        return i + operand_;
    }

private:
    int operand_{};
};
```

Add implements the execute() function by means of an addition operation (③) and the undo() function by means of a subtraction operation (④). Subtract implements the inverse (⑤ and ⑥).

Thanks to the CalculatorCommand hierarchy, the Calculator class itself can be kept rather simple:

```
//---- <Calculator.h> ----------------

#include <CalculatorCommand.h>
#include <stack>

class Calculator
{
public:
    void compute( std::unique_ptr<CalculatorCommand> command );  ⑦

void undoLast(); ⑧

int result() const;
void clear();

private:
    using CommandStack =
std::stack<std::unique_ptr<CalculatorCommand>>;

    int current_{}; ⑨
    CommandStack stack_; ⑩
};

//---- <Calculator.cpp> ----------------

#include <Calculator.h>

void Calculator::compute( std::unique_ptr<CalculatorCommand>
command ) ⑦
{
    current_ = command->execute( current_ );
    stack_.push( std::move(command) );
}

void Calculator::undoLast() ⑧
{
    if( stack_.empty() ) return;

    auto command = std::move(stack_.top());
    stack_.pop();

    current_ = command->undo(current_);
}

int Calculator::result() const
{
    return current_;
}

void Calculator::clear()
{
    current_ = 0;
    CommandStack{}.swap( stack_ ); // Clearing the stack
}
```

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

We first create a `calculator` (①) and a series of operations (⊕, ⊖ ⊖ and ⊗), which we apply one after another. After that we revert operation3 by means of the `undo()` operation, before we query the final result.

This design very nicely follows the SOLID principles.¹⁶ It adheres to the *Single-Responsibility Principle (SRP)* since the *variation point* has already been extracted by means of the *Command* design pattern. As a result, both `compute()` and `undo()` do not have to be virtual functions. The *SRP* also acts as an enabler for the *Open-Closed Principle (OCP)*, which allows us to add new operations without having to modify any existing code. Last, but not least, if the ownership for the `Command` base class is properly assigned to the high level, then the design also adheres to the *Dependency Inversion Principle (DIP)* (see Figure 5-8).

## High-level

```
class Calculator
{
public:
// ...
void compute( std::unique_ptr<CalculatorCommand> );
void undoLast();
// ...
};
```

```
class CalculatorCommand
{
public:
virtual ~CalculatorCommand() = default;

virtual int execute( int i ) const = 0;
virtual int undo( int i ) const = 0;
};
```

Architectural Boundary

```
class Addition
: public CalculatorCommand
{
public:
// ...
int execute( int i ) const override;
int undo( int i ) const override;
// ...
};
```

```
class Subtraction
: public CalculatorCommand
{
public:
// ...
int execute( int i ) const override;
int undo( int i ) const override;
// ...
};
```

# Low-level

Figure 5-8. Dependency graph for the Command design pattern.
