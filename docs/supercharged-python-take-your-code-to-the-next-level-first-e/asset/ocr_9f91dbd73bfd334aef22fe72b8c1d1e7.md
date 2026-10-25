```
movie_dict = { 'Star Bores': 5.0,
    'The Oddfather': 4.5,
    'Piranha: The Revenge': 2.0 }

my_str = 'hello, Dave, this is Hal.'
my_str[0] = 'H'      # ERROR!

n = int('10001', 2)  # Interpret in binary radix.

n1 = int('775', 8)
n2 = int('1E', 16)
print('775 octal and 16 hex:', n1, n2)

n = 1007
s = str(n)   # Convert to '1007'
print('The length of', n, 'is', len(s), 'digits.')

The length of 1007 is 4 digits.

dog1_str = 'Rover'      # Assignment
dog2_str = dog1_str   # Create alias for.

dog1_str == dog2_str  # True!
dog1_str == 'Rover'   # True!
```