addresses and not a multilevel list—that, of course, depends on what the business wants.

Thus, if we have a _one-to-many_ function and use the `map` method, we'll end up with a nested collection. If we want a collection instead, we have to take an extra step after mapping—we'll have to flatten the generated `List<List<String>>` to a one-level `List<String>`.

Unlike languages like Ruby and Kotlin, Java doesn't have a _flatten_ function, but, for a moment, let's assume it does so we could write code like this:

```
SAMPLE_DATA.stream().map(Person::emailAddresses).flatten() //hypothetical
```

We'd first map and then flatten to get a one-level list. That's good, but it would still involve making two separate calls. It would be great to merge the two steps into one step. Looking closely, it's quite logical that the single combined step should be called `mapFlatten` and could be used like so:

```
SAMPLE_DATA.stream().mapFlatten(Person::emailAddresses) //hypothetical
```

But there is no `mapFlatten`, fortunately. That's because, about the third time you say the method's name, you'll feel a sharp pain in the muscles on your cheek around the jaw joints. This is due to the awkward movements your lower jaw (mandible) had to endure to say "map flatten." Having a method named _mapFlatten_ would have led to repetitive stress injury and turned into an occupational hazard—oh, dear. To avoid that, the method could be named `flatMap` instead of `mapFlatten`.

The method name `flatMap` is easier to say, it rolls off the tongue, but remember it does the operation in reverse—it