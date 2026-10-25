## Python 交互式示例

### bytes 与 compile/exec

```python
>>> bs = bytes('Hi there!', encoding='utf-8')
>>> bs
b'Hi there!'
>>> command_str = '''
pi = 3.141592
print('pi/2 = ', pi/2)
'''
>>> cmd = compile(command_str, 'my_mod', 'exec')
>>> exec(cmd)
pi/2 = 1.570796
dir()   # Get attributes of the module.
quot, rem = divmod(203, 10) # Result is 20, remainder 3
beat_list = ['John', 'Paul', 'George', 'Ringo']
for n, item in enumerate(beat_list, 1):
    print('%s. %s' % (n, item))
(1, item0), (2, item1), (3, item2)...
>>> eval('3 * 100 + 55', {}) # This is fine.
355
>>> eval('3 * a + 55', {})   # ERROR; 'a' not defined
```

### eval 与 filter 示例

```python
>>> from math import *
>>> a = 25
>>> dict_1 = {'tan': tan, 'radians': radians, 'a': a }
>>> eval('1000 * tan(radians(a))', dict_1)
176.326980708465

eval('a * a + 100', {}, locals())

def is_neg(x):
    return x < 0

my_list = [10, -1, 20, -3, -2.5, 30]
print(list(filter(is_neg, my_list)))

n = 1
yyy = float(n)      # Assign 1.0 to yyy.
amt = float('-23E01') # Assign -23E01 to amt.

amt = float()       # Assign 0.0 to amt.

>>> frozenset((1, 2, 2, 3, 3, 4, 1))
frozenset({1, 2, 3, 4})

my_name = input('Enter your name, please: ')
my_age = int(input('Enter your age, please: '))
```

### 类与迭代器

```python
>>> class Floopy(int):
    pass

>>> f = Floopy()
>>> issubclass(Floopy, int)
True
>>> issubclass(int, Floopy)
False
>>> issubclass(int, int)
True
>>> issubclass(f, int)  # TypeError: f is not a class

>>> gen = iter(5)       # This raises a TypeError

>>> gen = iter([1, 2, 3])
>>> next(gen)
1
>>> next(gen)
2
>>> next(gen)
3
>>> next(gen)           # StopIteration exception

>>> new_list = list()   # Init new_list to empty list.
>>> new_list
[]
```

### map 与 fractions

```python
>>> def multy(a, b, c):
    return a * b * c
>>> m = map(multy, [1, 20], [1, 20], [1, 50])
>>> print(list(m))
[1, 20000]

>>> from fractions import Fraction
>>> a_list = [1, Fraction('5/2'), 2.1]
>>> max(a_list)
Fraction(5, 2)

>>> from fractions import Fraction
>>> a_list = [1, Fraction('5/2'), 2.1]
>>> min(a_list)
1
```

### 字符串与 reversed

```python
>>> my_str = "Hi, I'm Brian!"
>>> print(repr(my_str))
"Hi, I'm Brian!"

>>> print(list(reversed([1, 2, 3])))
[3, 2, 1]

>>> str(reversed('Wow, Bob, wow!'))
'<reversed object at 0x11124bc88>'
```

### 集合操作

```python
>>> my_list = [11, 11, 3, 5, 3, 3, 3]
>>> my_set = set(my_list)
>>> my_set
{3, 11, 5}

attr_str = 'breed'
...
setattr(d, attr_str, 'Dane') # set d.breed = 'Dane'

bs = b'Hello!' # Guaranteed to hold exactly six bytes
s = str(bs, encoding='utf-8')
print(s) # Print a normal string, ? bytes per char.

def get_avg(a_list):
    return sum(a_list)/len(a_list)

a = [1, 2, 3]
b = [10, 20, 30]
c = [i[0] + i[1] for i in zip(a, b)]

a = [1, 2, 3]
b = [10, 20, 30]
c = []
for i in range(min(len(a), len(b))):
    c.append(a[i] + b[i])
```

```python
a_set = {1, 2, 3}
a_set.add(4)  # Adds 4 to the set.

a_set = {1, 2, 3, 4}
b_set = {3, 4, 5, 6}
c = a_set.difference(b_set)
print(c)  # Prints {1, 2}
print(b_set.difference(a_set))  # Prints {5, 6}

print(a_set - b_set)  # Prints {1, 2}

a_set -= b_set  # Put results of diff. in a_set

a_set = {1, 2, 3, 4}
b_set = {3, 4, 5, 6}
print(a_set.intersection(b_set))  # Prints {3, 4}

a_set = {'Moe', 'Larry', 'Curly'}
a_set.discard('Curly')
print(a_set)  # Prints {'Moe', 'Larry'}

print(a_set & b_set)  # Prints {3, 4}

a_set &= b_set  # Put the intersection in a_set

{1, 2}.issubset({1, 2, 3})  # Produces the value True.
{1, 2}.issubset({1, 2})  # Also produces True.
```

```python
{1, 2}.issuperset({1})     # Produces the value True.
{1, 2}.issuperset({1, 2})   # Also produces True.

a_set = {'Moe', 'Larry', 'Curly'}
stooge = a_set.pop()
print(stooge, a_set)

a_set = {1, 2, 3, 4}
b_set = {3, 4, 5, 6}
print(a_set.symmetric_difference(b_set))

print(a_set ^ b_set)        # Prints {1, 2, 5, 6}

a_set ^= b_set              # Put the sym. difference in a_set

a_set = {1, 2, 3, 4}
b_set = {3, 4, 5, 6}
print(a_set.union(b_set))   # Prints {1, 2, 3, 4, 5, 6}

print(a_set | b_set)        # Prints {1, 2, 3, 4, 5, 6}

a_set |= b_set              # Put the union in a_set

a_set = {1, 2, 3}
a_set |= {200, 300}
print(a_set)                # Prints {1, 2, 3, 200, 300}
```




