

>>> print((52-30-38)%prime)
41

## 练习 3

编写相应的 __sub__ 方法，用于定义两个 FieldElement 对象的减法。

```
class FieldElement:
    ...
    def __sub__(self, other):
        if self.prime != other.prime:
            raise TypeError('Cannot subtract two numbers in different Fields')
        # self.num and other.num are the actual values
        # self.prime is what we need to mod against
        num = (self.num - other.num) % self.prime
        # we return an element of the same class
        return self.__class__(num, self.prime)
```

## 练习 4

在 F₉₇ 中求解以下方程（同样，假设 · 和幂运算为域版本）：

- 95 · 45 · 31
- 17 · 13 · 19 · 44
- 12⁷ · 77⁴⁹

```
>>> prime = 97
>>> print(95*45*31 % prime)
23
>>> print(17*13*19*44 % prime)
68
>>> print(12**7*77**49 % prime)
63
```

## 练习 5

对于 k = 1, 3, 7, 13, 18，在 F₁₉ 中这个集合是什么？

{k · 0, k · 1, k · 2, k · 3, ... k · 18}

你注意到这些集合有什么特点吗？

```
>>> prime = 19
>>> for k in (1,3,7,13,18):
...     print([k*i % prime for i in range(prime)])
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18]
[0, 3, 6, 9, 12, 15, 18, 2, 5, 8, 11, 14, 17, 1, 4, 7, 10, 13, 16]
```

>>> y3 = s*(x1-x3)-y1
>>> print(x3,y3)
18.0 -77.0

## 练习 7

编写当 P₁ = P₂ 时的 __add__ 方法。

```
class Point:
    ...
    def __add__(self, other):
        ...
if self == other:
    s = (3 * self.x**2 + self.a) / (2 * self.y)
    x = s**2 - 2 * self.x
    y = s * (self.x - x) - self.y
    return self.__class__(x, y, self.a, self.b)
```

## 第 3 章：椭圆曲线密码学

## 练习 1

判断以下点是否在 F₂₂₃ 上的曲线 y² = x³ + 7 上：

(192,105), (17,56), (200,119), (1,193), (42,99)

```
>>> from ecc import FieldElement
>>> prime = 223
>>> a = FieldElement(0, prime)
>>> b = FieldElement(7, prime)
>>> def on_curve(x,y):
...     return y**2 == x**3 + a*x + b
>>> print(on_curve(FieldElement(192, prime), FieldElement(105, prime)))
True
>>> print(on_curve(FieldElement(17, prime), FieldElement(56, prime)))
True
>>> print(on_curve(FieldElement(200, prime), FieldElement(119, prime)))
False
>>> print(on_curve(FieldElement(1, prime), FieldElement(193, prime)))
True
>>> print(on_curve(FieldElement(42, prime), FieldElement(99, prime)))
False
```

## 练习 2

对于 F₂₂₃ 上的曲线 y² = x³ + 7，求：

- (170,142) + (60,139)
- (47,71) + (17,56)
- (143,98) + (76,66)

```
>>> from ecc import FieldElement, Point
>>> prime = 223
>>> a = FieldElement(0, prime)
>>> b = FieldElement(7, prime)
>>> p1 = Point(FieldElement(170, prime), FieldElement(142, prime), a, b)
>>> p2 = Point(FieldElement(60, prime), FieldElement(139, prime), a, b)
>>> print(p1+p2)
Point(220,181)_0_7 FieldElement(223)
>>> p1 = Point(FieldElement(47, prime), FieldElement(71, prime), a, b)
>>> p2 = Point(FieldElement(17, prime), FieldElement(56, prime), a, b)
>>> print(p1+p2)
Point(215,68)_0_7 FieldElement(223)
>>> p1 = Point(FieldElement(143, prime), FieldElement(98, prime), a, b)
>>> p2 = Point(FieldElement(76, prime), FieldElement(66, prime), a, b)
>>> print(p1+p2)
Point(47,71)_0_7 FieldElement(223)
```

## 练习 3

扩展 ECCTest，以测试上一练习中的加法运算。将此方法命名为 test_add。

```
def test_add(self):
    prime = 223
    a = FieldElement(0, prime)
    b = FieldElement(7, prime)
    additions = (
        (192, 105, 17, 56, 170, 142),
        (47, 71, 117, 141, 60, 139),
        (143, 98, 76, 66, 47, 71),
    )
    for x1_raw, y1_raw, x2_raw, y2_raw, x3_raw, y3_raw in additions:
        x1 = FieldElement(x1_raw, prime)
        y1 = FieldElement(y1_raw, prime)
        p1 = Point(x1, y1, a, b)
        x2 = FieldElement(x2_raw, prime)
        y2 = FieldElement(y2_raw, prime)
        p2 = Point(x2, y2, a, b)
        x3 = FieldElement(x3_raw, prime)
        y3 = FieldElement(y3_raw, prime)
        p3 = Point(x3, y3, a, b)
        self.assertEqual(p1 + p2, p3)
```

