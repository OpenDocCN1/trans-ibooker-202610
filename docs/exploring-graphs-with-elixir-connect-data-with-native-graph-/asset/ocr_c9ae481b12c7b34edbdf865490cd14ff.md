![](bbox=[0.14, 0.1, 0.88, 0.48])

When working with lambda expressions, we should ensure that the code is without side effects. Doing so not only reduces the chance of errors but also helps us easily parallelize the code, as we saw in Taking a Leap to Parallelize. It's critical to eliminate side effects if we want to use techniques like the tail-call optimization we saw in Using Tail-Call Optimization.

# Prefer Expressions over Statements

Both expressions and statements are commands we write in programs to instruct the computer to perform some action or do some work. Statements perform actions but don't return anything, whereas expressions perform actions and return a result. When programming with lambda expressions, we can reap benefits by leaning toward creating expressions more than statements.