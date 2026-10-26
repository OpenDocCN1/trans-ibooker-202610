9 quarters, 5 nickels -> $2.25 + $0.25

8 quarters, 4 dimes, 2 nickels -> $2.00 + $0.40 + $0.10

8 quarters, 3 dimes, 4 nickels -> $2.00 + $0.30 + $0.20

8 quarters, 2 dimes, 6 nickels -> $2.00 + $0.20 + $0.30

With these parameters, the problem is rather straightforward and can be solved by simply trying all the available combinations of money until a sum matching $2.50 is found. But what if the change that's required is $337.81, and the register contains 100 banknotes/coins divided into denominations of $20.00, $10.00, $5.00, $1.00, $0.25, $0.10, $0.05, and $0.01? We can clearly see that trying every possible sum becomes quite impractical as the complexity increases. This is an example of a classic problem known as the subset sum problem.

In its most basic form, the subset sum problem asks the following question: given a set of non-negative integers, S, and an integer, x, is there a subset of S's elements whose sum is equal to x? Take a look at the following example:

S = { 13, 79, 45, 29 }

x = 42 -> True (13 + 29)

x = 25 -> False

Using the preceding set as an example, we can find the following 16 subsets:

{ }

{ 13 }

{ 79 }

{ 45 }

{ 29 }

{ 13, 79 }

{ 13, 45 }

{ 13, 29 }

{ 79, 45 }

{ 79, 29 }

{ 45, 29 }