**Solution**: This problem can be solved via recursion or via dynamic programming. Both techniques were covered in *Chapter 8, Recursion and Dynamic Programming*. So, let's look at the recursive algorithm for a sequence of *n* digits:

+   1. Initialize the total number of decodings with 0.
+   2. Start from the end of the given sequence of digits.
+   3. If the last digit is not 0, then apply recursion for the (*n*-1) digits and use the result to update the total number of decodings.
+   4. If the last two digits represent a number less than 27 (therefore, a valid character), then apply recursion to the (*n*-2) digits and use the result to update the total number of decodings.

In terms of code, we have the following:

```
public static int decoding(char[] digits, int n) {

    // base cases
    if (n == 0 || n == 1) {
        return 1;
    }

    // if the digits[] starts with 0 (for example, '0212')
    if (digits == null || digits[0] == '0') {
        return 0;
    }

    int count = 0;

    // If the last digit is not 0 then last
    // digit must add to the number of words
    if (digits[n - 1] > '0') {
        count = decoding(digits, n - 1);
    }

    // If the last two digits represents a number smaller
    // than or equal to 26 then consider last two digits
    // and call decoding()
```