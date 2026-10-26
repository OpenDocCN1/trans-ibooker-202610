4. Now, let's write a hash function. It simply uses the MD5 function to calculate the hash:

```
void hash(const std::string& key)
{
    MD5(reinterpret_cast<const unsigned char*>(key.data()), key.length(),
    hashValue);
}
```

5. Let's add the function so that we can insert an email:

```
void add(const std::string& key)
{
    hash(key);
    for(auto it = &hashValue[0]; it < &hashValue[nHashes]; it++)
    {
        bits[*it] = true;
    }
    std::cout << key << " added in bloom filter." << std::endl;
}
```

As we can see, we are iterating from the the bytes 0 to nHashes in the hash value buffer and setting each bit to 1.

6. Similarly, let's add a function to find an email address:

```
bool mayContain(const std::string &key)
{
    hash(key);
    for (auto it = &hashValue[0]; it < &hashValue[nHashes]; it++)
    {
        if (!bits[*it])
        {
            std::cout << key << " email can by used." << std::endl;
            return false;
        }
    }
    std::cout << key << " email is used by someone else." << std::endl;
    return true;
};
```