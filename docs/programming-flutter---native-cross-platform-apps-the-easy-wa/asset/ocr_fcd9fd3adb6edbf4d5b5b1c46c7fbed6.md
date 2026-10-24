# Functions

Dart supports top-level functions, meaning you can define functions outside any class definition that can be called from anywhere in the code.

A Dart function is defined in the following way:

```
returnType functionName(parameters) {
    blockOfInstructions;
    return returnValue;
}
```

where:

+   - returnType is the data type, see the section *Conversion Between Native Java/Apple and Dart Data Types*, the function will return.

+   - The parameters, explained in the next paragraph, which will be the data the function will use to do what it needs to do. Parameters are variables and their value is set using function call arguments. If you are not familiar with functions and function calls, the example that follows is designed to give a real, working example of a few functions that take advantage of other functions.

+   - In the curly braces is where you add the block of instructions (one or more lines) which, if the function has any return type other than void, includes a return instruction, which is used to return a value to the calling code.

For example, the following function is very simple:

```
double square(double n) {
    return n*n;
}
```

but it can be used inside a function to calculate gravitational attraction, which is slightly more complex, with more parameters:

```
// We'll see how to set all of those arguments soon
double calculateGravitationalAttraction(double mass1, double mass2, double distance)
{
    const G = 6.67e-11; // universal gravitation constant
    /*
    * Calling the square function using distance as
    * the only argument (n will be equal to
    * the value in the distance variable)
    * and saving the return value
    * to the squaredDistance variable
    */
```