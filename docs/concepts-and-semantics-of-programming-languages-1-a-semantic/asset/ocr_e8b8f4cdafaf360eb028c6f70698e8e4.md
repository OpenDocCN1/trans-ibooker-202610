```
elif isinstance (ty, T_var) :
        try :
            return list_assoc (ty.vname, fresh_assoc)
        except Not_found : return ty
        else : raise ValueError
return copy ( body)
```

The function specialize, used to obtain an instance of a type scheme, creates an association list that binds each generalized variable of the scheme to a fresh variable. It then follows a recursive path, copying the structure of the body of the scheme and replacing each generalized variable with its corresponding fresh version (otherwise, the non-generalized variable is retained). The implementation in OCaml uses the function List.assoc from the standard library, which searches through an association list for the value associated with a key, and, if none is found, the exception Not_found is raised. This function does not exist in Python, so we define it under the name list_assoc.

As the environment contains type schemes, we also need to be able to apply a substitution to a scheme and to an environment.

DEFINITION 5.8.– Substitution applied to a type scheme: take a type scheme \(\sigma = \forall \alpha_1 \dots . \alpha_n . \tau\) and a substitution \(\theta\) such that:

\((dom(\theta) \cup \mathcal{F}(\theta(\tau))) \cap \{ \alpha_1, \dots , \alpha_n \} = \emptyset\)

we define \(\theta(\sigma) = \forall \alpha_1 \dots . \alpha_n . \theta(\tau)\).

This definition notably prohibits substitution of the generalized variables of a type scheme. In the following implementation, we do not verify that the variables from the substitution domain and codomain are not present in the scheme. This choice has no effect on the correctness of the inference algorithm, which only establishes equality constraints between types. The types here never include generalized variables, as they are produced by the instantiation operation that replaces generalized variables with fresh variables. Thus, the constructed substitutions will never contain generalized variables.

NOTE.– A more general definition of the application of a substitution \(\theta\) to a type scheme \(\sigma\), with no hypotheses concerning \(\sigma\) and \(\theta\), is possible. This is obtained by renaming the generalized variables \(\alpha_1 \dots . \alpha_n\) of \(\sigma\) with variables found neither in the domain of \(\theta\) nor in \(\theta(\tau)\).

Applying a substitution to an environment comes down to creating an environment in which the bodies of the schemes involved in the bindings have been transformed by the substitution.