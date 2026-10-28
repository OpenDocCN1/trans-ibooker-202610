To get the key-value pairs as tuples, use *items*.

```
for model, year in guitars.items():
    print(f"The {model} was introduced in {year}")

The Fender Stratocaster was introduced in 1954
The Fender Telecaster was introduced in 1950
The Gibson Les Paul was introduced in 1952
The Gibson Flying V was introduced in 1958
The Ibanez RG was introduced in 1987
```

The dictionary and its keys are ordered in the sequence in which the key-value pairs are added. This is different from the previous behavior where the order was random and unspecified. If you are writing code that may be run on older versions of Python, you should not depend on the order being consistent.

I’ve found that *sorted* and *reversed* are useful functions for working with dictionaries. Here I use them both:

```
for model in reversed(sorted(guitars)): print(model)

Ibanez RG
Gibson Les Paul
Gibson Flying V
Fender Telecaster
Fender Stratocaster
```

# 3.9.8 Adding items to a dictionary

Use the dictionary square-bracket syntax on the left-hand side of an assignment to add key-value pairs.

```
numbers = {1: "one", 2: "two"}

numbers[3] = "three"

numbers

{1: 'one', 2: 'two', 3: 'three'}
```