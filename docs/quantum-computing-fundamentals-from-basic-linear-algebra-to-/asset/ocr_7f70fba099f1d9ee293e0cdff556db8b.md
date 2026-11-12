Abstract algebra concerns itself with structures just like this. These structures (groups, rings, fields, etc.) have a set of numbers and certain operations that can be performed on those numbers. The only allowed operations in a given structure are those whose result would still be within the prescribed set of numbers. This discussion of algebraic groups will be applied in the discussion of vector spaces in section 1.4 later in the chapter.

Don't be overly concerned with the term _abstract algebra_. There are certainly practical applications of abstract algebra. In fact, some sources prefer to call this _modern algebra_; however, because it dates back a few centuries, even that might be a misnomer. So, let us now examine some of these structures.

# ## 1.2.1.1 Groups

A _group_ is an algebraic system consisting of a set, an identity element, one operation, and its inverse operation. Let us begin with explaining what an identity element is. An identity element is simply some number within a set that you can use to apply some operation to any other number in the set, and the other number will still be the same. Put more mathematically,

```
a * I = a
```

where * is any operation that we might specify, not necessarily multiplication. An example would be with respect to the addition operation, zero is the identity element. You can add zero to any member of any given group, and you will still have that same number. With respect to multiplication, 1 is the identity element. Any number multiplied by 1 is still the same number.

There are four properties any group must satisfy:

+   - **Closure:** Closure is the simplest of these properties. It simply means that an operation performed on a member of the set will result in a member of the set. This is what was discussed a bit earlier in this section. It is important that any operations allowed on a particular set will result in an answer that is also a member of the set.

+   - **Associativity:** The associative property just means that you can rearrange the elements of a particular set of values in an operation without changing the outcome. For example, (2 + 2) + 3 = 7. Even if I change the order and instead write 2 + (2 + 3), the answer is still 7. This is an example of the associative property.

+   - **Identity:** The identity element was already discussed.

+   - **Invertibility:** The invertibility property simply means that a given operation on a set can be inverted. As we previously discussed, subtraction is the inversion of addition; division is the inversion of multiplication.

Think back to the example of the set of integers. Integers constitute a group. First, there is an identity element, zero. There is also one operation (addition) and its inverse (subtraction). Furthermore, you have closure. Any element of the group (any integer) added to any other element of the group (any other integer) still produces a member of the group (the answer is still an integer).