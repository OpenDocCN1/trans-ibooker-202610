# A child class can customize the behavior of parent class methods by using `super()` to access those methods.

This BikeTrail class modifies the behavior of the method `run_trail()`.

```
```python
class BikeTrail(Trail):
    """Represent a bike trail."""

    def __init__(self, dest, len=0):
        --snip--

    def run_trail(self):
        """Simulate running the trail."""
        if self.bikes_only:
            print("You can't run this trail!")
        else:
            super().run_trail()

cross_trail = BikeTrail("Harbor Mountain", 5)
cross_trail.run_trail()
```
```

The method first checks if this is a bikes-only trail; if it is, an appropriate message prints:

```
```text
You can't run this trail!
```
```

If the trail allows runners, the method calls the parent method `run_trail()`, inheriting the behavior of a regular `Trail`.