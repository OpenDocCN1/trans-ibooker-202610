# Here's the output of the previous code:

```
Anyone has email address: true
Anyone has more than 10 email address: false
```

If, instead of checking that any element matches the given criteria, you want to check whether each and every element matches, use the allMatch. Anytime a call to the predicate returns false for an element, further processing of the collection is terminated by allMatch—short-circuiting for efficiency. In addition to getting an efficient implementation, a bigger benefit of using these methods is that we can focus on expressing our intent clearly and concisely and leave it to the runtime to figure out the details for efficient execution. Let's put this function to use in an example:

```
System.out.println("Everyone has at least one email address: " +
SAMPLE_DATA.stream()
.allMatch(person -> person.emailAddresses().size() > 0));

System.out.println("Everyone has zero or more email address: " +
SAMPLE_DATA.stream()
.allMatch(person -> person.emailAddresses().size() >= 0));
```

# The output of this evaluation is shown here:

```
Everyone has at least one email address: false
Everyone has zero or more email address: true
```

We're not restricted to only checking for any or all matches. We can also check if none of the elements satisfy a criteria using the noneMatch method.

In the examples we've seen so far, we got a single value or list as the output. But the Collectors class takes care of us even if we need to transform a collection into multiple collections.