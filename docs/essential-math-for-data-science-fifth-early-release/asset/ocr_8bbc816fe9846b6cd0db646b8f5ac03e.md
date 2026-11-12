# WHY EULER'S NUMBER IS USED SO MUCH

A property of Euler's number is its exponential function is a derivative to itself, which is convenient for exponential and logarithmic functions. We will learn about derivatives later in this chapter. In many applications where the base does not really matter, we pick the one that results in the simplest derivative, and that is Euler's number. That is also why it is the default base in many data science functions.

Here is how I like to discover Euler's number. Let's say you loan $100 to somebody with 20% interest annually. Typically interest will be compounded monthly, so the interest each month would be .20/12 = .01666. How much will the loan balance be after two years? To keep it simple, let's assume the loan does not require payments (and no payments are made) until the end of those two years.

Putting together the exponent concepts we learned so far (or perhaps pulling it out a finance textbook), we can come up with a formula to calculate interest. It consists of a balance $A$ for a starting investment $P$, interest rate $r$, time span $t$ (number of years), and periods $n$ (number of months in each year). Here it is as follows:

A = P \times (1 + \frac{r}{n})^{nt}

So if we were to compound interest every month, the loan would grow to $148.69 as calculated below:

A = P \times (1 + \frac{r}{n})^{nt}

100 \times \left(1 + \frac{.20}{12}\right)^{12 \times 2} = 148.6914618

If you want to do this in Python, try it out with the code in Example 1-15.

# Example 1-15. Calculating compound interest in Python