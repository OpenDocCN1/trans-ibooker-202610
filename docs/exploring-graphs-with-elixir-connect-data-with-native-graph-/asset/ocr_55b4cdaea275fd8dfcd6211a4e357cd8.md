There's one caveat: inferred parameters are non-final. In the earlier example, where we explicitly specified the type, we also marked the parameter as final. This prevents us from modifying the parameter within the lambda expression. In general, modifying parameters is in poor taste and leads to errors, so marking them final is a good practice. Unfortunately, when we favor type inference we have to practice extra discipline not to modify the parameter, as the compiler won't protect us.

We've come a long way with this example and reduced the code quite a bit. But there's more. Let's take one last step to tease out another ounce of conciseness.

collections/fpij/Iteration.java

```
friends.forEach(System.out::println);
```

In the preceding code, we used a method reference. Java lets us simply replace the body of code with the method name of our choice. We'll dig into this further in the next section, but for now, let's reflect on the wise words of Antoine de Saint-Exupéry: “Perfection is achieved not when there is nothing more to add, but when there is nothing left to take away.”

Lambda expressions helped us concisely iterate over a collection. Next, we'll cover how they help remove mutability and make the code even more concise when transforming collections.