```
default:
    Writeline("Default");
    break;
} // end of switch statement

Writeline("After end of switch");
A_label:
Writeline($"After A_label");
```

![](bbox=[0.22, 0.23, 0.3, 0.29])

**Good Practice:** You can use the goto keyword to jump to another case or a label. The goto keyword is frowned upon by most programmers but can be a good solution to code logic in some scenarios. However, you should use it sparingly.

+   2. Run the code multiple times to see what happens in various cases of random numbers, as shown in the following example output:

```
// first random run
My random number is 4
Three or four
One
After end of switch
After A_label

// second random run
My random number is 2
Two
One
After end of switch
After A_label

// third random run
My random number is 6
Default
After end of switch
After A_label

// fourth random run
My random number is 1
One
After end of switch
After A_label

// fifth random run
My random number is 5
After A_label
```