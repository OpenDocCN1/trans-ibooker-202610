## D. 字典方法

本附录列出了 `dict` 类型的方法。它不涵盖也可与此类型一起使用的函数，例如 `len`。有关这些内容，请参见附录 B。

在每个部分开头的语法显示中，方括号并非字面意思，而是表示可选项。

### 表 D.1. 最常用的字典方法。

| 要执行的任务： | 调用此方法： |
|---|---|
| 从另一个集合添加键/值对 | `update` |
| 清除字典的所有内容 | `clear` |
| 复制另一个字典的所有内容 | `copy` |
| 删除键及其关联的值 | `pop` |
| 按键获取值；如果未找到键，则返回默认值（例如 `None`） | `get` |
| 获取包含所有关联值的序列 | `values` |
| 获取包含所有键的序列 | `keys` |
| 获取所有键值对 | `items` |
| 如果键存在则获取其值；否则插入默认值 | `setdefault` |

## DICT_OBJ.CLEAR()

从现有字典中清除所有元素。不接受参数，并返回值 **None**。

```python
a_dict.clear()
```

## DICT_OBJ.COPY()

通过逐个成员复制，返回字典的浅拷贝。例如：

```python
a_dict = {'pi': 3.14159, 'e': 2.71828 }
b_dict = a_dict.copy()
```

执行这些语句后，b_dict 与 a_dict 具有相同的内容，但它们是两个独立的集合，因此对其中一个的更改不会影响另一个。

## DICT_OBJ.GET(KEY_OBJ, DEFAULT_VAL = NONE)

返回 `dict_obj` 中指定键关联的值。如果未找到键，则该方法返回 `default_val`，如果未指定，则为 **None**。例如：

```python
v = my_dict.get('BrianO')
if v:
    print('Value is: ', v)
else:
    print('BrianO not found.')
```

**get** 的以下用法是创建直方图的有效快捷方式。假设 `wrd_list` 是一个字符串列表，每个字符串包含一个单词。我们可以这样形成这样的列表，并伴随一个空字典。

```python
s = 'I am what I am and that is all that I am'
wrd_list = s.split()
hist_dict = {}
```

现在我们可以使用 **get** 方法来生成我们要查找的单词计数。

```python
for wrd in wrd_list:
    hist_dict[wrd] = hist_dict.get(wrd, 0) + 1
print(hist_dict)
```

这些语句在字典中维护单词/频率对。每次找到新单词时，它都会以值 1 添加到字典中；否则，现有值将加 1。这些语句生成以下输出：

```
{'I': 3, 'am': 3, 'what': 1, 'and': 1, 'that': 2,
'is': 1, 'all': 1, 'am.': 1}
```

## DICT_OBJ.ITEMS()

返回一个元组序列，格式为 (_key_, _value_)，包含字典中的所有键值对。例如：

```python
grades = {'Moe':1.5, 'Larry':1.0, 'BillG':4.0}
print(grades.items())
```

这段代码打印以下内容：

```
dict_items([('Moe', 1.5), ('Larry', 1.0), ('BillG',
4.0)])
```

## DICT_OBJ.KEYS()

返回包含字典中所有键的序列。例如：

```python
grades = {'Moe':1.5, 'Larry':1.0, 'BillG':4.0}
print(grades.keys())
```

这段代码打印以下内容：

```
dict_keys(['Moe', 'Larry', 'BillG'])
```

## DICT_OBJ.POP(KEY [, DEFAULT_VALUE])

返回与 _key_ 关联的值，然后从字典中删除该键值对。如果未找到键，则该方法返回 _default_value_（如果指定）；如果未指定该参数，则引发 **KeyError**。例如：

```python
grades = {'Moe':1.5, 'Larry':1.0, 'BillG':4.0}
print(grades.pop('BillG', None)) # Prints 4.0
print(grades) # Prints grades,
with # BillG removed.
```

## DICT_OBJ.POPITEM()

从字典对象中返回任意键值对并将其删除。（这与“随机对象”不完全相同，因为选择不保证符合真正随机性的统计要求。）键值对作为元组返回。例如：

```python
grades = {'Moe':1.5, 'Larry':1.0, 'BillG':4.0}
print(grades.popitem())
print(grades)
```

