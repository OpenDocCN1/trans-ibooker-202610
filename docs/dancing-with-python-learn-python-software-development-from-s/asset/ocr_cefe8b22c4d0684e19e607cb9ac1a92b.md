# 3.2.4 Removing list items

Let’s recall our list of electric guitar brand names and make a copy of it so we can use the original later:

```
brands = [
    "Fender",
    "Gibson",
    "Ibanez"
]
brands_2 = brands

print(f"{brands = }")
print(f"{brands_2 = }")
```

brands = ['Fender', 'Gibson', 'Ibanez']
brands_2 = ['Fender', 'Gibson', 'Ibanez']

I also assigned brands to brands_2, and we’ll see why in a moment.

Let’s remove the list entry "Ibanez". We can see by inspection that this is at index 2. Python uses del to remove a list item.

```
del brands[2]
print(f"{brands = }")
print(f"{brands_2 = }")
```

brands = ['Fender', 'Gibson']
brands_2 = ['Fender', 'Gibson']

brands changes and so does brands_2! When I did the assignment brands_2 = brands, I was not making two lists. I was giving two names to the same list. Therefore, when I changed the underlying list, we saw it whether we referred to it by brands or brands_2.

![](bbox=[0.25, 0.78, 0.78, 0.86])

Figure 3.3: Two variables referring to the same list object