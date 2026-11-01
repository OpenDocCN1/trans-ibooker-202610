```
rb: State<S, B>,
    f: (A, B) -> C
): State<S, C> =
    ra.flatMap { a ->
        rb.map { b ->
            f(a, b)
        }
    }

    fun <S, A> sequence(fs: List<State<S, A>>): State<S, List<A>> =
        foldRight(fs, unit(List.empty<A>())),
            { f, acc ->
                map2(f, acc) { h, t -> Cons(h, t) }
            }
        )
}

    fun <B> map(f: (A) -> B): State<S, B> =
        flatMap { a -> unit<S, B>(f(a)) }

    fun <B> flatMap(f: (A) -> State<S, B>): State<S, B> =
        State { s: S ->
            val (a: A, s2: S) = this.run(s)
            f(a).run(s2)
        }
}
```

# EXERCISE 6.11 (HARD/OPTIONAL)

To gain experience with the use of State, implement a finite state automaton that models a simple candy dispenser. The machine has two types of input: you can insert a coin, or you can turn the knob to dispense candy. It can be in one of two states: locked or unlocked. It also tracks how many candies are left and how many coins it contains.

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

+   - Inserting a coin into a locked machine will cause it to unlock if there’s any candy left.
+   - Turning the knob on an unlocked machine will cause it to dispense candy and become locked.
+   - Turning the knob on a locked machine or inserting a coin into an unlocked machine does nothing.
+   - A machine that’s out of candy ignores all inputs.

The method simulateMachine should operate the machine based on the list of inputs and return the number of coins and candies left in the machine at the end. For example, if the input Machine has 10 coins and 5 candies, and a total of 4 candies are successfully bought, the output should be (14, 1).