of function \( f \), the value associated with \( x_2 \) was an integer, 3, and the application of the operator \( + \) in the body of \( f \) does not pose any problem in this case. Function \( f \) was thus “correctly typed” during definition.

Using the lexical scope mechanism, the values associated with the free occurrences of variables in a function body are determined by the environment in which the function is defined. Thus, in the body of \( f \) in the previous example, the value of \( x_2 \) will always be 3 and expression [3.4] will always be evaluated as 5. The lexical scope mechanism will be used throughout the remainder of the chapter.

# 3.3. Execution semantics: operational semantics

Operational semantics describes the specification of a language’s compiler through a set of inference rules as:

\( (R) \frac{h_1 \cdots h_n}{c} (I) \)

This rule, named \( R \), means that, if hypotheses \( h_1, \cdots, h_n \) are true, we can deduce \( c \), or, in an equivalent form, that \( c \) can be deduced by establishing \( h_1, \cdots, h_n \). \( I \) corresponds to additional information (condition for application of the rule, or intermediate computation). For example, consider a rule specifying the evaluation of the addition of two expressions:

\( (R) \frac{\text{Env} \vdash e_1 \rightsquigarrow v_1 \quad \text{Env} \vdash e_2 \rightsquigarrow v_2}{\text{Env} \vdash e_1 + e_2 \rightsquigarrow v} (v = v_1 + v_2) \)

In this case, \( \text{Env} \vdash e \rightsquigarrow v \) expresses the fact that, in environment Env, the evaluation of expression \( e \) returns a value \( v \). The order of evaluation of \( e_1 \) and \( e_2 \) is not specified, and the two rules below leave the choice of the error propagation to the (developer of the) compiler:

\( (R_1) \frac{\text{Env} \vdash e_1 \rightsquigarrow v_1}{\text{Env} \vdash e_1 + e_2 \rightsquigarrow v_1} (v_1 \in \text{IE}) \quad (R_2) \frac{\text{Env} \vdash e_2 \rightsquigarrow v_2}{\text{Env} \vdash e_1 + e_2 \rightsquigarrow v_2} (v_2 \in \text{IE}) \)

This is an evaluation relation, not a function. Function \( [[\_]] \), defined in section 3.2, implements rule \( R_1 \).

Function eval12 presented in section 3.2 defines a very precise evaluation strategy, establishing an order for evaluating subexpressions. This function therefore provides a fully deterministic definition of the evaluation.

Function \( [[\_]] \), on the other hand, leaves certain choices open (for example the order in which the subexpressions of an application are to be evaluated) while fixing others (such as the order of evaluation of the subexpressions of an operator).