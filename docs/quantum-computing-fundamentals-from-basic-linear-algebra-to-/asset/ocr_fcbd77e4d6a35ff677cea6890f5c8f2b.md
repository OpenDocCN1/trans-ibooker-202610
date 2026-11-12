```
namespace testproject {
    open Microsoft.Quantum.Canon;
    open Microsoft.Quantum.Intrinsic;
    open Microsoft.Quantum.Convert;
    open Microsoft.Quantum.Math;
    open Microsoft.Quantum.Measurement;

    operation QuantumPseudoRandomNumberGenerator() : Result {
        using (q = Qubit()) { // Allocate a qubit.
            H(q); // Put the qubit to superposition. It now has a 50% chance of being 0 or 1.
            return MResetZ(q); // Measure the qubit value.
        }
    }

    operation RandomNumberInRange(max : Int) : Int {
        mutable bits = new Result[0];
        for (idxbit in 1..BitsizeL(max)) {
            set bits += [QuantumPseudoRandomNumberGenerator()];
        }
        let sample = ResultArrayAsInt(bits);
        return sample > max
            ? RandomNumberInRange(max)
            : sample;
    }

    @EntryPoint()
    operation SampleRandomNumber() : Int {
        let max = 50;
        Message($"Sampling a random number between 0 and {max}:");
        return RandomNumberInRange(max);
    }
}
```

FIGURE 16.5 Q# random-number generator

```
open Microsoft.Quantum.Canon;
open Microsoft.Quantum.Intrinsic;
open Microsoft.Quantum.Convert;
open Microsoft.Quantum.Math;
open Microsoft.Quantum.Measurement;
```

FIGURE 16.6 Q# open statements

A brief description of these namespaces is provided here:

+   - Microsoft.Quantum.Canon contains the fundamental items needed in quantum programming.
+   - Microsoft.Quantum.Convert has functions for converting from one Q# data type to another.
+   - Microsoft.Quantum.Math contains mathematical data types and functions.
+   - Microsoft.Quantum.Measurement contains the objects/functions necessary to measure qubits.