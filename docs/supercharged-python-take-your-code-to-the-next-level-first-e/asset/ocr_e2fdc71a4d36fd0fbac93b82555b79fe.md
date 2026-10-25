```
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __add__(self, other):
        """ Return a point containing self+other."""
        newx = self.x + other.x
        newy = self.y + other.y
        return Point(newx, newy)

    def __sub__(self, other):
        """ Return the distance between points."""
        dx = self.x - other.x
        dy = self.y - other.y
        return (dx * dx + dy * dy) ** 0.5

    def __mul__(self, n):
        """ Return point * a scalar number, n."""
        newx = self.x * n
        newy = self.y * n
        return Point(newx, newy)

    def __neg__(self):
        newx = -self.x
        newy = -self.y
        return Point(newx, newy)
```