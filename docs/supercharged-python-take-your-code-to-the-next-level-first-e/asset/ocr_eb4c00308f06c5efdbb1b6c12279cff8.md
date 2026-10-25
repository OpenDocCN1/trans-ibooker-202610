# # 10. Decimal, Money, and Other Classes

In the movie _Superman III_, a computer genius figures out that if he can steal fractions of a penny in each transaction done by a bank, transferring that fraction to his own account, he can enrich himself. That’s because a fraction of a penny multiplied by millions of transactions a day adds up to a fortune.

That’s why bankers care about rounding errors. They’re aware that fractions of a penny can add up. Precise amounts matter.

Early on, electronic computers were used for commercial purposes, making it important to record dollars and cents (or any currency) precisely, without errors. That’s what this chapter is all about: utilizing ways of tracking data especially suited for financial purposes. This chapter will present the `Decimal` class and a Money class that we’ll build up to.

We’ll also look briefly at the `Fraction` and the built-in `complex` classes, although the latter is used mainly with advanced math and scientific applications.

# ## 10.1 OVERVIEW OF NUMERIC CLASSES