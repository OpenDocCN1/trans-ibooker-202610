# CHAPTER 2　 CRYPTOGRAPHY FUNDAMENTALS

![](bbox=[0.201, 0.109, 0.764, 0.234])

**Figure 2-9.** *The execution of a Caesar cipher*

The application is very simple and easy to interact with it.

# **Listing 2-2.** Source Code for a Caesar Cipher Implementation

```
```cpp
#include <iostream>
using namespace std;

// This function receives text and shift and
// returns the encrypted text
string encrypt(string text, int s)
{
    string result = "";

    // traverse text
    for (int i=0;i<text.length();i++)
    {
        // apply transformation to each character
        // Encrypt Uppercase letters
        if (isupper(text[i]))
            result += char(int(text[i]+s-65)%26 +65);

        // Encrypt Lowercase letters
        else
            result += char(int(text[i]+s-97)%26 +97);
    }

    // Return the resulting string
    return result;
}
```
```