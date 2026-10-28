```
```python
print(my_mustang.get_descriptive_name())
my_leaf = ElectricCar('nissan', 'leaf', 2024)
print(my_leaf.get_descriptive_name())
```
```

You import multiple classes from a module by separating each class with a comma ❶. Once you’ve imported the necessary classes, you’re free to make as many instances of each class as you need.

In this example we make a gas-powered Ford Mustang ❷ and then an electric Nissan Leaf ❸:

```
```
2024 Ford Mustang
2024 Nissan Leaf
```
```

# **Importing an Entire Module**

You can also import an entire module and then access the classes you need using dot notation. This approach is simple and results in code that is easy to read. Because every call that creates an instance of a class includes the module name, you won’t have naming conflicts with any names used in the current file.

Here’s what it looks like to import the entire car module and then create a regular car and an electric car:

```
```python
# my_cars.py

import car

my_mustang = car.Car('ford', 'mustang', 2024)
print(my_mustang.get_descriptive_name())

my_leaf = car.ElectricCar('nissan', 'leaf', 2024)
print(my_leaf.get_descriptive_name())
```
```

First we import the entire car module ❶. We then access the classes we need through the *module_name.ClassName*