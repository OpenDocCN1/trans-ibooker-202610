First, you can loop across the set members.

```
for brand in my_brands: print(brand)
```

Gretsch
Gibson
Fender
Ovation

Second, you can pop, remove, and return a random member.

```
my_brands
my_brands.pop()
```

'Gretsch'

```
my_brands
```

{'Fender', 'Gibson', 'Ovation'}

# Exercise 3.34

What kind of exception is raised if you try to pop from an empty set?

# 3.10.6 Subsets and supersets

Let’s recover our original guitar collections.

```
my_brands = my_original_brands.copy()
my_brands
```

{'Fender', 'Gibson', 'Ovation'}

```
their_brands = their_original_brands.copy()
their_brands
```

{'Fender', 'Gibson', 'Gretsch'}