## 字典操作示例

```python
a_dict = {'pi': 3.14159, 'e': 2.71828 }
b_dict = a_dict.copy()

v = my_dict.get('BrianO')
if v:
    print('Value is: ', v)
else:
    print('BrianO not found.')

s = 'I am what I am and that is all that I am'
wrd_list = s.split()
hist_dict = {}

for wrd in wrd_list:
    hist_dict[wrd] = hist_dict.get(wrd, 0) + 1
print(hist_dict)

{'I': 3, 'am': 3, 'what': 1, 'and': 1, 'that': 2,
 'is': 1, 'all': 1, 'am.': 1}

grades = {'Moe':1.5, 'Larry':1.0, 'BillG':4.0}
print(grades.items())

dict_items([('Moe', 1.5), ('Larry', 1.0), ('BillG', 4.0)])

grades = {'Moe':1.5, 'Larry':1.0, 'BillG':4.0}
print(grades.keys())

dict_keys(['Moe', 'Larry', 'BillG'])

grades = {'Moe':1.5, 'Larry':1.0, 'BillG':4.0}
print(grades.pop('BillG', None))  # Prints 4.0
print(grades)                     # Prints grades, with
                                  # BillG removed.

grades = {'Moe':1.5, 'Larry':1.0, 'BillG':4.0}
print(grades.popitem())
print(grades)

print(grades.setdefault('Stephen Hawking', 4.0))

grades = {'Moe':1.5, 'Larry':1.0, 'Curly':1.0,
          'BillG': 4.0}
print(grades.values())

grades1 = {'Moe':1.0, 'Curly':1.0}
grades2 = {'BillG': 4.0}
grades3 = [('Brian0', 3.9), ('SillySue', 2.0)]
grades1.update(grades2)
grades1.update(grades3)
print(grades1)

{'Moe': 1.0, 'Curly': 1.0, 'BillG': 4.0, 'Brian0': 3.9,
 'SillySue': 2.0}
```
