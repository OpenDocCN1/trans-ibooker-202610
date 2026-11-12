If you ponder long enough, you may realize the logical way to remove double-counting in a union probability is to subtract the joint probability. This is known as the **sum rule of probability** and ensures every joint event is only counted once.

```
P(A OR B) = P(A) + P(B) - P(A AND B)
P(A OR B) = P(A) + P(B) - P(A) × P(B)
```

So going back to our example of calculating the probability of a heads or a “six”, we need to subtract the joint probability of getting a heads or a “six” from the union probability:

```
P(HEADS) = 1/2
P(6) = 1/6
P(A OR B) = P(A) + P(B) - P(A) × P(B)
P(HEADS OR 6) = 1/2 + 1/6 - (1/2 × 1/6) = .58333
```

Note that this formula also applies to mutually exclusive events. If the events are mutually exclusive where only one outcome A or B is allowed but not both, then the joint probability P(A AND B) is going to be 0, and therefore remove itself from the formula. You are then left with simply summing the events as we did earlier.

In summary, when you have a union probability between two or more events that are not mutually exclusive, be sure to subtract the joint probability so no probabilities are double-counted.

# Conditional Probability and Bayes Theorem

A probability topic that people easily get confused by is the concept of **conditional probability**, which is the probability of an event A occurring given event B has occurred. It is typically expressed as P(A GIVEN B) or P(A|B).

Let’s say some study makes a claim that 85% of cancer patients drank coffee. How do you react to this claim? Does this alarm you and make you want to abandon your favorite morning drink? Let’s first define this as a conditonal probability