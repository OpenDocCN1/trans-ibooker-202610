differently when NULLs are present. Using PySpark, we can choose several statements to transform NULL values. The most common ones are as follows:

```
.Na.drop()
```

This function removes entire rows if one or more columns contain NULL values. While this can help clean up incomplete records, it also comes with risks because dropping rows may lead to unintended data loss. So, before you use .na.drop(), you need to determine whether the affected rows contain valuable data that should be retained and whether the missing value truly makes the entire row unusable.

```
.fillna()
```

With .fillna(), you replace NULL with a specific default, so you should only use .fillna() when you’re confident that the default value reflects the intended meaning. For example, using 0 might be misleading if the actual reason for the NULL was a processing delay or a missing entry from another system.

```
.replace()
```

While .fillna() specifically targets NULL (which indicates missing values), .replace() is a more general function that’s used to replace any specified value with another. This is useful for standardizing or cleaning data beyond just handling NULLs (for example, replacing placeholder strings like "N/A" or correcting inconsistent spellings). It can replace values regardless of whether they are NULL or not.

Figure 3-29 shows us the effect each of these commands has on our data.