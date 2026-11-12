Step 4. If gcd (a,N) = 1, you used the quantum period subroutine to find r. The quantum period function substeps are given here:
Quantum Step 1: Initialize the registers.
Quantum Step 2: Construct the f(x) quantum function and apply it to the state.
Quantum Step 3: Apply the inverse quantum Fourier transform to the input register.
Quantum Step 4: Measure.
Quantum Step 5: Perform classical continued fraction expansion to find approximations.
Quantum Step 6: Check whether you have found the period; if you have, you are done.
Quantum Step 7: Otherwise, obtain more candidates for r.

+   Step 5. If r is odd, go back to step 1.

+   Step 6. If a^(r/2) = -1(mod N), then go back to step 1.

+   Step 7. Otherwise, you have found nontrivial factors of N and you are done.

As should be clear to you at this point, we don’t have a stable working quantum computer that can avoid decoherence long enough to factor large integers, such as would be found in RSA algorithms used in practice. However, there have been multiple demonstrations of Shor’s algorithm with smaller integers.

# 10.7 Grover's Algorithm

Grover's algorithm is essentially a search algorithm. It was developed by Lov Grover in 1996. There are certainly other descriptions of it that we will explore a bit later in this section, but for the time being, you can view it as a search algorithm. To explain how the algorithm works, consider some unsorted data store that has N entries. Given N elements, the worst-case scenario for classical queries is that it will take n queries. The average case is N / 2. Grover's algorithm works in √N, which is a substantial improvement over classical methods.

Now to work with Grover's algorithm on our N element data store, we must perform a bit of initial setup. There must be an N-dimension state space, which is provided by log2N qubits. We will call this state space H. Now we number each entry in the data store from 0 to N – 1, much like any standard array in programming. Then we have to select some observable that acts on the state space H. Many sources denote this observable with the symbol Ω. This observable must have N eigenvalues, all of which are known. Each eigenstate of Ω encodes one of the entries in the database. The eigenstates are denoted using the bra-ket notation:

{|0>, |1>, |2>, ..., |N – 1>}

And, of course, the eigenvalues are denoted in much the same way:

{λ0, λ1, λ2, ..., λN-1.}