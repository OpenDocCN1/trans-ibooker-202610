## 字典

![](img/b4d4158283a99b0f9a7bef8cfd6e5671_111_0.png)

字典允许你在各条信息之间建立联系，例如州的缩写和全名。字典中可以存储的信息量没有限制。

与列表一样，你可以处理字典中的全部信息，也可以处理字典中的任意部分。这些卡片展示了如何创建和使用字典。

### 字典方法

- 字典方法 get() 有什么作用？
- keys() 方法有什么作用？
- values() 方法有什么作用？
- items() 方法有什么作用？

如果字典中存在某个键，get() 方法会返回与该键关联的值。如果键不存在，get() 返回 None，而不是报错。

如果键不存在，你还可以传入一个默认值作为返回值（示例延续自卡片 4.1）：

```python
>>> capitals.get('AK')
'Juneau'
>>> capitals.get('WY')
>>> capitals.get('WY', 'unknown')
'unknown'
```

keys()、values() 和 items() 方法返回字典的不同方面，也有助于你遍历字典：

```python
>>> capitals.keys()
dict_keys(['AK', 'AL', 'AZ'])
>>> capitals.values()
dict_values(['Juneau', 'Montgomery', 'Phoenix'])
>>> capitals.items()
dict_items([('AK', 'Juneau'), ('AL', 'Montgomery'), ('AZ', 'Phoenix')])
```

根据你要对每个元素执行的操作，选择其中一个方法。

### 遍历字典

- 如何遍历字典中的所有键？
- 如何遍历字典中的所有值？
- 如何遍历字典中的所有键值对？

遍历字典可以只访问键。这是遍历字典时的默认行为（示例延续自卡片 4.1）：

```python
for state in capitals:
    print(f"State: {state}")
```

```
State: AK
State: AL
State: AZ
```

你也可以只访问字典中的值：

```python
for capital in capitals.values():
    print(f"Capital: {capital}")
```

```
Capital: Juneau
Capital: Montgomery
Capital: Phoenix
```

若要同时处理键和值，请使用 items() 方法：

```python
for state, capital in capitals.items():
    print(f"{capital}, {state}")
```

```
Juneau, AK
Montgomery, AL
Phoenix, AZ
```

### 字典示例

- 包含多个相似对象的字典是什么样的？
- 表示单个对象的字典是什么样的？

如果某个字典包含关于一个对象的多种信息，那么将一组字典存储在列表中会很有帮助：

```python
musicians = []

e_fitzgerald = {
    'first': 'Ella',
    --snip--
}
musicians.append(e_fitzgerald)

j_joplin = {
    'first': 'Janis',
    --snip--
}
musicians.append(j_joplin)
--snip--
```

要处理这类数据，请遍历列表，并依次处理每个字典：

```python
for musician in musicians:
    full_name = f"{musician['first']} "
    full_name += f"{musician['last']}"
    print(full_name)
```

```
Ella Fitzgerald
Janis Joplin
```

字典的键可以与一个值列表相关联：

```python
states_visited = {
    'Eric': ['AK', 'WY', 'WA', 'NH'],
    'Isaac': ['CO', 'NY', 'AK'],
    --snip--
}
```

当你遍历这些字典项时，每个值都是一个列表。为此，请在用于遍历字典的 for 循环内嵌套一个用于遍历列表的 for 循环：

```python
for name, states in states_visited.items():
    print(f"{name} has visited:")
    for state in states:
        print(f"  {state}")
```

```
Eric has visited:
AK
WY
WA
NH

Isaac has visited:
CO
NY
AK
```
