Let's dig into this refactored version. We filtered the list of assets using the `filter` method, then mapped the assets to their prices using the `mapToInt` function, and totaled them. We simply passed the `Predicate` we received on to the `filter` method and used a method reference for the `mapToInt`'s argument.

The filter method takes care of picking only the assets we're interested in. Under the hood, it calls the given `Predicate`'s `test` method to make that decision. If the selector accepts the asset, we use its value to total further down in the chain.

With this refactoring, we turned the three normal methods into one higher-order function that depends on a lightweight strategy to handle a configurable concern, as the following figure illustrates.

![](bbox=[0.148, 0.462, 0.696, 0.786])

Let's use this refactored version of `totalAssetValues` to total the values of all the assets.

```
designing/fpij/AssetUtilRefactored.java
```