## 练习 4

对于 F₂₂₃ 上的曲线 y² = x³ + 7，求：

- 2 · (192,105)
- 2·(143,98)
- 2·(47,71)
- 4·(47,71)
- 8·(47,71)
- 21·(47,71)

```
>>> from ecc import FieldElement, Point
>>> prime = 223
>>> a = FieldElement(0, prime)
>>> b = FieldElement(7, prime)
>>> x1 = FieldElement(num=192, prime=prime)
>>> y1 = FieldElement(num=105, prime=prime)
>>> p = Point(x1,y1,a,b)
>>> print(p+p)
Point(49,71)_0_7 FieldElement(223)
>>> x1 = FieldElement(num=143, prime=prime)
>>> y1 = FieldElement(num=98, prime=prime)
>>> p = Point(x1,y1,a,b)
>>> print(p+p)
Point(64,168)_0_7 FieldElement(223)
>>> x1 = FieldElement(num=47, prime=prime)
>>> y1 = FieldElement(num=71, prime=prime)
>>> p = Point(x1,y1,a,b)
>>> print(p+p)
Point(36,111)_0_7 FieldElement(223)
>>> print(p+p+p+p)
Point(194,51)_0_7 FieldElement(223)
>>> print(p+p+p+p+p+p+p+p)
Point(116,55)_0_7 FieldElement(223)
>>> print(p+p+p+p+p+p+p+p+p+p+p+p+p+p+p+p)
Point(infinity)
```

## 练习 5

对于 F₂₂₃ 上的曲线 y² = x³ + 7，求由 (15,86) 生成的群的阶。

```
>>> prime = 223
>>> a = FieldElement(0, prime)
>>> b = FieldElement(7, prime)
>>> x = FieldElement(15, prime)
>>> y = FieldElement(86, prime)
>>> p = Point(x, y, a, b)
>>> inf = Point(None, None, a, b)
>>> product = p
>>> count = 1
>>> while product != inf:
...     product += p
...     count += 1
>>> print(count)
7
```

```
>>> from ecc import PrivateKey
>>> priv = PrivateKey(5002)
>>> print(priv.point.address(compressed=False, testnet=True))
mmTPbQXFboEtNRkwhf6K51jvdtHLxGeMA
>>> priv = PrivateKey(2020**5)
>>> print(priv.point.address(compressed=True, testnet=True))
mopVkx p8UhxQRYbCYJsbeE1hifiF64jcoH
>>> priv = PrivateKey(0x1234deadbeef)
>>> print(priv.point.address(compressed=True, testnet=False))
1F1Pn2y6pDb68E5nYJJeba4TLg2U7B6KF1
```

## 练习 6

求以下私密值对应的私钥的 WIF：

- 5003 (压缩, 测试网)
- 2021⁵ (未压缩, 测试网)
- 0x54321deadbeef (压缩, 主网)

```
>>> from ecc import PrivateKey
>>> priv = PrivateKey(5003)
>>> print(priv.wif(compressed=True, testnet=True))
cMahea7zqjxrtgAbB7LSGbcQUr1uX1oJuat9jZodMN8rFTv2s fUK
>>> priv = PrivateKey(2021**5)
>>> print(priv.wif(compressed=False, testnet=True))
91avARGdfge8E4tZFYLoxeJ5sGbDNJQH4kvjpWAXgzcZjbCwxic
>>> priv = PrivateKey(0x54321deadbeef)
>>> print(priv.wif(compressed=True, testnet=False))
KwDiBf890gGbJEhKnhXJuH7LrciVrZi3qYjgiuQJv1h8Ytr2S53a
```

## 练习 7

编写一个函数 little_endian_to_int，它接受 Python 字节，将这些字节按小端序解释，并返回该数字。

```
def little_endian_to_int(b):
    """little_endian_to_int takes byte sequence as a little-endian number.
    Returns an integer"""
    return int.from_bytes(b, 'little')
```

## 练习 8

编写一个函数 int_to_little_endian，该函数是上一练习的逆操作。

```
def int_to_little_endian(n, length):
    """endian_to_little_endian takes an integer and returns the little-endian byte sequence of length"""
    return n.to_bytes(length, 'little')
```

