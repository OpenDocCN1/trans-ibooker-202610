```
PRINTS 'Enter side 1: '
INPUT side1
PRINTS 'Enter side 2: '
INPUT side2
total side1 side1 * side2 side2 * + =
hyp total 0.5 ^ =
PRINTS 'Hypotenuse equals '
PRINTVAR hyp

Enter RPN source: rpn_hyp.txt
Enter side 1: 30
Enter side 2: 40
Hypotenuse equals 50.0

sym_tab = { }     # Symbol table (for variables)
stack = []        # Stack to hold the values.
pc = -1           # Program Counter

def main():
    global pc
    a_list = open_rpn_file()
    if not a_list:
        print('Bye!')
```