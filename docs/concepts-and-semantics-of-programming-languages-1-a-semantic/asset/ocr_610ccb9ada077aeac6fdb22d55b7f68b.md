```
else if (t[i] == 0) (*vnull)++ ;
   else (*vpos)++ ;
}

void caller () {
   int t[6] = { 1, 1, 0, 0, 0, -2 } ;
   int vneg, vnul, vpos ;
   count (t, 6, &vneg, &vnul, &vpos) ;
   printf ("Negative: %d Null: %d Positive: %d\n", vneg, vnul, vpos) ;
}
```

The caller function transmits the addresses of the three local variables vneg, vnul
and vpos, which will act as “mailboxes” to count. The count function will store the
values returned by its computation in these locations. The computation increments
the values found at the specified addresses (use of * in (*vneg)++, (*vnul)++ and
(*vpos)++). We have chosen not to use return in this function: all results will be
transmitted using the passing by address mechanism.

In example 7.3, the count function uses variables passed by address in write-only
mode. This mode is often known as “out”, indicating that the parameters are the output
of the function. These arguments may also be used in both reading and writing: this is
known as “in-out” mode. In this case, the function begins by reading the values located
at the addresses in order to carry out a computation; it then overwrites them in order to
store the results of the computation. These modes, with a dedicated syntax, for passing
arguments by address are also found in Ada, a language that is briefly presented in
Volume 2, section 2.2. Consider the function in example 7.4, which multiplies a vector
of dimension 2 by a scalar. The resulting vector may be re-written “over” the argument
vector.

# EXAMPLE 7.4.-

```
void mult_vect_scal (float *x, float *y, float n) {
   *x = *x * n ;
   *y = *y * n ;
}
```

The function mult_vect_scal must first retrieve the value of each component of
the vector before multiplying it by the scalar, then writing the result of this product in
the original vector.

Passing arguments by address is also justified when the argument to pass to a
function is large. Instead of copying the entire contents of the structure to transmit to
the called function – which can be costly in terms of both time and memory – we may
simply transmit the address of the structure. However, this strategy is not without its
risks if the contents of the structure must be left as given by the called function: if
this function has access to the memory zone of the caller, there is nothing to prevent it