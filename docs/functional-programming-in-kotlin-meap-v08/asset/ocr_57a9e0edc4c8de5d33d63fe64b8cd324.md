# 1.1.2 A functional solution: removing the side effects

The functional solution is to eliminate side effects and have `buyCoffee` return the charge as a value in addition to returning the `Coffee`. The concerns of processing the charge by sending it off to the credit card provider, persisting a record of it, and so on, will be handled elsewhere.

![](bbox=[0.13, 0.18, 0.88, 0.4])

Figure 1.1 A call to buyCoffee, with and without side effect

Here’s what a functional solution in Kotlin might look like:

# Listing 1.3 A more functional approach to buying coffee

```
```kotlin
class Cafe {
    fun buyCoffee(cc: CreditCard): Pair<Coffee, Charge> {
        val cup = Coffee()
        return Pair(cup, Charge(cc, cup.price))
    }
}
```
```

Here we’ve separated the concern of *creating* a charge from the *processing* or *interpretation* of that charge. The `buyCoffee` function now returns a `Charge` as a value along with the `Coffee`. We’ll see shortly how this lets us reuse it more easily to purchase multiple coffees with a single transaction. But what is `Charge`? It’s a data type we just invented containing a `CreditCard` and an `amount`, equipped with a handy function, `combine`, for combining charges with the same `CreditCard`:

# Listing 1.4 Charge as a data type

```
```kotlin
data class Charge(val cc: CreditCard, val amount: Float) {
    fun combine(other: Charge): Charge =
        if (cc == other.cc)
            Charge(cc, amount + other.amount)
        else throw Exception(
            "Cannot combine charges to different cards"
        )
}
```
```