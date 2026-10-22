a
t
i
o
n

|  | GCC 11.1 | Clang 11.1 |
|---|---|---|
| Classic Decorator | 1.0 | 1.0 |
| Compile Time Decorator | 0.078067 | 0.080313 |

As stated before, the performance of the compile time solution is significantly faster than the runtime solution: for both GCC and Clang it only takes approx. 8% of the time of the runtime solution and therefore is faster by more than one order of magnitude. I know, this sounds amazing. However, while the performance of the compile time solution is extraordinary, it comes with a couple of potentially severe limitations: due to the complete focus on templates, there is no runtime flexibility left. Since even the discount and tax rates are realized via template parameters, a new type needs to be created for each different rate. This may lead to longer compile times and more generated code (i.e. larger executables).
Additionally, it stands to reason that all class templates reside in header files, which again increases compile time and may reveal more implementation details than desired. More importantly, changes to the implementation details are widely visible and may cause massive recompilations. However, the most limiting factor appears to be that the solution can only be used in this form, if all information is available at compile time. Thus, you may only be able to get to this performance level for a few special cases.

# A Value-Based Runtime Decorator