# ## 17.1.2 Commands

QASM does have a number of commands for various tasks. In this section, we examine some of the more common commands you will need to use. Table 17.2 describes the commands you will need to know.

| Command | Definition |
| :--- | :--- |
| display | The display outputs writes the current state of the qubit register. |
| error_model depolarizing_channel, 0.001 | The depolarizing channel error model causes a random error between each operation on a qubit. |
| display_binary | display_binary obtains the contents of the binary register. |
| number of shots | Algorithms can be deterministic or nondeterministic. If they are deterministic, they are single shot (number of shots (N=1)) or multi-shot (number of shots (N>1)). |

With these instructions and commands, you will be able to follow along with the code presented in this chapter. In fact, much of it may be almost obvious to you. That is the power of QASM.

# ## 17.2 Getting Started with QASM

Once you set up your free Quantum Inspire account at https://www.quantum-inspire.com, you can start new projects. Figure 17.6 shows the first step.

![](bbox=[0.113, 0.59, 0.48, 0.92])

**FIGURE 17.6** Starting a new project