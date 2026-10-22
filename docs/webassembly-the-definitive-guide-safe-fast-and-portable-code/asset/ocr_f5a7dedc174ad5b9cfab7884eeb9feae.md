o
n
s

| Id | Name | Description |
|---|---|---|
| 0 | Custom | Debugging or metadata information for third-party uses |
| 1 | Type | Type definitions used in the modules |
| 2 | Import | Imported elements used by a module |
| 3 | Function | Type signatures associated with the functions in a module |
| 4 | Table | Tables that define indirect, immutable references used by a module |
| 5 | Memory | Linear memory structures used by a module |
| 6 | Global | Global variables |
| 7 | Export | Exported elements provided by a module |
| 8 | Start | An optional start function to initiate a module |
| 9 | Element | Elements defined by a module |
| 10 | Code | The body of the functions defined by a module |
| 11 | Data | The data elements defined by a module |
| 12 | Data Count | The number of data elements defined by the module |

Here is our example from Chapter 2 again for reference.

# *Example 3-2. A simple WebAssembly text file.*

```
(module
    (func $how_old (param $year_now i32) (param $year_born i32)
(result i32) ①
```