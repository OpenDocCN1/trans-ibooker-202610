
## Operators

There are some mathematical operations that can be executed on all numerical variables of the same types, that is, `+`, `-`, `*`, and `/`. The sum operation is also used for concatenating strings. `++` and `--` are shorthand for incrementing or decrementing a number by one. `+=`, `-=`, `*=`, and `/=` execute the operation before the equals sign between what's before and what's after the operator and assigns it to the variable on the left. These four operations produce a value of the same type of the variables involved; there are also other comparison operators that are specific to numbers: `<`, `<=`, `>`, and `>=`.

Some operations are exclusive for integers and produce other integers: `%`, `&`, `|`, `^`, `&^`, `<<`, and `>>`.

Others are just for Booleans and produce another Boolean: `&&`, `||`, and `!`.

One operator is channel only, `<-`, and it's used to receive values from or send them to a channel.

For all non-pointer variables, it is also possible to use `&`, the reference operator, to obtain the variable address that can be assigned to a pointer variable. The `*` operator makes it possible to execute a dereference operation on a pointer and obtain the value of the variable indicated by it:

| Operator | Name | Description | Example |
| --- | --- | --- | --- |
| `=` | Assignment | Assigns the value to a variable | `a = 10` |
| `:=` | Declaration and assignment | Declares a variable and assigns a value to it | `a := 0` |
| `==` | Equals | Compares two variables, returns a Boolean if they are the same | `a == b` |
| `!=` | Not equals | Compares two variables, returns a Boolean if they are different | `a != b` |
| `+` | Plus | Sum between the same numerical type | `a + b` |
| `-` | Minus | Difference between the same numerical type | `a - b` |
| `*` | Times | Multiplication between the same numerical type | `a * b` |
| `/` | Divided | Division between the same numerical type | `a / b` |
| `%` | Modulo | Remainder after division of the same numerical type | `a % b` |
| `&` | AND | Bit-wise AND | `a & b` |
| `&^` | Bit clear | Bit clear | `a &^ b` |
| `<<` | Left shift | Bit shift to the left | `a << b` |
| `>>` | Right shift | Bit shift to the right | `a >> b` |
| `&&` | AND | Boolean AND | `a && b` |
| `||` | OR | Boolean OR | `a || b` |
| `!` | NOT | Boolean NOT | `!a` |
| `<-` | Receive | Receive from a channel | `<-a` |
| `->` | Send | Send to a channel | `a <- b` |
| `&` | Reference | Returns the pointer to a variable | `&a` |
| `*` | Dereference | Returns the content of a pointer | `*a` |

## Casting

Converting a type into another type is an operation called **casting**, which works slightly differently for interfaces and concrete types:

*   Interfaces can be casted to a concrete type that implements it. This conversion can return a second value (a Boolean) and show whether the conversion was successful or not. If the Boolean variable is omitted, the application will panic on a failed casting.
*   With concrete types, casting can happen between types that have the same memory structure, or it can happen between numerical types:

```
type N [2]int                // User defined type
var n = N{1,2}
var m [2]int = [2]int(N)     // since N is a [2]int this casting is possible

var a = 3.14                 // this is a float64
var b int = int(a)           // numerical types can be casted, in this case a will be rounded to 3

var i interface{} = "hello"  // a new empty interface that contains a string
x, ok := i.(int)             // ok will be false
y := i.(int)                 // this will panic
z, ok := i.(string)          // ok will be true
```

There's a special type of conditional operator for casting called **type switch** which allows an application to attempt multiple casts at once. The following is an example of using `interface{}` to check out the underlying value:

```
func main() {
    var a interface{} = 10
    switch a.(type) {
    case int:
        fmt.Println("a is an int")
    case string:
        fmt.Println("a is a string")
    }
}
```

## Scope

The variables have a scope or visibility that is also connected to its lifetime. This can be one of the following:

*   **Package**: The variable is visible in all the packages; if the variable is exported, it is also visible from other packages.
*   **Function**: The variable is visible inside the function that declares it.
*   **Control**: The variable is visible inside the block in which it's defined.

The visibility goes down, going from package to block. Since blocks can be nested, outer blocks don't have visibility over the variables of inner blocks.

Two variables in the same scope cannot have the same name, but a variable of an inner scope can reuse an identifier. When this happens, the outer variable is not visible in the inner scope – this is called **shadowing**, and it needs to be kept in mind in order to avoid issues that are hard to identify, such as the following:

```
// this exists in the outside block
var err error
// this exists only in this block, shadows the outer err
if err := errors.New("Doh!"); err != nil {
    fmt.Println(err)           // this is not changing the outer err
}
fmt.Println(err)               // outer err has not been changed
```

## Constants

Go doesn't have immutability for its variables, but defines another type of immutable value called constant. This is defined by the `const` keyword (instead of `var`), and they are values that cannot change. These values can be base types and custom types, which are as follows:

*   Numeric (integer, `float`)
*   Complex
*   String
*   Boolean

The value that's specified doesn't have a type when it's assigned to a variable. There's an automatic conversion of both numeric types and string-based types, as shown in the following code:

```
const PiApprox = 3.14

var PiInt int = PiApprox // 3, converted to integer
var Pi float64 = PiApprox // is a float

type MyString string

const Greeting = "Hello!"

var s1 string = Greeting   // is a string
var s2 MyString = Greeting // string is converted to MyString
```

Numerical constants are very useful for mathematical operations because they are just regular numbers, so they can be used with any numerical variable type.

## Functions and Methods

Functions in Go are identified by the `func` keyword, followed by an identifier, eventual arguments, and return values. Functions in Go can return more than one value at a time. The combination of arguments and returned types is referred to as a **signature**, as shown in the following code:

```
func simpleFunc()
func funcReturn() (a, b int)
func funcArgs(a, b int)
func funcArgsReturns(a, b int) error
```

The part between brackets is the function body, and the `return` statement can be used inside it for an early interruption of the function. If the function returns values, then the return statement must return values of the same type.

The `return` values can be named in the signature; they are zero value variables and if the `return` statement does not specify other values, these values are the ones that are returned:

```
func foo(a int) int {        // no variable for returned type
    if a > 100 {
        return 100
    }
    return a
}

func bar(a int) (b int) {    // variable for returned type
    if a > 100 {
        b = 100
        return               // same as return b
    }
    return a
}
```

Functions are first-class types in Go and they can also be assigned to variables, with each signature representing a different type. They can also be anonymous; in this case, they are called **closures**. Once a variable is initialized with a function, the same variable can be reassigned with another function with the same signature. Here's an example of assigning a closure to a variable:
