```
from rpn_io import open_rpn_file
def main():
    a_list = open_rpn_file()
    if not a_list:
        print('Bye!')
        return

for a_line in a_list:
    a_line = a_line.strip()
    if a_line.startswith('PRINTS'):
        do_prints(a_line[7:])
    elif a_line.startswith('PRINTLN'):
        do_println(a_line[8:])
    elif a_line.startswith('PRINTVAR'):
        do_printvar(a_line[9:], sym_tab)
    elif a_line.startswith('INPUT'):
        do_input(a_line[6:], sym_tab)
    elif a_line:
        tokens, unknown = scanner.scan(a_line)
        if unknown:
            print('Unrecognized input:', unknown)
```