```
inputs = []
for _ in range(num_inputs):
    inputs.append(TxIn.parse(s))
num_outputs = read_varint(s)
outputs = []
for _ in range(num_outputs):
    outputs.append(TxOut.parse(s))
locktime = little_endian_to_int(s.read(4))
return cls(version, inputs, outputs, locktime, testnet=testnet)
```

## 练习 5

对于这笔交易，第二个输入的 ScriptSig、第一个输出的 ScriptPubKey 以及第二个输出的金额分别是什么？

010000000456919960ac691763688d3d3bcea9ad6ecaf875df5339e148a1fc61c6ed7a069e0100 0006a4703440220458bcdef85e6b1c6af5c2669d4830ff86e42dd205c0e089bc2a821657e951 c002201024a10366077f87d6bce1f7100ad8cfa8a064b39d4e8fe4e1a13a7b71aa8180f012102f0 da57e85eec2934a82a585ea337ce2f4998b50ae699dd79f5880e253dafafb7feffffffeb8f51f4 038dc17e6313cf831d4f02281c2a468bde0fafd37f1bf882729e7fd3000000006a473044022078 99531a5d2d59a6de200179928ca900254a36b8ddf8bb75f5f5d7b1cdc26152020008b422690b84 61c52c3cc303b30d23d57435187b2c7c361e9aae3649071c1a7160121035c93d9ac96881f19ba 1f686f15f009ded7c62efe85a872e6a19b43c15a2937feffffff567bf40595119d1bb8a3037c35 6efd56170b64cbc160fb028fa10704b45d775000000006a47304402204c7c818424c7f7911da 6cdc59655a70af1c5eaf17c69dadbfc74ffa0b662f02207599e08bc8023693a4de49527d42c3 4210f7a7d1ddfc8492b654a11e72620a0012120158b46fdbf65d0172b798ae8850aa0dae49 abfb84c81ae6e5b251a58ace5cfefffffd63a5e6c16e620f86f375925b21cabaf736c779f88fd 04dcad51d26690f7f345010000006a47304402200633ea0d3314bea0d95b3cd8dadb2ef79ea833 1ffe1e61f762c0f6daea0fabde022029f23b3e9c30f080446150b23852028751635dcee2be669c 2a1686a4b5edf304012103ffd6f4a67e94aba353a008e2e563ff772e6b4cff0ad6006e86ee20df e7520d55fefffff0251430f000000000001976a914ab0c0b2e98b1ab6dbf67d4750b0a56244948 a87988ac005a6202000000001976a914c82d7df364eb6c75be8c80df2b3eda8db57397088ac46 430600

```
>>> from io import BytesIO
>>> from tx import Tx
>>> hex_transaction = '01000000456919960ac691763688d3d3bcea9ad6ecaf875df5339e148a1fc61c6ed7a069e0100\n0006a4703440220458bcdef85e6b1c6af5c2669d4830ff86e42dd205c0e089bc2a821657e951\n c002201024a10366077f87d6bce1f7100ad8cfa8a064b39d4e8fe4e1a13a7b71aa8180f012102f0da57e85eec2934a82a585ea337ce2f4998b50ae699dd79f5880e253dafafb7feffffffeb8f51f4\n038dc17e6313cf831d4f02281c2a468bde0fafd37f1bf882729e7fd3000000006a473044022078\n99531a5d2d59a6de200179928ca900254a36b8ddf8bb75f5f5d7b1cdc26152020008b422690b84\n61c52c3cc303b30d23d57435187b2c7c361e9aae3649071c1a7160121035c93d9ac96881f19ba1f686f15f009ded7c62efe85a872e6a19b43c15a2937feffffff567bf40595119d1bb8a3037c356efd56170b64cbc160fb028fa10704b45d775000000006a47304402204c7c818424c7f7911da\n2204c7f781842c7f7911da6cdc59655a70af1c5eaf17c69dadbfc74ffa0b662f02207599e08bc8023693a4de49527d42c3\n4210f7a7d1ddfc8492b654a11e72620a0012120158b46fdbf65d0172b798ae8850aa0dae49abfb84c81ae6e5b251a58ace5cfefffffd63a5e6c16e620f86f375925b21cabaf736c779f88fd04dcad51d26690f7f345010000006a47304402200633ea0d3314bea0d95b3cd8dadb2ef79ea8331ffe1e61f762c0f6daea0fabde022029f23b3e9c30f080446150b23852028751635dcee2be669c2a1686a4b5edf304012103ffd6f4a67e94aba353a008e2e563ff7722eb4cff0ad6006e86ee20dfe7520d55fefffff0251430f00000000001976a914ab0c0b2e98b1a'
```

