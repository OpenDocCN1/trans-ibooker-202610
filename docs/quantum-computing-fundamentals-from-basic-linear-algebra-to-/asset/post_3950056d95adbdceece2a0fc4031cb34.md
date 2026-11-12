
You might notice that there are more here than were in the random-number generator discussed earlier in this chapter. Specifically, there are two that were not in the previous code:

```
open Microsoft.Quantum.Intrinsic;
open Microsoft.Quantum.Arrays;
```

These are included because they contain specific classes we will need in implementing Grover's algorithm. The next part of the code is the function ReflectMarked. As you can see in Figure 16.11, it is preceded by comments, which are given to help explain what the function is doing.

![](img/49061be7fe11d9f7f3e56114ab15acdc_323_0.png)

### FIGURE 16.11 ReflectMarked

This function takes in a Qubit array we named InQubits. The message portion is not strictly necessary, but it is a good idea to tell the user what is happening. Notice we have X and H. Those are quantum gates that are part of the Q# Quantum Development Kit. This is one of the reasons Quantum Development Kit is so useful. The various gates you need are already built in.

H is the Hadamard gate and is part of the Microsoft.Quantum.Intrinsic namespace. There are other gates included in this namespace. Some that you will see frequently are shown in Table 16.3.

### TABLE 16.3 Operations in Microsoft.Quantum.Intrinsic

| X, Y, and Z | Pauli-X, -Y, and -Z gates. |
|---|---|
| T | The T gate. |
| Rx, Ry, and Rz | These are rotations about the x-, y-, and z-axes. |
| R | This applies rotation about a given Pauli axis. |
| M | This performs a measurement of a single qubit in the Pauli-Z basis. |
| Measure | Performs a joint measurement of one or more qubits in the specified Pauli bases. |
| CNOT | This applies a controlled (not) gate. |

We also see something new—the ApplyToEach operation. This is part of the Microsoft.Quantum.Canon namespace. It applies a single qubit operation to each element in a given register.

Then we move on to the next function in the code, ReflectUniform, shown in Figure 16.12.

```
// Reflects about the superposition state.
operation ReflectUniform(InQubits : Qubit[]) : Unit {
    within {
        // Transform the uniform superposition to all-zero.
        Adjoint PermuteUniformZeros(InQubits);
        // Transform the all-zero state to all-ones
        SetAllOnes(InQubits);
    } apply {
        // Now that we've transformed the uniform superposition to the
        // all-ones state, reflect about the all-ones state, then leave
        // the within/apply block transform us back.
        ReflectAboutAllOnes(InQubits);
    }
}
```

### FIGURE 16.12 ReflectUniform

You can see that this calls two functions: SetAllOnes and ReflectAboutAllOnes. Both of these are passed an array, InQubits. We also see something called Adjoint. This is a functor. Functors (not functions) are something specific to Q#. To quote from Microsoft:

> “Functors are factories that allow to access particular specialization implementations of a callable. Q# currently supports two functors; the Adjoint and the Controlled functor, both of which can be applied to operations that provide the necessary specialization(s).”²

The Adjoint functor defines a unitary transformation of the quantum state. This is often represented as U (unitary transformation).

There are three more functions in the preparation of the data to search, and we have seen these called in previous code segments. These functions are shown in Figure 16.13.

```
// Reflects about the all-ones state.
operation ReflectAboutAllOnes(InQubits : Qubit[]) : Unit {
    Controlled Z(Most(InQubits), Tail(InQubits));
}

operation PermuteUniformZeros(InQubits : Qubit[]) : Unit is Adj + Ctl {
    ApplyToEachCA(H, InQubits);
}

operation SetAllOnes(InQubits : Qubit[]) : Unit is Adj + Ctl {
    ApplyToEachCA(X, InQubits);
}
```

### FIGURE 16.13 Additional functions for Grover's algorithm

