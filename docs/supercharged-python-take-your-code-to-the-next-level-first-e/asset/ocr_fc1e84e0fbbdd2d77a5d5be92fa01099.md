```
import poly # Import the file poly.py
def main():
    r = float(input('Enter radius:'))
    print('Area of circle is',
    poly.get_circle_area(r))
    x = float(input('Enter side:'))
    print('Area of square is',
    poly.get_square_area(x))
main()
# File poly.py ------------------------------------
def get_circle_area(radius):
    pi = 3.141593
    return 3.141592 * radius * radius
def get_square_area(side):
    return side * side
def get_triangle_area(height, width):
    return height * width * 0.5
```

The main module is do_areas.py. The file poly.py is in
the same directory. After you start do_areas.py, the first
thing the program does is to import the file poly.py:

> Click here to view code image

```
import poly # Import the file poly.py
```

When Python reads and executes this statement, it suspends