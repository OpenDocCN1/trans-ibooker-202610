# 9.8 Universality for quantum gates

The existence of a universal set of gates for quantum computing is mostly academic for a coding book, but it is worth a short discussion to complement the classical situation.

Unlike the classical case, there is an infinite number of 1-qubit quantum gates. The RX_φ, RY_φ, and RZ_φ give us such a set, where 0 ≤ φ < 2π. If we can’t generate these precisely, we need to approximate them as closely as we wish. We modify our definition to say that a set of quantum gates is universal if we can create a circuit with those gates and approximate any n-qubit quantum gate to arbitrary precision. [TCN]

We need qubit entanglement, which does not have a classical counterpart.

Finally, below the elegant gates like X, Y, Z, H, and CNOT, we need to understand which gates engineers implemented in the hardware in any given quantum computing technology. These vary across “superconducting transmon,” “ion trap,” “quantum dot,” and “photonic” qubit approaches, for example, and may vary within each category. [DRQ] Can we approximately generate any gate we wish from the hardware gates?

Researchers have discovered several universal sets of quantum gates including

+   all 1-qubit gates plus CNOT,
+   Toffoli CCX, Hadamard H, and S

and

+   CNOT, Hadamard H, and T.

In practice, the set of gates available through Qiskit or that you can build yourself with circuits is large. We focus on them.

# 9.9 Summary

This chapter explored the gate and circuit foundation of classical computing. It showed how low-level coders could build bit addition and higher-level functions. From there, we moved to quantum gates and circuits. While we used the same words, the programming models are fundamentally different. In particular, superposition and entanglement give us behavior and tools that are not available using classical techniques alone.

In the next chapter, we look at techniques to test and optimize your code, be it classical or quantum. We saw some of this already when we used Qiskit’s transpile function to optimize quantum gates for general and hardware-specific use.