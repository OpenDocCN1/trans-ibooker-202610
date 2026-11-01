```
val update: (Input) -> (Machine) -> Machine =
  { i: Input ->
    { s: Machine ->
      when (i) {
        is Coin ->
          if (!s.locked || s.candies == 0) s
          else Machine(false, s.candies, s.coins + 1)
        is Turn ->
          if (s.locked || s.candies == 0) s
          else Machine(true, s.candies - 1, s.coins)
      }
    }
  }

fun simulateMachine(
  inputs: List<Input>
): State<Machine, Tuple2<Int, Int>> =
  StateFX(Id.monad()) {
    val (x) = inputs.map(update).map(StateApi::modify)
      .stateSequential()
    val (s: Machine) = StateApi.get<Machine>()
    Tuple2(s.candies, s.coins)
  }
```

# CHAPTER 7. PURELY FUNCTIONAL PARALLELISM

# EXERCISE 7.1

Par.map2 is a new higher-order function for combining the result of two parallel computations. What is its signature? Give the most general signature possible (don’t assume it works only for Int).

```
fun <A, B, C> map2(
  sum: Par<A>,
  sum1: Par<B>,
  function: (A, B) -> C
): Par<C> = Par(function(sum.get, sum1.get))
```

# EXERCISE 7.2

At any point while evolving an API, you can start thinking about possible representations for the abstract types that appear. Try to come up with a representation for Par that makes it possible to implement the functions of our API.

```
class Par<A>(val get: A) {
  companion object {
    fun <A> unit(a: A): Par<A> = Par(a)
    fun <A, B, C> map2(
      a1: Par<A>,
      a2: Par<B>,
      f: (A, B) -> C
    ): Par<C> = Par(f(a1.get, a2.get))
    fun <A> fork(f: () -> Par<A>): Par<A> = f()
    fun <A> lazyUnit(a: () -> A): Par<A> = Par(a())
    fun <A> run(a: Par<A>): A = a.get
  }
}
```