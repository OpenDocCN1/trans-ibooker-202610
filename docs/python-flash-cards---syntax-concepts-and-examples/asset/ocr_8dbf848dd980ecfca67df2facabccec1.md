# You store classes in modules and import them the same way as functions.

Here we store the classes Trail and BikeTrail in the trails.py file:

```
class Trail():
    """A class to represent trails."""
    --snip--

class BikeTrail(Trail):
    """Represent a bike trail."""
    --snip--
```

Now we can import these classes into another file:

```
from trails import Trail, BikeTrail

verst = Trail("Mt. Verstovia", 4)
cross_trail = BikeTrail("Harbor Mt", 5)
```

Use dot notation to import the entire module:

```
import trails

verst = trails.Trail("Mt. Verstovia", 4)
cross_trail = trails.BikeTrail("Harbor Mt", 5)
```

Use aliases when importing classes. Store classes to organize your code and make the classes available to multiple programs.