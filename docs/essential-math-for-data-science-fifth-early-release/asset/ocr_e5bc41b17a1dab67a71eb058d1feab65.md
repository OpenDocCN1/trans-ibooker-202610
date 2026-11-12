```
P (A|B) = \frac{P(B|A)P(A)}{P(B)}
```

If we plug the information we already have into this formula, we can solve for the probability someone has cancer given they drink coffee:

```
P (A|B) = \frac{P(B|A)*P(A)}{P(B)}

P (Cancer|Coffee) = \frac{P(Coffee|Cancer)*P(Coffee)}{P(Cancer)}

P (Cancer|Coffee) = \frac{.85*.005}{.65} = .0065
```

If you want to calculate this in Python, check out Example 2-1.

# Example 2-1. Using Bayes Theorem in Python

```
p_coffee_drinker = .65
p_cancer = .005
p_coffee_drinker_given_cancer = .85
p_cancer_given_coffee_drinker = p_coffee_drinker_given_cancer *
    p_cancer / p_coffee_drinker

# prints 0.006538461538461539
print(p_cancer_given_coffee_drinker)
```

Okay, so the probability someone has cancer given they are a coffee drinker is only 0.65%! This number is very different than the probability someone is a coffee drinker given they have cancer, which is 85%. Now do you see why the direction of the condition matters? Bayes Theorem is helpful for this reason. It can also be used to chain several conditional probabilities together to keep updating our beliefs based on new information.