In these examples, we used 2% interest and started with 100 units of currency. You doubled your money after 35 years. That’s not very impressive!

# Exercise 5.20

Modify *compound_continuously* to round to two decimal places.

# Exercise 5.21

If the annual interest rate is 5%, how long will it take to double your money? What if it is 10%?

For more general exponential functions, we use *pow*.

```
y = math.pow(8.0, 3.0)
y
```

512.0

```
math.pow(y, 0.3333333333333333)
```

7.999999999999982

The second value approximates the cube root.

We use square roots often, so **math** includes a dedicated function for it.

```
math.sqrt(105.0)
```

10.246950765959598

We are working with real numbers and the argument to *sqrt* must be non-negative.

```
math.sqrt(-1.0)
```

ValueError: math domain error