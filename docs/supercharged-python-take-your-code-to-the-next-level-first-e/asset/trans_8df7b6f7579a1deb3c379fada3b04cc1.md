

plt.hist(A, bins=80, color='g')
plt.title('Normal Distribution in 80 Bins')
plt.show()

plt.histogram(A [, bins=10] [, keyword_args] )

import numpy as np
import matplotlib.pyplot as plt

A = np.random.standard_normal(2000000)
A = A * 10 + 100
B = np.histogram(A, 50)[0]
plt.plot(B)
plt.show()

import numpy as np
import matplotlib.pyplot as plt

A = np.random.standard_normal(2000000)
A = A * 10 + 100
B, X = np.histogram(A, 50)
X = (X[1:]+X[:-1])/2   # 使用箱中心而非边缘。
plt.plot(X, B)         # 针对值而非箱编号进行绘图。
plt.show()             #   箱编号。

X = (X[1:]+X[:-1])/2   # 使用箱中心而非边缘

(cosine(theta), sine(theta))

import numpy as np
import matplotlib.pyplot as plt
theta = np.linspace(0, 2 * np.pi, 1000)
plt.plot(np.cos(theta), np.sin(theta))
plt.show()

import numpy as np
import matplotlib.pyplot as plt
theta = np.linspace(0, 2 * np.pi, 1000)
plt.plot(np.cos(theta), np.sin(theta))
plt.axis('equal')
plt.show()

plt.pie(array_data, labels=None, colors=None)

A_data = [3.7, 2.5, 1.9, 0.5]
A_labels = ['Poker', 'Chess', 'Comic Books', 'Exercise']
A_colors = ['k', 'g', 'r', 'c']

import numpy as np
import matplotlib.pyplot as plt
plt.pie(A_data, labels=A_labels, colors=A_colors)
plt.title('Relative Hobbies of Dev. Team')
plt.axis('equal')
plt.show()

D. P. = A[0]*B[0] + A[1]*B[1] + ... + A[N-1] * B[N-1]

>>> import numpy as np
>>> A = np.ones(5)
>>> B = np.arange(5)
>>> print(A, B)
[1. 1. 1. 1. 1.] [0 1 2 3 4]
>>> np.dot(A, A)
5.0
>>> np.dot(A, B)
10.0
>>> np.dot(B, B)
30

D. P. = 0*0 + 1*1 + 2*2 + 3*3 + 4*4
    = 0 + 1 + 4 + 9 + 16
    = 30

A = np.arange(6).reshape(2,3)
B = np.arange(6).reshape(3,2)
C = np.dot(A, B)
print(A, B, sep='\n\n')
print('\nDot product:\n', C)

[[0 1 2]
 [3 4 5]]

[[0 1]
 [2 3]
 [4 5]]

Dot product:
[[10 13]
 [28 40]]

>>> print(np.dot([10, 15, 30], B))
[150, 205]

numpy.outer(A, B, out=None)

>>> import numpy as np
>>> A = np.array([0, 1, 2])
>>> B = np.array([100, 200, 300, 400])
>>> print(np.outer(A, B))
[[   0    0    0    0]
 [ 100  200  300  400]
 [ 200  400  600  800]]

s = str(np.outer(A, A))
print(' ' + s.replace('[', '').replace(']', ''))
from mpl_toolkits.mplot3d import Axes3D
import matplotlib.pyplot as plt
import numpy as np
fig = plt.figure()
ax = fig.add_subplot(111, projection='3d')
# Make data
ua = np.linspace(0, 2 * np.pi, 100)
va = np.linspace(0, np.pi, 100)
X = 10 * np.outer(np.cos(ua), np.sin(va))
Y = 10 * np.outer(np.sin(ua), np.sin(va))
Z = 10 * np.outer(np.ones(np.size(ua)), np.cos(va))
# Plot the surface
ax.plot_surface(X, Y, Z, color='w')
plt.show()
numpy.pmt(rate, nper, pv [, fv] [, when ])
>>> import numpy as np
>>> np.pmt(0.065 / 12, 20 * 12, 250000)
-1863.9328387775

import numpy as np
def monthly_payment():
    ''Calculate monthly payment, after
    getting input data and calling np.pmt.''

    # Calculate monthly rate.
    s = 'Enter rate as a yearly percentage fig.: '
    rate = (float(input(s)) / 100) / 12

    # Calculate no. of periods
    nyears = int(input('Enter number of years: '))
    nper = nyears * 12

    # Get loan amount.
    pv = float(input('Enter amount of loan: '))

    # Print results
    payment= -1 * np.pmt(rate, nper, pv)
    print('The monthly payment is: $%.2f' % payment)

>>> monthly_payment()
Enter rate as a yearly percentage fig.: 5.0
Enter number of years: 30
Enter amount of loan: 155000
The monthly payment is: $832.07

numpy.ppmt(rate, per, nper, pv [, fv] [, when])
numpy.ipmt(rate, per, nper, pv [, fv] [, when])

import numpy as np
import matplotlib.pyplot as plt

# Set up basic parameters
rate = 0.05 / 12   # Assume 5% a year.
nper = 30 * 12     # Assume 30 years on loan.
pv = 155000        # $155,000 is amount of loan.

