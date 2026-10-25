```
>>> bs = bytes('Hi there!', encoding='utf-8')
>>> bs
b'Hi there!'
>>> command_str = '''
pi = 3.141592
print('pi/2 = ', pi/2)
'''
>>> cmd = compile(command_str, 'my_mod', 'exec')
>>> exec(cmd)
pi/2 = 1.570796
dir()   # Get attributes of the module.
quot, rem = divmod(203, 10) # Result is 20, remainder 3
beat_list = ['John', 'Paul', 'George', 'Ringo']
for n, item in enumerate(beat_list, 1):
    print('%s. %s' % (n, item))
(1, item0), (2, item1), (3, item2)...
>>> eval('3 * 100 + 55', {}) # This is fine.
355
>>> eval('3 * a + 55', {})   # ERROR; 'a' not defined
```