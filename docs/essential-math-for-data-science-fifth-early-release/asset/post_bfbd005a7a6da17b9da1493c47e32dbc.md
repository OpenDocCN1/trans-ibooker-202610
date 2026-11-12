
## Appendix A. Appendix

### Using Latex Rendering with SymPy

As you get more comfortable with mathematical notation, it can be helpful to take your SymPy expressions and display them in mathematical notation.

The quickest way to do this is to use the `latex()` function SymPy on your expression and then copy the result to a Math Latex viewer.

Example A-1 is an example that takes a simple expression and turns it into a latex string. Of course we can take the result of derivatives, integrals, and other SymPy operations and render that as Latex too. But let’s keep the example straightforward.

### Example A-1. Using SymPy to convert an expression into Latex

```
from sympy import *
x,y = symbols('x y')
z = x**2 / sqrt(2*y**3 - 1)
print(latex(z))
# prints
# \frac{x^{(2)}}{\sqrt{2 y^{(3)} - 1}}
```

This \frac{x^{(2)}}{\sqrt{2 y^{(3)} - 1}} string is formatted math Latex, and there are a variety of tools and document formats that can be adapted to support it. But to simply render the math latex, go to a latex equation editor. Here are two different ones I use online:

- https://lateditor.lagrida.com/
- https://latex.codecogs.com/

In Figure A-1 I use the Lagrida’s Latex editor to render the mathematical expression.

![](img/8e0dcc1636d032e7728a07e97751e446_407_0.png)

![](img/8e0dcc1636d032e7728a07e97751e446_407_1.png)

Figure A-1. Using a math latex editor to view the SymPy Latex output

If you want to save the copy/paste step, you can append the latex directly as an argument to the Codecogs Latex editor URL as shown in Example A-2, and it will show the rendered math equation in your browser.

### Example A-2. Open a math latex rendering using CodeCogs.

```python
import webbrowser
from sympy import *

x,y = symbols('x y')

z = x**2 / sqrt(2*x*y**3 - 1)

webbrowser.open("https://latex.codecogs.com/png.image?\dpi(200)" + latex(z))
```

If you use Jupyter, you can also use plugins to render Math Latex.

### Binomial Distribution from Scratch

If you want to implement a binomial distribution from scratch, here are all the parts you need in Example A-3.

### Example A-3. Building a Binomial Distribution from scratch

```python
# Factorials multiply consecutive descending integers down to 1
# EXAMPLE: 5! = 5 * 4 * 3 * 2 * 1
def factorial(n: int):
    f = 1
    for i in range(n):
        f *= (i + 1)
    return f

# Generates the coefficient needed for the binomial distribution
def binomial_coefficient(n: int, k: int):
    return factorial(n) / (factorial(k) * factorial(n - k))

# Binomial distribution calculates the probability of k events out of n trials
# given the p probability of k occurring
def binomial_distribution(k: int, n: int, p: float):
    return binomial_coefficient(n, k) * (p ** k) * (1.0 - p) ** (n - k)

# 10 trials where each has 90% success probability
n = 10
p = 0.9

for k in range(n + 1):
    probability = binomial_distribution(k, n, p)
    print("{0} - {1}".format(k, probability))
```

Using the factorial() and the binomial_coefficient(), we can build a binomial distribution function from scratch. The factorial function multiplies a consecutive range of integers from 1 to n. For example, a factorial of 5! would be 1*2*3*4*5 = 120.

The binomial coefficient function allows us to select k outcomes from n possibilities with no regard for ordering. If you have k = 2 and n = 3, that would yield sets (1,2) and (1,2,3), respectively. Between those two sets, the possible distinct combinations would be (1,3), (1,2), and (2,3). That is 3 combinations so that would be a binomial coefficient of 3. Of course, using the `binomial_coefficient()` function we can avoid all that permutation work by using factorials and multiplication instead.

When implementing `binomial_distribution()`, notice how we take the binomial coefficient and multiply it by the probability of success p occurring k times (hence the exponent). We then multiply it by the opposite case: the probability of failure 1.0 - p occurring n - k times. This allows us to account for the probability p of an event occurring versus not occurring across several trials.

