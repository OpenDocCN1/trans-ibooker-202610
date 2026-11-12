
You also have the option of using the Quantum Development Kit online for free at https://www.microsoft.com/en-us/quantum/development-kit. The Binder option is the free version. When you launch it, you will immediately see samples, as shown in Figure 16.4. This is a great way to get started.

![](img/49061be7fe11d9f7f3e56114ab15acdc_318_0.png)

**FIGURE 16.4 QDK Samples**

Let us begin by examining a random-number generator done in Visual Studio Code. Random-number generators are common tutorial code samples for Q#. You can see the entire code in Figure 16.5. We will then break it down by explaining it.

We will assume you have no programming background and explain all of it. The first line states the namespace and then has the name of your project (whatever you selected when you created the project). A namespace allows you to group related classes together.

Next, you see several open statements, as shown in Figure 16.6.

```qsharp
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
        for (idxbit in 1..BitsToSize(max)) {
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

**FIGURE 16.5 Q# random-number generator**

```qsharp
open Microsoft.Quantum.Canon;
open Microsoft.Quantum.Intrinsic;
open Microsoft.Quantum.Convert;
open Microsoft.Quantum.Math;
open Microsoft.Quantum.Measurement;
```

**FIGURE 16.6 Q# open statements**

A brief description of these namespaces is provided here:

- Microsoft.Quantum.Canon contains the fundamental items needed in quantum programming.
- Microsoft.Quantum.Convert has functions for converting from one Q# data type to another.
- Microsoft.Quantum.Math contains mathematical data types and functions.
- Microsoft.Quantum.Measurement contains the objects/functions necessary to measure qubits.

What this is doing is opening some of the namespaces that are part of the Microsoft Quantum Development Kit. These namespaces include classes/objects you can use.

After the comments, you see the first operation, as shown in Figure 16.7.

```qsharp
operation QuantumPseudoRandomNumberGenerator() : Result {
    using (q = Qubit()) { // Allocate a qubit.
        H(q); // Put the qubit to superposition. It now has a 50% chance of being 0 or 1.
        return MResetZ(q); // Measure the qubit value.
    }
}
```

**FIGURE 16.7 Operation QuantumPseudoRandomNumberGenerator**

The first thing to note is this operation does not take in any parameters. It does implement a using statement, which we discussed earlier in this chapter. It is using a single instance of the Qubit class we simply called q.

The next thing you see is some text preceded by //, and it appears in green in Visual Studio or Visual Studio Code. This is called a comment. Comments are text that will be ignored by Visual Studio but are there to provide information to whoever is reading them. It is a good idea to comment on your code to indicate what it is you are trying to accomplish. Also, when you are new to programming, or new to a particular language such as Q#, a good tip for learning it is to get sample code and heavily comment it. Go through all the sample code, commenting every line. This can help you to understand every line. If you don’t understand a given part of the code, put a comment stating as much. Then make it a point to go back and seek out answers to all the code you don’t yet understand.

The function QuantumPseudoRandomNumberGenerator is actually called from the function RandomNumberInRange. Therefore, we should examine that operation next (note we are using the terms function and operation interchangeably). You can see this function in Figure 16.8.

```qsharp
operation RandomNumberInRange(max : Int) : Int {
    mutable bits = new Result[0];
    for (idxBit in 1..BitsToSize(max)) {
        set bits += [QuantumPseudoRandomNumberGenerator()];
    }
    let sample = ResultArrayAsInt(bits);
    return sample > max ?
        RandomNumberInRange(max)
        : sample;
}
```

**FIGURE 16.8 Operation RandomNumberInRange**

This operation does have a single parameter. It is an integer we name max. That is the maximal range. We then create an array of type Result that we call bits. Notice that it is preceded by the keyword mutable, indicating that it is indeed mutable/changeable.

We next have a for loop (also discussed earlier in this chapter). This for loop will add to the result array bits, the value retrieved from QuantumPseudoRandomNumberGenerator. Now we can more clearly see the purpose of the algorithm. It is to return a bit. That bit is then put in an array to create a set of random bits.

Then we come to the function `SampleRandomNumber`, as shown in Figure 16.9.

```qsharp
@EntryPoint()
operation SampleRandomNumber() : Int {
    let max = 50;
    Message($"Sampling a random number between 0 and {max}: ");
    return RandomNumberInRange(max);
}
```

**FIGURE 16.9 Operation SampleRandomNumber**

This operation is preceded by the `@EntryPoint()` directive, which tells us that this is the starting point for this program. It has only two things it does. The first is to set a `max` (in this case, 50). The next is to call the function `RandomNumberInRange`, passing it the value `max`.

It is recommended that you write this program yourself and then execute it. It is common to have some configuration issues with whatever tool you are using (Visual Studio 2017/2019, Visual Studio Code, or the online Quantum Development Kit). It is best to get those issues sorted out now before you move on to more complex programs. Also, once you get this program to execute as written, experiment with changing a single line (perhaps start with the `max` value) to see what happens. You should take the time to become very comfortable with this simple program before you move forward into more complex examples.

## 16.3 Grover's Algorithm

You may wish to review the presentation of Grover's algorithm from Chapter 10, “Quantum Algorithms.” However, a brief review is presented before we delve into the code.

### 16.3.1 Grover's Algorithm Reviewed

Recall that Grover's algorithm is a search algorithm. There must be an N-dimension state space, which we will call H, that is provided by log2N qubits. Then the entries in the data store are numbered from 0 to N-1. Then there is an observable, Ω, that acts on the state space H. Ω must have eigenvalues, all of which are known. Each eigenstate of Ω encodes one of the entries in the database. The eigenstates are denoted using bra-ket notation:

{ |0>, |1>, |2>, ..., |N-1> }

And, of course, the eigenvalues are denoted in much the same way:

{ λ0, λ1, λ2, ..., λN-1 }.

The next step is to determine some operator that functions to compare database entries based on some criterion. Grover's algorithm does not specify the operator or its criteria. That will be selected based on the particular data store and search; however, the operator must be a quantum subroutine that functions via a superposition of states. The operator is often designated as Uω.

Our operator has the following property when applied to the previously mentioned eigenstates (which we will denote as |ω>):

Uω|ω> = -|ω>

However, for all x that are not ω, we want the following:

Uω|x> = |x>

Put another way, we are trying to identify the particular eigenstate |ω> that Uω acts on in a manner different from the other eigenstates. This operator, Uω, is often referred to as a quantum oracle. It is some unitary operator that operates on two qubits. That defines the Uω operator. There is a second operator called Us. The s is a superposition of possible states. The operator is as follows:

Us = 1 |s><s| - I

The first step is to initialize the system to a state. That state will be a superposition of states, often denoted as follows:

|s> = 1/√N Σ|x>

We are simply stating that the state |s> is a superposition of all possible states |x>.

The next step is called the Grover iteration, r, and is executed N times. This step r is simply to apply the operator Uω and apply the operator Us.

The third step is to perform the measurement of the observable Ω. This measurement will provide some eigenvalue λω. As we iterate through the process, we eventually get to the proper answer.

This review should help refresh your memory of Grover's algorithm. In the next section, we look at code that implements a simple version of Grover's algorithm.

## 16.3.2 The Code for Grover's Algorithm

The code will be presented in sections, each described in light of the review of Grover's algorithm. Some of this depends on objects that are part of the Quantum Development Kit. So, the first item to examine is the open statements, as shown in Figure 16.10.

![](img/49061be7fe11d9f7f3e56114ab15acdc_322_0.png)

**FIGURE 16.10 The open statements in Grover's algorithm code**
