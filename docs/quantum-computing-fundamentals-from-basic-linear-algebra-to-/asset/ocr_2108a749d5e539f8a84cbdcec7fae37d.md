# # 17.5 Deutsch-Jozsa Algorithm

This is another algorithm from Chapter 10. We will briefly review it here and then work through code that implements this algorithm.

# ## 17.5.1 Deutsch-Jozsa Algorithm Reviewed

Recall from Chapter 10 that Deutsch-Jozsa is a modification of the Deutsch algorithm. The issue is actually rather straightforward. The Deutsch algorithm looks at functions of one variable. The Deutsch-Jozsa algorithm generalizes this to functions of n variables. Recall this problem involves a black-box quantum computer that implements some function that takes in n-digit binary values and produces either a 0 or 1 for the output of each such value. The output will either be balanced (an equal number of 1s and 0s) or constant (either all 1s or all 0s). The goal of the algorithm is to determine if the function is constant or balanced.

The actual algorithm begins with the first n bits in state |0>, and the final bit is in the state |1>. Then a Hadamard transform is applied to each bit. Now we have a function, which we will call f(x), that maps the state |x>|y> to |x>|y ⊕ f(x)>. In this case, the symbol ⊕ denotes addition modulo 2. As the values of x are put through the oracle f(x), either a 1 or 0 is produced. The output will be 0 if f(x) is balanced and 1 if f(x) is constant. For n bits input, the final output will be n 0s if f(x) is constant. Any other output (any combination of 0s and 1s, or all 1s) indicates that f(x) is balanced.

# ## 17.5.2 The Code for the Deutsch-Jozsa Algorithm

We begin with the namespace opening and the open statements for other namespaces, as shown here:

```
version 1.0

qubits 2
# The Deutsch-Jozsa algorithm an oracle is used to
# determine if a given binary function f(x) is constant or
# balanced.
# Constant f(x)=fc1=0 OR f(x)=fc2=1
# Balanced f(x)=fb3=x OR f(x)=fb4=NOT(x)
# The algorithm requires only a single query of that
# function f(x).

# Initialize qubits in |+> and |-> statew
.initialize
prep_z q[0:1]
X q[1]
{H q[0]|H q[1]}
```