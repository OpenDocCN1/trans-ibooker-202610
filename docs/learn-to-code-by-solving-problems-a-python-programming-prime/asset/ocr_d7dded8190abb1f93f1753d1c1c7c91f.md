quadratic-time algorithm to arise even in the absence of such nested loops. We can find such an example in our first solution to the Email Addresses problem, Listing 8-2. I’ve reproduced that solution here:

```
```python
# clean function not shown

for dataset in range(10):
    n = int(input())
    addresses = []
    for i in range(n):
        address = input()
        ❶ address = clean(address)
        ❷ if not address in addresses:
            addresses.append(address)

print(len(addresses))
```
```

We’ll let *n* be the maximum number of email addresses that we see in our 10 test cases. The outer `for` loop iterates 10 times; the inner `for` loop iterates at most *n* times. We’re therefore processing at most 10*n* email addresses, which is linear in *n*.

Cleaning an email address ❶ takes a constant number of steps, so we don’t need to worry about that. But this is still *not* a linear-time algorithm, because each iteration of the inner `for` loop takes more than a constant number of steps. Specifically, checking whether an email address is already in our list ❷ takes work proportional to the number of email addresses already in the list, because Python has to search through the list. That’s a linear-time operation on its own! So we’re processing 10*n* email addresses, each of which requires *n* work, for a total of 10*n*², or quadratic-time, work. This quadratic-time performance is precisely why we received a time limit exceeded error with this code, leading us to use a set rather than a list.

# Cubic Time

If one loop can lead to linear time, and two nested loops can lead to quadratic time, then what about three nested loops? Three nested loops, each of which depends on *n*, leads to a *cubic-time* algorithm. In big O notation, we say that a cubic-time algorithm is *O(n*³*).*

If you thought quadratic-time algorithms were slow, wait till you see how slow cubic-time algorithms are. Suppose that *n* is 1,000. We already know that a linear-time algorithm will take about 1,000 steps and that a