这些语句打印以下内容：

```
('BillG', 4.0)
{'Moe': 1.5, 'Larry': 1.0}
```

## DICT_OBJ.SETDEFAULT(KEY, DEFAULT_VALUE=NONE)

返回指定 _key_ 的值。如果找不到键，则该方法插入该键，以及指定为 _default_value_ 的关联值；如果未指定，则该值为 **None**。无论哪种情况，都会返回该值。

例如，以下语句如果存在键 'Stephen Hawking'，则返回其关联的当前值；否则，它插入一个键值对并返回新值 4.0。

```python
print(grades.setdefault('Stephen Hawking', 4.0))
```

## DICT_OBJ.VALUES()

返回包含字典中所有关联值的序列。要将此序列视为列表，可以使用列表转换。例如：

```python
grades = {'Moe':1.5, 'Larry':1.0, 'Curly':1.0,
          'BillG': 4.0}
print(grades.values())
```

这些语句打印以下内容：

```
dict_values([1.5, 1.0, 1.0, 4.0])
```

## DICT_OBJ.UPDATE(SEQUENCE)

此方法通过将序列中的所有键值条目添加到 dict_obj 来扩展字典对象。序列参数可以是另一个字典，也可以是包含键值对的元组序列。

例如，以下语句从 grades1 中的两个条目开始，然后向该字典添加三个更多条目。

```python
grades1 = {'Moe':1.0, 'Curly':1.0}
grades2 = {'BillG': 4.0}
grades3 = [('BrianO', 3.9), ('SillySue', 2.0)]
grades1.update(grades2)
grades1.update(grades3)
print(grades1)
```

执行这些语句时，打印以下内容：

```
{'Moe': 1.0, 'Curly': 1.0, 'BillG': 4.0, 'BrianO': 3.9, 'SillySue': 2.0}
```

## 面向 IT 专业人士的视频培训

- **快速学习**
  只需几小时即可学习新技术。视频培训可以在更短的时间内教授更多内容，且材料通常更容易吸收和记住。

- **观看并学习**
  讲师演示概念，让您看到技术的实际应用。

- **自我测试**
  我们的完整视频课程在整个过程中提供自我评估测验。

- **方便**
  大多数视频均为流媒体，并提供下载课程以供离线观看的选项。

了解更多，浏览我们的商店，并在 [informit.com/video](informit.com/video) 观看免费示例课程。

使用折扣代码 **VIDBOB**，视频课程定价立减 50%***

*折扣代码 VIDBOB 可在 informit.com 上购买的合格书籍享受定价 50% 的折扣。合格书籍包括大多数完整课程视频书籍。图书 + 电子书捆绑包、图书/电子书 + 视频捆绑包、单独的视频课程、粗剪版、Safari Books Online、不可折扣书籍、与我们零售合作伙伴促销的书籍，以及任何作为电子书每日特惠或视频每周特惠的书籍均不享受折扣。折扣不可与其他任何优惠合并使用，也不可兑换现金。优惠可能会更改。

## 在 informit.com/register 注册您的产品

获取额外权益，下次购买立减 **35%**

- 自动获得下次购买 35% 折扣的优惠券，有效期 30 天。请在您的 InformIT 购物车或账户页面的“管理代码”部分查找您的代码。
- 下载可用的产品更新。
- 如果可用，访问奖励材料。*
- 勾选框以接收我们的来信，并获取新版本和相关产品的独家优惠。

*注册权益因产品而异。权益将在您的账户页面的“已注册产品”下列出。

## InformIT.com—值得信赖的技术学习来源

InformIT 是 Pearson（世界领先的教育公司）旗下信息技术品牌的在线家园。在 InformIT.com 上，您可以：

- 购买我们的图书、电子书、软件和视频培训
- 利用我们的特别优惠和促销活动（informit.com/promotions）
- 注册特别优惠和内容通讯（informit.com/newsletters）
- 访问数千个免费章节和视频课程

### 与 InformIT 联系—访问 informit.com/community

Addison-Wesley • Adobe Press • Cisco Press • Microsoft Press • Pearson IT Certification • Prentice Hall • Que • Sams • Peachpit Press




