```
array([(101, 70.5, 21, 2.3, ' Harvard'),
       (110, 69.5, 22, 3.1, ' MIT'),
       (130, 76. , 21, 3.5, ' Cal Tech'),
       (120, 72.5, 29, 3.7, ' Yale'),
       (120, 73.2, 33, 2.9, ' Harvard'),
       (105, 68. , 35, 3. , ' U. of Wash.'),
       (107, 74. , 44, 2.7, ' Tacoma Comm.'),
       'College'),
       (140, 67. , 30, 3.1, ' Oregon State'),
       (100, 72.5, 31, 2. , ' UCLA')],
      dtype=[('IQ', 'i2'), ('Height', 'f4'),
             ('Age', 'i2'), ('Rating', 'f4'),
             ('College', 'U30')])
```

There’s at least one quirk in this example. The strings all have
a leading space. That’s because the delimiter was only a comma.
There are several ways to solve this problem. The simplest way
is probably to make the delimiter into a combination comma
and space (‘, ’).

```
team_a = np.loadtxt('team_data.txt', dt,
                   delimiter=', ')
```

You can now isolate columns as you choose and manipulate
them or analyze the data, as we demonstrated in the previous
section.

```
iq_a = team_a['IQ']
ht_a = team_a['Height']
age_a = team_a['Age']
rat_a = team_a['Rating']
```