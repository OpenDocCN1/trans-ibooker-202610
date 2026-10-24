# Running the Matplotlib Program as a Script

You can use Python’s script mode to run the Matplotlib program. Save the program shown in Listing 3-1 as prog01.py.

# Listing 3-1. prog01.py

```
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(6)
print(x)
type(x)

y=x+1

plt.plot(x, y, 'o--')
plt.show()
```

When you run this program, the output is shown in a separate window, as shown in Figure 3-7.

![](bbox=[0.185, 0.515, 0.777, 0.865])

Figure 3-7. Output in a separate window