# Discovering additional JIT compiler settings

Table 10.2 provides a summary of all other opcache.jit* settings in the php.ini file that have not already been covered:

| opcache.* Setting | Type | Description |
|---|---|---|
| jit_bisect_limit | int | Much like a break point, this tells JIT to stop after this limit has been reached. Use this to isolate the process causing the break. |
| jit_prof_threshold | float | If your "T" value is set to 2, this setting sets the level at which a function is considered "hot". (# calls to this func) divided by the (# calls to all funcs) must be > the value set here. For example, 0.1 means that functions that comprise more than 10% of all calls will be compiled |
| jit_max_root_traces | int | Maximum number of root traces. |
| jit_max_side_traces | int | Maximum number of side traces. |
| jit_max_exit_counters | int | Maximum number of side trace exit counters. Puts a limit on the total number of side traces. |
| jit_hot_loop | int | A loop is considered "hot" after this many iterations. |
| jit_hot_func | int | A function is considered "hot" after this many iterations. |
| jit_hot_return | int | A return is considered "hot" after this many iterations. |
| jit_hot_side_exit | int | An exit is considered "hot" after this many iterations. |
| jit_blacklist_root_trace | int | Maximum times a root trace compilation can fail before being blacklisted. |
| jit_blacklist_side_trace | int | Maximum times a side trace compilation can fail before being blacklisted. |
| jit_max_loop_unrolls | int | Maximum times a an attempt to close the loop on a side trace compilation can fail before being blacklisted. |
| jit_max_recursive_calls | int | Maximum number of times recursive call loops closings can fail. |
| jit_max_recursive_returns | int | Maximum number of times recursive return loops closings can fail. |
| jit_max_polymorphic_calls | int | Maximum number of times polymorphic (dynamic or method) calls can be inlined. Any call above this limit is classed as megamorphic and is not inlined. |

Table 10.2 – Additional opcache.jit* php.ini file settings

As you can see from the table, you have a high degree of control over how the JIT compiler operates. Collectively, these settings represent thresholds that control decisions the JIT compiler makes. These settings, if properly configured, allow the JIT compiler to ignore infrequently used loops and function calls. We'll now leave the exciting world of the JIT compiler and have a look at how to improve array performance.

# Speeding up array handling

Arrays are a vital part of any PHP program. Indeed, dealing with arrays is unavoidable as much of the real-world data your program handles day to day arrives in the form of an array. One example is data from an HTML form posting. The data ends up in either $_GET or $_POST as an array.