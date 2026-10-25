```
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
```