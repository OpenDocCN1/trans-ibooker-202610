Unit to indicate that it doesn’t have a return value other than the state.

What would the get and set actions look like? They’re exceedingly simple. The get action simply passes the incoming state along and returns it as the value:

# Listing 6.20 The get combinator retrieves, then passes on its state

```
```kotlin
fun <S> get(): State<S, S> =
    State { s -> Tuple2(s, s) }
```
```

The set action is constructed with a new state s. The resulting action ignores the incoming state, replaces it with the new state, and returns Unit instead of a meaningful value:

# Listing 6.21 The set combinator updates the state, then returns Unit

```
```kotlin
fun <S> set(s: S): State<S, Unit> =
    State { Tuple2(s, Unit) }
```
```

The get, set and modify combinators can already be found on the arrow.mtl.StateApi class, but we just wanted to show what they entail for the purpose of demonstration. These two simple actions, together with the state combinators that we wrote—unit, map, map2, and flatMap—are all the tools we need to implement any kind of state machine or stateful program in a purely functional way.

**TIP** We recommend that you familiarize yourself with the Arrow documentation for State (arrow-kt.io/../-state.html), as well as to look at the underlying source code (github.com/../State.kt) in order fully grasp how it works. As mentioned before in section 1.5, the Arrow implementation is somewhat richer than the one that we’ve written, employing a monad transformer StateT, and exposing a public API StateAPI for the State class. Don’t worry too much about what a monad transformer is, just be aware that the StateApi is responsible for exposing methods such as get, set, modify stateSequential and stateTraverse which might come in handy in the exercise that follows.