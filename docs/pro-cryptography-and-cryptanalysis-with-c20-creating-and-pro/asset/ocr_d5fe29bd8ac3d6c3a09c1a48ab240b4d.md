We will continue with a functional that will shift to the left a specific number of spaces, as shown in Listing 4-6.

# Listing 4-6. Shifting to the Left a Specific Number of Spaces⁶

```
void leftShifting (int BigInt1[], int number) {
    int i;

    //** moving starting from left to right,
    //** we will move anything with left n spaces
    for (i=N-1; i>= number; i--)
        BigInt1[i] = BigInt1[i- number];

    //** complete the last n digits with zeros
    while (i >= 0) BigInt1[i--] = 0;
}
```

# Big Integer Libraries

There are several libraries and frameworks that deal with high numbers. For some, the development process was suspended, but they are still used in cryptography applications.

The libraries for working with big integers are as follows:

+   - **Matt McCutchen⁷** proposed a very easy-to-use C++ library for calculations on big integers [1]. The code has very good explanations and it is easy to follow. The results obtained in symmetric and asymmetric cryptography algorithms were promising. Most of the results were compared with other tools for reference and checking, such as CryptTool.⁸

⁶The code is meant to be a sketch of a function that will transform a simple integer to a big integer. The source code will not compile without a proper adjustment for a real cryptographic application.

⁷Matt McCutchen’s web site, https://mattmccutchen.net/

⁸CrypTool, www.cryptool.org/en/