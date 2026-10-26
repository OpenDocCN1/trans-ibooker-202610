```
```cpp
int M = 0;
int Lim_value = 100;
bool flag = true;
for (int i = 0; i < Len; ++i) {
    S += a[i];

    if (i == 0 || M > a[i]) {
        M = a[i];
    }

    if (a[i] >= Lim_value) {
        flag = true;
    }

    if (a[i] < 0) {
        a[i] *= 2;
    }
}
}
```
```

Now that the code is a bit easier to follow, let's try to understand what it does. Thanks to the comments, we understand that we have an input array, **a**, whose length is **Len**. Better names for these would be **input** and **inputLength**.

+   3. Let's make that first change and rename **a** to **input**. If you are using Eclipse, you can select **Refactor | Rename** to rename one occurrence and all others will be renamed as well. Do the same for **Len** and rename it to **inputLength**.

+   4. The updated code will look like the following. Note that we do not need the comment anymore since parameter names are self-explanatory:

```
```cpp
void arrayPlay(int *input, int inputLength) {

    int S = 0;
    int M = 0;
    int Lim_value = 100;
    bool flag = true;
    for (int i = 0; i < inputLength; ++i) {
        S += input[i];

        if (i == 0 || M > input[i]) {
            M = input[i];
        }
```
```