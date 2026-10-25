```
pi = 3.14159265
phi = 1.618

fss = '{:.3} + {:.3} = {:.3}'
print(fss.format(pi, phi, pi + phi))

fss = '  {:10.3f}\n  {:10.3f}'
print(fss.format(22.1, 1000.007))

fss = '  {:10,.3f}\n  {:10,.3f}'
print(fss.format(22333.1, 1000.007))

fss = ' {:10.2f}'
for x in [22.7, 3.1415, 555.5, 29, 1010.013]:
    print(fss.format(x))

print('{:.5}'.format('Superannuated.'))  # Prints 'Super'
print('{:.5}'.format('Excellent!'))      # Prints 'Excel'
print('{:.5}'.format('Sam'))              # Prints 'Sam'

print('{:b}  {:b}  {:b}'.format(5, 6, 16))
print('{:o},  {:x},  {:X}'.format(63, 63, 63))
print('{0:o},  {0:x},  {0:X}'.format(63))
print('{0:#o},  {0:#x},  {0:#X}'.format(63))
print('You own {:%} of the shares.'.format(.517))
```