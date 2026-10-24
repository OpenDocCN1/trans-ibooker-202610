You can select a sheet as follows:

```
```python
currSheet = wb['Sheet1']
print(currSheet)
print(type(currSheet))
```
```

The output is as follows:

```
```text
<Worksheet "Sheet1">
<class 'openpyxl.worksheet.worksheet.Worksheet'>
```
```

Similarly, the following code has the same effect:

```
```python
currSheet = wb[wb.sheetnames[0]]
print(currSheet)
print(type(currSheet))
```
```

You can print the name of the current sheet as follows:

```
```python
print(currSheet.title)
```
```

The output is as follows:

Sheet1

You can print the value of a cell as follows:

```
```python
var1 = currSheet['A1']
print(var1.value)
```
```

The output is as follows:

Food Item

The other way to do the same activity is as follows:

```
```python
print(currSheet['B1'].value)
```
```

You can do this another way as follows:

```
```python
var2 = currSheet.cell(row=2, column=2)
print(var2.value)
```
```