# ## 1.2.1.2 Abelian Group

Now that you have the general idea of a group down, it’s time to move on to discuss specific types of groups. The first and easiest to understand is an _abelian group_ or commutative group has an additional property. That property being the commutative property: a + b = b + a if the operation is addition. Commutativity means ab = ba if the operation is multiplication.

This commutative requirement simply means that applying the group operation (whatever that operation might be) does not depend on the order of the group elements. In other words, whatever the group operation is, you can apply it to members of the group in any order you wish. To use a trivial example, consider the group of integers with the addition operation. Order does not matter:

```
4 + 2 = 2 + 4
```

Therefore, the set of integers with the operation of addition is an abelian group. As you can see, abelian groups are a subset of groups. They are groups with an additional restriction: the commutative property.

# ## 1.2.1.3 Cyclic Group

A _cyclic group_ is a group that has elements that are all powers of one of its elements. So, for example, if you start with element x, then the members of a cyclic group would be

```
x⁻², x⁻¹, x⁰, x¹, x², x³, ...
```

Of course, the other requirements for a group, discussed previously, would still apply to a cyclic group. It must be a set of numbers, with an operation, and its inverse. The basic element x is considered to be the generator of the group, because all other members of the group are derived from it. It is also referred to as a _primitive element_ of the group. Integers could be considered a cyclic group with 1 being the primitive element (i.e., generator). All integers can be expressed as a power of 1. This might seem like a rather trivial example, but it is also one that is easy to understand.

# ## 1.2.1.4 Rings

A _ring_ is an algebraic system consisting of a set, an identity element, two operations, and the inverse operation of the first operation. That is the formal definition of a ring, but it might seem a bit awkward to you at first read and therefore warrants a bit more explanation.

A ring is essentially just an abelian group that has a second operation. Previously, you learned that the set of integers with the addition operation form a group, and furthermore they form an abelian group. If you add the multiplication operation, then the set of integers with both the addition and the multiplication operations form a ring.

Note that you only have to have the inverse of the first operation. Therefore, if we consider the set of integers with addition as the first operation and multiplication as the second operation, we do have a ring. As an example, 4 + 5 = 9, which is still an integer (still in the ring). However, so is 4 – 5 = –1. The answer is still an integer, thus still in the ring. With multiplication, we don’t need the inverse (division) to always yield an integer, but any two integers multiplied together, such as 4 * 5 = 20, will always yield an integer.