Note that ReflectAboutAllOnes simply uses the controlled Z gate. PrepareUniformZeros and SetAllOnes each use the function ApplyToEach. Recall that this function applies a single qubit operation to each element in a given register.

Now we have the function to actually search for input. This is shown in Figure 16.14.

```
@EntryPoint()
operation SearchForMarkedInput(nQubits : Int) : Result[] {
    using (qubits = Qubit[nQubits]) {
        // Initialize a uniform superposition over all possible inputs.
        PrepareUniformSuperposition(qubits);
        // The search itself consists of repeatedly reflecting about the
        // marked state and our start state, which we can write out in Q#
        // as a for loop.
        for (idxIteration in 0..NumberOfIterations(nQubits) - 1) {
            ReflectMarked(qubits);
            ReflectUniform(qubits);
        }

        // Measure and return the answer.
        return ForEach(MResetZ, qubits);
    }
}
```

### FIGURE 16.14 Entry point for Grover's algorithm

Notice this is the entry point for the program, and it should help demonstrate for you how the other functions are used. First, we initialize a uniform superposition, and then we search via reflecting operations. Finally, we measure each qubit.

There is one more function that will simply tell you how many iterations are needed to find a single marked item. This function was called in NumberOfIterations and is shown in Figure 16.15.

```
function NumberOfIterations(nQubits : Int) : Int {
    let items = 1 <<< nQubits; // 2^numQubits
    // compute number of iterations:
    let angle = ArcSin(1. / Sqrt(IntAsDouble(items)));
    let iterations = Round(0.25 * PI() / angle - 0.5);
    return iterations;
}
```

### FIGURE 16.15 The NumberOfIterations function

As with the random-number generator, it is recommended you actually code this and then get it to execute. After you get it executed, work with different variations. Alter small bits of code and see what happens. This will help you to understand the code better.

## 16.4 Deutsch-Jozsa Algorithm

This is another algorithm from Chapter 10. We will briefly review it here and then work through code that implements this algorithm.

### 16.4.1 Deutsch-Jozsa Algorithm Reviewed

Recall from Chapter 10 that this is a modification of the Deutsch algorithm. The issue is actually rather straightforward. The Deutsch algorithm looks at functions of one variable. The Deutsch-Jozsa algorithm generalizes this to functions of n variables. Recall this problem involves a black-box quantum computer that implements some function that takes in n-digit binary values and produces either a 0 or 1 for the output of each such value. The output will either be balanced (an equal number of 1s and 0s) or constant (either all 1s or all 0s). The goal of the algorithm is to determine if the function is constant or balanced.

The actual algorithm begins with the first n bits in state |0>, and the final bit is in the state |1>. Then a Hadamard transform is applied to each bit. Now we have a function we will call f(x). This function maps the state |x>|y> to |x>|y ⊕ f(x)>. In this case, the symbol ⊕ denotes addition modulo 2. As the values of x are put through the oracle f(x), either a 1 or 0 is produced. The output will be 0 if f(x) is balanced and 1 if f(x) is constant. For n bits input, the final output will be n 0s if f(x) is constant. Any other output (any combination of 0s and 1s, or all 1s) indicates that f(x) is balanced.

### 16.4.2 The Code for Deutsch-Jozsa Algorithm

We begin with the namespace opening and the open statements for other namespaces. This is shown in Figure 16.16.

```
namespace DeutschJozsa {
    open Microsoft.Quantum.Intrinsic;
    open Microsoft.Quantum.Canon;
    open Microsoft.Quantum.Arrays;
    open Microsoft.Quantum.Convert;
    open Microsoft.Quantum.Measurement;
}
```

### FIGURE 16.16 Beginning of Deutsch-Jozsa

Let us begin at the entry point, shown in Figure 16.17.

```
[@EntryPoint()]
operation RunDeutschJozsa(nQubits : Int, mElements : Int[]) : Bool {
    return IsConstant(
        BooleanFunction(nQubits, mElements), nQubits
    );
}
```

### FIGURE 16.17 Deutsch-Jozsa entry point
