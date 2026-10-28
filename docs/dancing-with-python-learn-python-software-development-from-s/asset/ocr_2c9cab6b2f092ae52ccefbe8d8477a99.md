```
from qiskit import IBMQ

IBMQ.save_account("TOKEN")
```

Note that save_account expects a string as its argument. To test that you correctly set up your account, enter the Python command:

```
IBMQ.load_account()
```

<AccountProvider for IBMQ(hub='ibm-q', group='open', project='main')>

You should see information about your account provider.

# A.11 The IBM Quantum Composer and Lab

IBM Quantum provides an online drag-and-drop circuit editor and execution environment called the IBM Quantum Composer. You can access it by visiting

https://quantum-computing.ibm.com/composer/

From this web page, you can read the documentation and interactive tutorials, design circuits, run circuits on hardware and in simulators, and visualize the execution results in several ways.

![](bbox=[0.17, 0.49, 0.84, 0.75])

Figure A.9: IBM Quantum Composer