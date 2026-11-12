This will then open the editor with your new project, as shown in Figure 17.7.

![](bbox=[0.07, 0.16, 0.69, 0.475])

FIGURE 17.7 New project editor

Selecting the backend will determine much of what you can do. Only some backends are available with the free version. Quantum Inspire has two quantum processors in addition to emulators. There are some restrictions on what you can do with the actual processors. For more details, it is best to refer to the documentation on this issue available at https://www.quantum-inspire.com/kbase/hardware-backends/.

# 17.3 Quantum Error Correction

It is clearly important to have some level of error correction in quantum algorithms. In fact, quantum error correction is a rich field of research. The example shown here is a very simple error correction algorithm. It is not terribly efficient, but it does demonstrate the basic concepts. In this case, we encode one logical qubit with three physical qubits.

The concept is simple. Let us assume you have a single qubit and perform the operation shown in Equation 17.1.

| ψ> = α| 0> + β| 1>
EQUATION 17.1 Quantum Error Basic Equation

How will you detect errors? That is indeed a serious problem. Now our solution here is effective, but inelegant. We will simply use two extra bits; that way, we easily detect if there was a bit-flipping error. So now we use the equation shown in Equation 17.2.