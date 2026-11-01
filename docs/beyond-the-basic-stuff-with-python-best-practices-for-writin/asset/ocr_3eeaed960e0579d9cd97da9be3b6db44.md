Instead of making the WizardCustomer class inherit methods from WizCoin, we give the WizardCustomer class a purse attribute ①, which contains a WizCoin object. When you use composition, any changes to the WizCoin class's methods won't change the WizardCustomer class's methods. This technique offers more flexibility in future design changes for both classes and leads to more maintainable code.

# Inheritance's Downside

The primary downside of inheritance is that any future changes you make to parent classes are necessarily inherited by all its child classes. In most cases, this tight coupling is exactly what you want. But in some instances, your code requirements won't easily fit your inheritance model.

For example, let's say we have Car, Motorcycle, and LunarRover classes in a vehicle simulation program. They all need similar methods, such as startIgnition() and changeTire(). Instead of copying and pasting this code into each class, we can create a parent Vehicle class and have Car, Motorcycle, and LunarRover inherit it. Now if we need to fix a bug in, say, the changeTire() method, there's only one place we need to make the change. This is especially helpful if we have dozens of different vehicle-related classes inheriting from Vehicle. The code for these classes would look like this:

```
class Vehicle:
    def __init__(self):
        print('Vehicle created.')
    def startIgnition(self):
        pass  # Ignition starting code goes here.
    def changeTire(self):
        pass  # Tire changing code goes here.

class Car(Vehicle):
    def __init__(self):
        print('Car created.')

class Motorcycle(Vehicle):
    def __init__(self):
        print('Motorcycle created.')

class LunarRover(Vehicle):
    def __init__(self):
        print('LunarRover created.')
```

But all future changes to Vehicle will affect these subclasses as well. What happens if we need a changeSparkPlug() method? Cars and motorcycles have combustion engines with spark plugs, but lunar rovers don't. By favoring composition over inheritance, we can create separate CombustionEngine and ElectricEngine classes. Then we design the Vehicle class so it “has an”