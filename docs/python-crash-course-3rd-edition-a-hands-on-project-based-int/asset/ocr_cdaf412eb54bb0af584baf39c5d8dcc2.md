```
super().__init__(make, model, year)
self.battery_size = 40

def describe_battery(self):
    """Print a statement describing the battery size."""
    print(f"This car has a {self.battery_size}-kWh battery.")

my_leaf = ElectricCar('nissan', 'leaf', 2024)
print(my_leaf.get_descriptive_name())
my_leaf.describe_battery()
```

We add a new attribute `self.battery_size` and set its initial value to 40 ❶. This attribute will be associated with all instances created from the `ElectricCar` class but won’t be associated with any instances of `Car`. We also add a method called `describe_battery()` that prints information about the battery ❷. When we call this method, we get a description that is clearly specific to an electric car:

```
2024 Nissan Leaf
This car has a 40-kWh battery.
```

There’s no limit to how much you can specialize the `ElectricCar` class. You can add as many attributes and methods as you need to model an electric car to whatever degree of accuracy you need. An attribute or method that could belong to any car, rather than one that’s specific to an electric car, should be added to the `Car` class instead of the `ElectricCar` class. Then anyone who uses the `Car` class will have that functionality available as well, and the `ElectricCar` class will only contain code for the information and behavior specific to electric vehicles.

# Overriding Methods from the Parent Class

You can override any method from the parent class that doesn’t fit what you’re trying to model with the child class.