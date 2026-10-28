+   3. Continue by dividing the number using digits at ten's place and print the corresponding Roman number.
+   4. Continue by dividing the number using digits at one's place and print the corresponding Roman number.

In terms of code, this algorithm works as follows:

```
private static final String HUNDREDTHS[]
= {"", "C", "CC", "CCC", "CD", "D",
    "DC", "DCC", "DCCC", "CM"};
private static final String TENS[]
= {"", "X", "XX", "XXX",
    "XL", "L", "LX", "LXX", "LXXX", "XC"};
private static final String ONES[]
= {"", "I", "II", "III", "IV", "V",
    "VI", "VII", "VIII", "IX"};

public static String convert(int n) {
    String roman = "";
    // Step 1
    while (n >= 1000) {
        roman = roman + 'M';
        n -= 1000;
    }
    // Step 2
    roman = roman + HUNDREDTHS[n / 100];
    n = n % 100;
    // Step 3
    roman = roman + TENS[n / 10];
    n = n % 10;
    // Step 4
    roman = roman + ONES[n];
```