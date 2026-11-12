

/* This is a comment. */
if [x == 3 int +) cos ($v

lexical analysis will recognize the keyword if, the opening bracket, the identifier x, the operator ==, the integer constant 3, the type identifier int, etc. No word in C can contain the character $, so a lexical error will be highlighted when $v is encountered.

Lexical analysis may be seen as a form of “spell check”, in which each recognized word is assigned to a category (keyword, constant, identifier). These words are referred to as tokens.

## 1.2.3.2. Syntactic analysis

Every language follows grammar. For example, in English, a sentence is generally considered to be correctly formed if it contains a subject, verb and complement in an understandable order. Programming languages are no exception: syntactic analysis verifies that the phrases of a source file conform with the grammar of their language. For example, in C, the keyword if must be followed by a bracketed expression, an instruction must end with a semicolon, etc. Clearly, the source text given in example above in the context of lexical analysis does not respect the syntax of C.

Technically, the syntactic analyzer is in charge of the complete grammatical analysis of the source file. It calls the lexical analyzer every time it requires a token to progress through the analyzed source. Syntactic analysis is thus a form of grammar verification, and it also builds a representation of the source file by a data structure, which is often a tree, called the abstract syntax tree (AST). This data structure will be used by all the following phases of compilation, up to the point of execution by an interpreter or the creation of an executable file.

## 1.2.3.3. Semantic analyses

The first two analysis phases of compilation only concern the textual structure of the source. They do not concern the meaning of the program, i.e. its semantics. Source texts that pass the syntactic analysis phase do not always have meaning. The phrase “the sea eats a derivable rabbit” is grammatically correct, but is evidently nonsense.

The best-known semantic analysis is the typing analysis, which prohibits the combination of elements that are incompatible in nature. Thus, in the previous phase, “derivable” could be applicable to a function, but certainly not to a “rabbit”.

Semantic analyses do not reduce to a form of typing analysis but they all interpret the constructs of a program according to the semantics of the chosen language. Semantic analyses may be used to eliminate programs, which leads to execution errors. They may also apply some transformations to program code in order to get an executable file (dependency analysis, closure elimination, etc.). These semantic analyses may be carried out during subsequent passes of source code processing, even after the code generation phase described in the following section.

## 1.2.3.4. Code interpretation/generation

Once the abstract syntax tree (or a derived tree) has been created, there are two options. Either the tree may be executed directly via an *interpreter*, which is a program supplied by the programming language, or the AST is used to generate *object code* files, with the aim of creating an executable file that can run independently. Let us first focus on the second approach. The interpretation mechanism will be discussed later.

Compilation uses the AST generated from the source file to produce a sequence of instructions to be executed either by the CPU or by a virtual machine (VM). The compilation is correct if the execution of this sequence of instructions gives a result, which conforms to the program’s semantics.

Optimization phases may take place during or after object code generation, with the aim of improving its compactness or its execution speed. Modern compilers implement a range of optimizations, which study lies outside the scope of this book. Certain optimizations are “universal”, while others may be specific to the CPU for which the code is generated.

The object code produced by the compiler may be either binary code encoding instructions directly or source text in assembly code. In the latter case, a program – known as the *assembler* – must be called to transform this low-level source code into binary code. Generally speaking, assemblers simply produce a mechanical translation of instructions written mnemonically (mov, add, jmp, etc.) into binary representations. However, certain more sophisticated assemblers may also carry out optimization operations at this level.

Assembling mnemonic code into binary code is a very simple operation, which does not alter the structure of the program. The reference manual of the target CPU provides, for each instruction, the meaning of the bits of the corresponding binary word. For example, the reference manual for the MIPS32® architecture [MIP 13] describes the 32-bit binary format of the instruction ADD rd , rs , rt (with the effect rd ← rs + rt on the registers) as:

| Bit weight | 31 | 26 | 25 | 21 | 20 | 16 | 15 | 11 | 10 | 0 |
|---|---|---|---|---|---|---|---|---|---|---|
| Bit value | 000000 | num.rs | num.rt | num.rd | | | | | | 000000100000 |

Figure 1.4. Coding the ADD instruction in MIPS32®

Three packets of 6 bits are reserved for encoding the register numbers; the other bits in this word are fixed and encode the instruction. The task of the assembler is to generate such bit patterns according to the instructions encountered in the source code.

## 1.2.3.5. Linking

A single program may be made up of several source files, compiled separately. Once the object code from each source file has been produced, all these codes must be collected into a single executable file. Each object file includes “holes”, indicating unknown information at the moment of production of this object code. It is important to know where to find this missing code, when calling functions defined in a different compilation unit, or where to find variables defined in a location outside of the current unit.

The linker has to gather all the object files and fill all the holes. Evidently, for a set of object files to lead to an executable file, all holes must be filled; so the code of every function called in the source must be available. The linking process also has to integrate the needed code, if it comes from some libraries, whether from the standard language library or a third-party library. There is one final question to answer, concerning the point at which execution should begin. In certain languages (such as C, C++ and Java), the source code must contain one, and only one, special function, often named main, which is called to start the execution. In other languages (such as Python and OCaml), definitions are executed in the order in which they appear, defined by the file ordering during the linking process. Thus, “executing” the definition of a function does not call the function: instead, the “value” of this function is created and stored to be used later when the function is called. This means that programmers have to insert into the source file a call to the function which they consider to be the “starting point” of the execution. This call is usually the final instruction of the last source file processed by the linker.

A simplified illustration of the different transformation passes involved in source code compilation is shown in Figure 1.5.

![](img/c9a176746efdbd36ba74d71dfdc86e86_28_0.png)

**Figure 1.5. Compilation process**

## 1.2.3.6. Interpretation and virtual machines

As we have seen, informally speaking, an interpreter “executes” a program directly from the AST. Furthermore, it was said that the code generation process may generate code for a virtual machine. In reality, interpreters rarely work directly on the tree; compilation to a virtual machine is often carried out as an intermediate stage. A virtual machine (VM) may be seen as a pseudo-microprocessor, with one or more stacks, registers and fairly high-level instructions. The code for a VM is often referred to as bytecode. In this case, compilation does not generate a file directly executable by the CPU. Execution is carried out by the virtual machine interpreter, a program supplied by the programming language environment. So, the difference between interpretation and compilation is not clear-cut.

There are several advantages of using a VM: the compiler no longer needs to take the specificities of the CPU into account, the code is often more compact and portability is higher. As long as the executable file for the virtual machine interpreter is available on a computer, it will be possible to generate a binary file for the computer in question. The drawback to this approach is that the programs obtained in this way are often slower than programs compiled as “native” machine code.

