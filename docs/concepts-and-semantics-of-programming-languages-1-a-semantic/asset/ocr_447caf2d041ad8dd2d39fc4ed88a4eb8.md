+   1) fetching the next instruction to be executed from the memory: every microprocessor contains a special register, the Program Counter (PC), which records the location (address) of this instruction. The PC is then incremented, i.e. the size of the fetched instruction is added to it;

+   2) decoding of the fetched instruction;

+   3) execution of this instruction.

However, the next instruction is not always the one located next to the current instruction. Consider the function min in example 1.1, written in C, which returns the smallest of its two arguments.

# EXAMPLE 1.1.-

```
C
int min (int a, int b) {
if (a < b) return (a) ;
else return (b) ;
}
```

This function may be translated, intuitively and naively, into elementary instructions, by first placing a and b into registers, then comparing them:

```
min:
load	a, reg0
load	b, reg1
compare	reg0, reg1
```

Depending on the result of the test – true or false – different continuations are considered. Execution continues using instructions for one or the other of these continuations: we therefore have two possible control paths. In this case, a conditional jump instruction must be used to modify the PC value, when required, to select the first instruction of one of the two possible paths.

```
branchgt	a_gt_b
load	reg0, reg2
jump	end
a_gt_b:
load	reg1, reg2
end:
return	reg2
```

The branchgt instruction loads the location of the instruction at label a_gt_b into the PC. If the result of the compare instruction is that reg0 > reg1, the next instruction is the one found at this address: load reg1, reg2. Otherwise, the next instruction is the one following branchgt: load reg0, reg2. This is followed by the unconditional