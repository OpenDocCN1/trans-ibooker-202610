```
```python
motorcycles = ['honda', 'yamaha', 'suzuki']
print(motorcycles)

del motorcycles[0]
print(motorcycles)
```
```

Here we use the del statement to remove the first item, 'honda', from the list of motorcycles:

```
```text
['honda', 'yamaha', 'suzuki']
['yamaha', 'suzuki']
```
```

You can remove an item from any position in a list using the del statement if you know its index. For example, here’s how to remove the second item, 'yamaha', from the list:

```
```python
motorcycles = ['honda', 'yamaha', 'suzuki']
print(motorcycles)

del motorcycles[1]
print(motorcycles)
```
```

The second motorcycle is deleted from the list:

```
```text
['honda', 'yamaha', 'suzuki']
['honda', 'suzuki']
```
```

In both examples, you can no longer access the value that was removed from the list after the del statement is used.

# Removing an Item Using the pop() Method

Sometimes you’ll want to use the value of an item after you remove it from a list. For example, you might want to get the x and y position of an alien that was just shot down, so you can draw an explosion at that position. In a web application, you might want to remove a user from a list of active members and then add that user to a list of inactive members.