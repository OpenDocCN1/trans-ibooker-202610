```
}

// return the result only if it is
// shorter than the given string
return result.length() > str.length()
    ? str : result.toString();
}
```

The complete application is called StringShrinker.

# Coding challenge 6 – Extracting integers

**Problem:** Consider a given string containing whitespaces and a-z and 0-9 characters. Write a snippet of code that extracts integers from this string. You can assume that any sequence of consecutive digits forms a valid integer.

**Solution:** Consider that the given string is cv dd 4 k 2321 2 11 k4k2 66 4d. The expected result will contain the following integers: 4, 2321, 2, 11, 4, 2, 66, and 4.

A straightforward solution will loop the given string character by character and concatenate sequences of consecutive digits. A digit contains ASCII code between 48 (inclusive) and 97 (inclusive). So, any character whose ASCII code is in the range [48, 97] is a digit. We can also use the Character#isDigit(char ch) method. When a sequence of consecutive digits is interrupted by a non-digit character, we can convert the harvested sequence into an integer and append it as a list of integers. Let's see this in terms of code:

```
public static List<Integer> extract(String str) {

    List<Integer> result = new ArrayList<>();
    StringBuilder temp = new StringBuilder(
        String.valueOf(Integer.MAX_VALUE).length());

    for (int i = 0; i < str.length(); i++) {

        char ch = str.charAt(i);

        // or, if (((int) ch) >= 48 && ((int) ch) <= 57)
        if (Character.isDigit(ch)) {
            temp.append(ch);
```