# Activity 7: Email Address Validator

In this activity, we will create a validator to check if an email address requested by a user is already taken. Complete the activity using these steps:

+   1. Let's include the required headers:

```
```cpp
#include <iostream>
#include <vector>

#include <openssl/md5.h>
```
```

+   2. Let's add a class for the Bloom filter:

```
```cpp
class BloomFilter
{
    int nHashes;
    std::vector<bool> bits;

    static constexpr int hashSize = 128/8;

    unsigned char hashValue[hashSize];
}
```
```

+   3. Let's add a constructor for this:

```
```cpp
BloomFilter(int size, int hashes) : bits(size), nHashes(hashes)
{
    if(nHashes > hashSize)
    {
        throw ("Number of hash functions too high");
    }
    if(size > 255)
    {
        throw ("Size of bloom filter can't be >255");
    }
}
```
```

Since we're going to use each byte in the hash value buffer as a different hash function value, and the size of the hash value buffer is 16 bytes (128 bits), we can't have more hash functions than that. Since each hash value is just 1 byte, its possible values are 0 to 255. So, the size of the Bloom filter can't exceed 255. Hence, we're throwing an error in the constructor itself.