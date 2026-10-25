```
print('x values: {}, {}'.format(x1, x2))

main()
```

The key lines in this version are in the following **if** statement:

Click here to view code image

```
if len(sys.argv) > 3:
    s1, s2, s3 = sys.argv[1], sys.argv[2],
        sys.argv[3]
    else:
        s1 = input('Enter a: ')
        s2 = input('Enter b: ')
        s3 = input('Enter c: ')
    a, b, c = float(s1), float(s2), float(s3)
```

If there are at least four elements in **sys.argv** (and therefore three command-line arguments beyond the program name itself), the program uses those strings. Otherwise, the program prompts for the values.

So, from the command line, you’ll be able to run the following:

```
python quad.py 1 -9 20
```

The program then prints these results:

```
x values: 4.0 5.0
```