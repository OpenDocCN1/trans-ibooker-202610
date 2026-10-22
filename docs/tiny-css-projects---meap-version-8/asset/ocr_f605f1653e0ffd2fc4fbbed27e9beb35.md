# Listing 1.12 Using multiple types of selectors to add a flag before Italian content

```
[lang="it"]::before {
  content: "🇮🇹"
}
```

Once applied, we can see in Figure 1.22 that our Italian content now has an emoji flag before it.

![](bbox=[0.18, 0.24, 0.9, 0.36])

Figure 1.22 Italian flag being applied using attribute and pseudo element selectors

# 1.6.5 The universal selector

The broadest type of selector is the universal selector which uses the asterisk symbol (*). This means that any declarations made using the universal selector will be applied to all of the HTML elements. Sometimes this can be used to reset CSS, but in terms of specificity the universal selector has a specificity value of 0, which means it can be overridden easily if needed.

In the case of our example project, we will use a universal selector (*) to set the font-family to sans-serif so it will be consistently sans-serif throughout the article: * { font-family: sans-serif; }.

Once applied we can see that all of the text in our document uses a sans-serif font regardless of their element type (Figure 1.23).