# Generate total payment
Total = -1 * np.pmt(rate, nper, pv)

# Plot each month's payment (A) as a ratio of Total
A = np.linspace(0, nper)
B = -1 * np.ppmt(rate, A, nper, pv) / Total
plt.plot(A, B)

# Set labels of axes, and display
plt.xlabel('Months')
plt.ylabel('Percentage Applied to Principal')
plt.show()

plt.yticks(np.arange(0.0, 1.1, 0.2),
           ('0', '20%', '40%', '60%', '80%', '100%'))

plt.xticks(np.arange(0, 361, 60),
           ('2020', '2025', '2030', '2035', '2040',
            '2045', '2050') )

>>> Words = np.array(('To', 'be', 'orNotToBe'))
>>> Words
array(['To', 'be', 'orNotToBe'], dtype='<U9')
>>> Words[0] = 'My uncle is Caesar.'
>>> Words[0]
'My uncle '

>>> Words = np.array(('To', 'be', 'orNotToBe'),
... dtype = 'U20')
>>> Words[0] = 'My uncle is Caesar.'
>>> Words[0]
'My uncle is Caesar.'

dtype = [(name1, format1), (name2, format2) ...]

>>> X = np.array([(12, 33, 'Red'),
... (0, 1, 'Blue'),
... (27, 103, 'Yellow'),
... (-1, -2, 'Blue') ],
... dtype = [('a', 'i4'), ('b', 'i4'), ('color', 'U10')])
>>> print(X)
[(12, 33, 'Red') ( 0, 1, 'Blue') (27, 103, 'Yellow')
(-1, -2, 'Blue')]
dtype = [('a', 'i4'), ('b', 'i4'), ('color', 'U10')]

>>> C = X['color']
>>> print(C)
['Red' 'Blue' 'Yellow' 'Blue']
101, 70.5, 21, 2.3, Harvard
110, 69.5, 22, 3.1, MIT
130, 76.0, 21, 3.5, Cal Tech
120, 72.5, 29, 3.7, Yale
120, 73.2, 33, 2.9, Harvard
105, 68.0, 35, 3.0, U. of Wash.
107, 74.0, 44, 2.7, Tacoma Comm. College
140, 67.0, 30, 3.1, Oregon State
100, 72.5, 31, 2.0, UCLA
dt=[('IQ', 'i2'), ('Height', 'f4'), ('Age', 'i2'),
    ('Rating', 'f4'), ('College', 'U30')]
array = np.loadtxt(fname, dtype=<class 'float'>,
                   delimiter)
team_a = np.loadtxt('team_data.txt', dt, delimiter=',')

array([(101, 70.5, 21, 2.3, ' Harvard'),
       (110, 69.5, 22, 3.1, ' MIT'),
       (130, 76. , 21, 3.5, ' Cal Tech'),
       (120, 72.5, 29, 3.7, ' Yale'),
       (120, 73.2, 33, 2.9, ' Harvard'),
       (105, 68. , 35, 3. , ' U. of Wash.'),
       (107, 74. , 44, 2.7, ' Tacoma Comm. College'),
       (140, 67. , 30, 3.1, ' Oregon State'),
       (100, 72.5, 31, 2. , ' UCLA')],
      dtype=[('IQ', 'i2'), ('Height', 'f4'),
             ('Age', 'i2'), ('Rating', 'f4'),
             ('College', 'U30')])
team_a = np.loadtxt('team_data.txt', dt, delimiter=', ')
[101 110 130 120 120 105 107 140 100]
print('Mean IQ of the dev. team is %.2f. '%
      np.mean(iq_a))
print('Std. dev. of team\'s IQ is %.2f.' % np.std(iq_a))
Mean IQ of the dev. team is 114.78.
Std. dev. of team's IQ is 12.95.
>>> np.corrcoef(iq_a, ht_a)[0, 1]
-0.023465749537744517
new_a = np.array((100, 70, 18, 5.5, 'C.C.C.'), dtype=dt)
team_a = np.append(team_a, new_a)

np.savetxt(fname, array, fmt='%.18e', newline='\n',
     header='', footer='')

fmt_str = '%i, %.1f, %i, %.1f, %s'
np.savetxt('team_data.txt', team_a, fmt=fmt_str)

# File run_me.py ------------------------------------

import printstuff         # Import printstuff.py.

printstuff.print_this('thing') # Call imported func.

# File printstuff.py ------------------------------------

def print_this(str1):
    print('Print this %s.' % str1)

printstuff.print_this('thing')

# File run_me.py  ------------------------------------

import printstuff         # Import printstuff.py

printstuff.print_this('thing') # Call the func

# File printstuff.py ------------------------------------

def print_this(str1):
    print('Print this %s.' % str1)

# File run_me.py ------------------------------------
import printstuff       # Import printstuff.py file,
                       #   and run the code there.
#----------------------------------------------------
# File printstuff.py
print('Print this thing.')
# File run_me.py ------------------------------------
import printstuff       # Import printstuff.py file.
# The next statement then uses imported variables.
print('%s and %s' % (printstuff.x, printstuff.y))
# File printstuff.py ------------------------------------
x, y = 100, 200

