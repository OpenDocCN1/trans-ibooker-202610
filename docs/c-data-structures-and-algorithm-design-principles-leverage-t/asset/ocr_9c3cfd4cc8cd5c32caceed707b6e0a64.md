```
ABC_B__E_B_ ABC B__E_B
ABC_B__E_A ABC B__EA_
ABC_B___FB_ ABC B_FB___
ABC_B___BA ABC B__B_A_
ABC_B__E_B_ ABC___BE_B
ABC_B__E_A ABC___BEA_
ABC___BE_B_ ABC___BE_B
ABC___BE_A ABC___BEA_
ABC____EFB_ ABC_EFB___
ABC____FBA ABC__FB_A_
SIZE = 7
    ABC_B_BE_B_ ABC B__BE_B
    ABC_B_BE__A ABC B__BEA_
    ABC_B__EFB_ ABC BEFB___
    ABC_B___FBA ABC B_FB_A_
    ABC____EFBA ABC_EFB_A_
SIZE = 8
    ABC_B__EFBA ABC BEFB_A_
```

Length of the longest common subsequence of ABCABDBEFBA and ABCBEFBEAB is:
8
TIME TAKEN USING BRUTE FORCE: 0.00242 SECONDS

Length of the longest common subsequence of ABCABDBEFBA and ABCBEFBEAB is:
8
TIME TAKEN USING MEMOIZATION: 0.00003 SECONDS

6. Of course, the time taken by the brute-force approach is going to be affected by the additional step of printing out the subsequences. By running our code again after setting the DEBUG constant to 0, the output is now as follows:

```
Length of the longest common subsequence of ABCABDBEFBA and ABCBEFBEAB is:
8
TIME TAKEN USING BRUTE FORCE: 0.00055 SECONDS

Length of the longest common subsequence of ABCABDBEFBA and ABCBEFBEAB is:
8
TIME TAKEN USING MEMOIZATION: 0.00002 SECONDS
```