# std::floating_point

Let’s consider the following class declaration:

```
///** the class is a general class for working with cryptographic
///** operations related to floating numbers.
class FloatingClassExample {};
```

If we invoke

```
std::cout << std::floating_point<FloatingClassExample>::value;
```

the output will be false. But if we have

```
std::cout << std::floating_point<float>::value;
```

the output will be true.

# std::destructible

In cryptography, once we reach the end of an operation (e.g. encryption or decryption) it is recommended that all functions, structures, classes, objects, and types be destroyed.

To achieve the destruction process using std::destructible, we just need to pass the type to destructible<> as follows:

```
class AESClass {};
std::cout << std::destructible<AESClass>::value;
```

The output of this will be true.

# std::integral, std::signed_integral, std::unsigned_integral

The concepts are used in the same way as the examples listed above. Let’s consider the following type tests:

```
std::cout << integral<AESClass>::value;
std::cout << integral<int>::value;
std::cout << integral<float>::value;
```

The output will be false, true, and false. For signed_integral and unsigned_integral, it’s the same.