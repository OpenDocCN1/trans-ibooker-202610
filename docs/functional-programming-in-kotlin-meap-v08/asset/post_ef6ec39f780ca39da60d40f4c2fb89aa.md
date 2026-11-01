
## Listing 6.16 State declarations and combinators required to perform state propagation

```
val int: State<RNG, Int> = TODO()

fun ints(x: Int): State<RNG, List<Int>> = TODO()

fun <A, B> flatMap(
    s: State<RNG, A>,
    f: (A) -> State<RNG, B>
): State<RNG, B> = TODO()

fun <A, B> map(
    s: State<RNG, A>,
    f: (A) -> B
): State<RNG, B> = TODO()
```

- 1. is a `State<RNG, Int>` with the ability of generating a single random integer
- 2. returns a `State<RNG, List<Int>>` that can generate a list of x random integers
- 3. a `flatMap` function that operates on a `State<RNG, A>` with a function from A to `State<RNG, B>`
- 4. a `map` function that operates on a `State<RNG, A>` with a function that transforms A to B

Now we write some code utilizing these declarations that we subsequently have available to us from Listing 6.16.

## Listing 6.17 State propagation using a series of flatMap and map.

```
val ns: State<RNG, List<Int>> =
    flatMap(int) { x ->
        flatMap(int) { y ->
            map(ints(x)) { xs ->
                xs.map { it % y }
            }
        }
    }
```

- 1. int will generate a single random integer.
- 2. ints(x) generates a list of length x random integers
- 3. replaces every element in the list with its remainder when divided by y

It’s not clear what’s going on here due to all the nested flatMap and map calls. Let’s look for a simpler approach by turning our attention to the for-comprehension. This construct will unravel a series of flatMap calls, allowing us to rewrite this code in what seems to be a series of imperative declarations. The trick with this code lies in the destructuring that occurs in every step. Each time we see something being destructured, it implies a call to flatMap.

For example, a line that is written as flatMap(int) { x -> ... } could be rewritten as val (x: Int) = int within the confines of a for-comprehension. With this in mind, let’s try rewriting Listing 6.17 using this technique.

## Listing 6.18 State propagation using a for-comprehension.

```
val ns2: State<RNG, List<Int>> =
    State.fx(Id.monad()) {
        val (x: Int) = int
        val (y: Int) = int
        val (xs: List<Int>) = ints(x)
        xs.map { it % y }
    }
```

- - ① open the for-comprehension by passing a code block into `State.fx(Id.monad())`
- - ② `int` is destructured to an `Int` named x
- - ③ `int` is again destructured to an `Int` named y
- - ④ `ints(x)` is destructured to a `List<Int>` of length x
- - ⑤ replace every element in xs with its remainder when divided by y, return the result

We begin by opening the for-comprehension using a call to `State.fx` and passing in an instance of `Id.monad()`. We won’t concern ourselves very much with the detail regarding this, but suffice to say that this will allow us to pass in an anonymous function that acts like a block of imperative code and then return the final outcome of this block as a value.

The code block making up the for-comprehension is much easier to read (and write), and it looks like what it is—an imperative program that maintains some state. But it’s the same code as in the previous example. We get the next `Int` and assign it to x, get the next `Int` after that and assign it to y, then generate a list of length x, and finally return the list with all of its elements modulo y.

We almost have everything we need in order to write fully fledged functional programs in an imperative style. To facilitate this kind of imperative programming with for-comprehensions (or `flatMaps`), we really only need two additional primitive `State` combinators—one for reading the state and one for writing the state. If we imagine that we have a combinator `get` for getting the current state, and a combinator `set` for setting a new state, we could implement a combinator that can modify the state in arbitrary ways:

## Listing 6.19 Combinator to modify the current State

```
fun <S> modify(f: (S) -> S): State<S, Unit> =
    State.fx(Id.monad()) {
        val (s: S) = get<S>()
        val (_) = set(f(s))
    }
```

- - ① Set up the for-comprehension for `State`
- - ② Get the current state and assign it to s
- - ③ Set the new state of f applied to s

This method returns a `State` action that modifies the incoming state by the function f. It yields
Unit to indicate that it doesn’t have a return value other than the state.

What would the get and set actions look like? They’re exceedingly simple. The get action simply passes the incoming state along and returns it as the value:

## Listing 6.20 The get combinator retrieves, then passes on its state

```kotlin
fun <S> get(): State<S, S> =
    State { s -> Tuple2(s, s) }
```

The set action is constructed with a new state s. The resulting action ignores the incoming state, replaces it with the new state, and returns Unit instead of a meaningful value:

## Listing 6.21 The set combinator updates the state, then returns Unit

```kotlin
fun <S> set(s: S): State<S, Unit> =
    State { Tuple2(s, Unit) }
```

The get, set and modify combinators can already be found on the arrow.mtl.StateApi class, but we just wanted to show what they entail for the purpose of demonstration. These two simple actions, together with the state combinators that we wrote—unit, map, map2, and flatMap—are all the tools we need to implement any kind of state machine or stateful program in a purely functional way.

**TIP** We recommend that you familiarize yourself with the Arrow documentation for State (arrow-kt.io/../-state.html), as well as to look at the underlying source code (github.com/../State.kt) in order fully grasp how it works. As mentioned before in section 1.5, the Arrow implementation is somewhat richer than the one that we’ve written, employing a monad transformer StateT, and exposing a public API StateAPI for the State class. Don’t worry too much about what a monad transformer is, just be aware that the StateApi is responsible for exposing methods such as get, set, modify stateSequential and stateTraverse which might come in handy in the exercise that follows.

## EXERCISE 6.11 (Hard/Optional)

To gain experience with the use of `State`, implement a finite state automaton that models a simple candy dispenser. The machine has two types of input: you can insert a coin, or you can turn the knob to dispense candy. It can be in one of two states: locked or unlocked. It also tracks how many candies are left and how many coins it contains.

```
sealed class Input

object Coin : Input()
object Turn : Input()

data class Machine(
    val locked: Boolean,
    val candies: Int,
    val coins: Int
)
```

The rules of the machine are as follows:

- - Inserting a coin into a locked machine will cause it to unlock if there's any candy left.
- - Turning the knob on an unlocked machine will cause it to dispense candy and become locked.
- - Turning the knob on a locked machine or inserting a coin into an unlocked machine does nothing.
- - A machine that's out of candy ignores all inputs.

The method `simulateMachine` should operate the machine based on the list of inputs and return the number of coins and candies left in the machine at the end. For example, if the input `Machine` has 10 coins and 5 candies, and a total of 4 candies are successfully bought, the output should be `(14, 1)`. Use the following declaration stubs to implement your solution:

```
fun simulateMachine(
    inputs: List<Input>
): State<Machine, Tuple2<Int, Int>> = TODO()
```

## 6.7 Conclusion

In this chapter, we touched on the subject of how to write purely functional programs that have state. We used random number generation as the motivating example, but the overall pattern comes up in many different domains. The idea is simple: use a pure function that accepts a state as its argument, and it returns the new state alongside its result. Next time you encounter an imperative API that relies on side effects, see if you can provide a purely functional version of it, and use some of the functions we wrote here to make working with it more convenient.
