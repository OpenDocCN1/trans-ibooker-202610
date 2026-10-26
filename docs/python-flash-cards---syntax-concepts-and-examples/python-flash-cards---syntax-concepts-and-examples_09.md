## 列表与元组

![](img/b4d4158283a99b0f9a7bef8cfd6e5671_89_0.png)

列表允许你在一个地方存储多条信息——想存多少就存多少。一旦定义了列表，你就可以访问和处理单个元素、列表的一部分，或一次处理列表中的所有信息。

理解列表的工作原理可以为处理更复杂的数据类型打下坚实基础。这些卡片涵盖列表和元组，元组与列表类似。

### 列表

- 什么是列表？
- 如何定义列表？
- 如何向列表添加元素？

### 从列表中删除元素

- 如何从列表中删除某个元素？
- 如何从列表中删除特定位置的元素？

remove() 方法从列表中删除特定元素：

```
>>> airports = ['ANC', 'JNU', 'SEA', 'SIT']
>>> airports.remove('SEA')
>>> airports
['ANC', 'JNU', 'SIT']
```

如果某个元素在列表中出现多次，则仅删除其第一次出现。

del 关键字使用索引删除列表中特定位置的元素：

```
>>> airports = ['ANC', 'JNU', 'SEA', 'SIT']
>>> del airports[2]
>>> airports
['ANC', 'JNU', 'SIT']
```

pop() 方法删除并返回列表中的最后一个元素。pop() 方法也可以接受索引，并删除和返回该索引处的元素：

```
>>> airports = ['ANC', 'JNU', 'SEA', 'SIT']
>>> airport = airports.pop()
>>> airport
'SIT'
>>> airport = airports.pop(0)
>>> airport
'ANC'
```



### 列表切片

- 什么是切片？
- 如何创建从列表开头开始的切片？
- 如何创建列表末尾的切片？

列表副本让你可以在操作副本内容的同时，不影响原列表。

通过切片并省略两个索引，即可创建列表的副本。这会生成一个从列表开头到结尾的切片。

```python
>>> states = ['CA', 'CO', 'CT', 'DE']
>>> my_states = states[:]
>>> my_states
['CA', 'CO', 'CT', 'DE']
```

你对新列表所做的修改不会影响原列表：

```python
>>> my_states.append('HI')
>>> my_states
['CA', 'CO', 'CT', 'DE', 'HI']
>>> states
['CA', 'CO', 'CT', 'DE']
```

### 遍历列表

使用 for 循环遍历列表中的所有元素：

```python
countries = ['CAN', 'CHL', 'IND', 'AUS']
for country in countries:
    print(f"I'm flying to {country}.")
```

我要飞往 CAN。
我要飞往 CHL。
我要飞往 IND。
我要飞往 AUS。

使用切片遍历列表的一部分：

```python
for country in countries[:2]:
    print(f"I've been to {country}.")
```

我去过 CAN。
我去过 CHL。

### 列表排序

- 如何临时对列表排序？
- 如何永久对列表排序？

要以自然顺序的反向对列表排序，请将 reverse 参数设置为 True：

```python
>>> vowels = ['a', 'i', 'e', 'u', 'o']
>>> sorted(vowels, reverse=True)
['u', 'o', 'i', 'e', 'a']
```

reverse 参数也适用于 sort() 和 sorted()。若要反转列表的原始顺序，请使用 reverse() 方法：

```python
>>> vowels = ['a', 'i', 'e', 'u', 'o']
>>> vowels.reverse()
>>> vowels
['o', 'u', 'e', 'i', 'a']
```

sort() 或 sorted() 的 reverse 参数会得到自然顺序的反向；reverse() 方法会反转列表的原始顺序。

### 列表推导式

```python
>>> nums = [10**exp for exp in range(5)]
>>> nums
[1, 10, 100, 1000, 10000]
```

列表推导式使用方括号、一个生成列表中每个元素的表达式，以及一个作为列表基础的 for 循环。

列表推导式可以修改现有列表中的每个元素：

```python
>>> states = ['CA', 'CO', 'CT']
>>> lower_states = [state.lower()
...   for state in states]
>>> lower_states
['ca', 'co', 'ct']
```
