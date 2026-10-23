# Rules

In the following sections, we will give a short overview of the main rules that strongly apply to the implementation of cryptographic algorithms and security schemes using C++20. It is best to follow these rules. Note that we will examine only six out the 10 rules. All the explanations and examples are provided within the guide [19].

For some rules, there are also rules from the C programming language that apply to C++. The following rules can be used within the procedure explained in the Secure Coding Guidelines in Table 7-1.

The duty of any information security officer, security analyst, or ethical hacker is to improve the code by following such a checklist. Further, the checklist also can be used by developers as a guide when they are developing critical cryptographic algorithms.

It is recommended to do a code review on the sections of the algorithm that are quite vulnerable and to make sure that the rules (Rules 1 through 7) are followed as much as possible (see Tables 7-4 through 7-9).

Following these rules will give you as a security analyst or ethical hacker a certain level of trust that the security mechanisms (cryptographic algorithms, security protocols, security schemes, and other cryptographic primitives) have been implemented properly and common vulnerabilities have been eliminated.

# Rule 1 - Declarations and Initializations (DCL)

Table 7-4. Rule 1 - Declarations and Initializations [19]

| Rule | Title |
|------|-------|
| DCL50-CPP | Do not define a C-style variadic function. |
| DCL51-CPP | Do not declare or define a reserved identifier. |
| DCL52-CPP | Never qualify a reference type with const or volatile. |
| DCL53-CPP | Do not write syntactically ambiguous declarations. |
| DCL54-CPP | Overload allocation and deallocation functions as a pair in the same scope. |
| DCL55-CPP | Avoid information leakage when passing a class object across a trust boundary. |
| DCL56-CPP | Avoid cycles during initialization of static objects. |
| DCL57-CPP | Do not let exceptions escape from destructors or deallocation functions. |