### Beta Distribution from Scratch

If you are curious how to build a beta distribution from scratch, you will need to re-use the `factorial()` function we used for the binomial distribution as well as the `approximate_integral()` function we built in the last chapter.

Just like we did in the previous chapter, we pack rectangles under the curve for the range we are interested in as shown in Figure A-2.

![](img/8e0dcc1636d032e7728a07e97751e446_409_0.png)

Figure A-2. Packing rectangles under the curve to find the area/probability

This is just using 6 rectangles and we will get better accuracy if we were to use more rectangles. Let's implement the `beta_distribution()` from scratch and integrate 1000 rectangles between .9 and 1.0 as shown in Example A-4.

### Example A-4. Beta distribution from scratch

```python
# Factorials multiply consecutive descending integers down to 1
# EXAMPLE: 5! = 5 * 4 * 3 * 2 * 1
def factorial(n: int):
    f = 1
    for i in range(n):
        f *= (i + 1)
    return f
```

```python
def approximate_integral(a, b, n, f):
    delta_x = (b - a) / n
    total_sum = 0

    for i in range(1, n + 1):
        midpoint = 0.5 * (2 * a + delta_x * (2 * i - 1))
        total_sum += f(midpoint)

    return total_sum * delta_x
```

```python
def beta_distribution(x: float, alpha: float, beta: float) -> float:
    if x < 0.0 or x > 1.0:
        raise ValueError("x must be between 0.0 and 1.0")

    numerator = x ** (alpha - 1.0) * (1.0 - x) ** (beta - 1.0)
    denominator = (1.0 * factorial(alpha - 1) * factorial(beta - 1)) / (1.0 *
    factorial(alpha + beta - 1))

    return numerator / denominator
```

```python
greater_than_90 = approximate_integral(a=.90, b=1.0, n=1000, f=lambda x:
beta_distribution(x, 8, 2))
less_than_90 = 1.0 - greater_than_90

print("GREATER THAN 90%: {}, LESS THAN 90%: {}".format(greater_than_90,
less_than_90))
```

You will notice with the `beta_distribution()` function, we provide a given probability x, an alpha value quantifying successes, and a beta value quantifying failures. The function will return how likely we are to observe a given likelihood x. But again, to get a probability of observing probability x we need to find an area within a range of x values. Thankfully, we have our `approximate_integral()` function defined and ready to go from the last chapter. We can calculate the probability that the success rate is greater than 90%, as well as less than 90% as shown in the last few lines above.

### Deriving Bayes Theorem

If you want to understand why Bayes Theorem works rather than take my word for it, let's do a thought experiment. Let's say I have a population of 100,000 people. Multiply it with our given probabilities to get the count of people who drink coffee and the count of people who have cancer.

```
N = 100,000

P(Coffee Drinker) = .65

P(Cancer) = .005

Coffee Drinkers = 65,000

Cancer Patients = 500
```

We have 65,000 coffee drinkers and 500 cancer patients. Now of those 500 cancer patients, how many are coffee drinkers? We were provided with a conditional probability P(Coffee|Cancer) we can multiply against those 500 people, which should give us 425 cancer patients who drink coffee.

```
P(Coffee Drinker|Cancer) = .85

Coffee Drinkers with Cancer = 500×.85 = 425
```

Now what is the percentage of coffee drinkers who have cancer? What two numbers do we divide? We already have the number of people who drink coffee AND have cancer. Therefore we proportion that against the total number of coffee drinkers.

```
P(Cancer|Coffee Drinker) = \frac{Coffee Drinkers with Cancer}{Coffee Drinkers}

P(Cancer|Coffee Drinker) = \frac{425}{65,000}

P(Cancer|Coffee Drinker) = 0.006538
```

Hold on a minute, did we just flip our conditional probability? Yes we did! We started with P(Coffee Drinker|Cancer) and ended up with P(Cancer|Coffee Drinker). By taking two subsets of the population (65,000 coffee drinkers and 500 cancer patients), and then applying a joint probability using the conditional probability we had, we ended up with 425 people in our population who both drink coffee and have cancer. We then divide that by the number of coffee drinkers to get the probability of cancer given one's a coffee drinker.
