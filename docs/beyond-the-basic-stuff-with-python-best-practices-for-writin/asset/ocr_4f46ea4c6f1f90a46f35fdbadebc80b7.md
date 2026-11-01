# 13

# MEASURING PERFORMANCE AND
BIG O ALGORITHM ANALYSIS

![](bbox=[0.15, 0.47, 0.32, 0.62])

For most small programs, performance isn’t all that important. We might spend an hour writing a script to automate a task that needs only a few seconds to run. Even if it takes longer, the program will probably finish by the time we’ve returned to our desk with a cup of coffee.

Sometimes it’s prudent to invest time in learning how to make a script faster. But we can’t know if our changes improve a program’s speed until we know how to measure program speed. This is where Python’s timeit and cProfile modules come in. These modules not only measure how fast code runs, but also create a profile of which parts of the code are already fast and which parts we could improve.

In addition to measuring program speed, in this chapter you’ll also learn how to measure the theoretical increases in runtime as the data for your program grows. In computer science, we call this big O notation. Software developers without traditional computer science backgrounds might sometimes feel they have gaps in their knowledge. But although a computer science education is fulfilling, it’s not always directly relevant to