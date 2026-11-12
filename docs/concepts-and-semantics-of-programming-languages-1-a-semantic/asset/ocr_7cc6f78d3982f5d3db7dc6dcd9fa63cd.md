If Env \vdash e \rightsquigarrow v has been obtained using rule F15 (respectively, F16), then e = fun x \mapsto e_f and v = \langle x, e_f, \text{Env} \rangle (respectively, e = rec f \ x = e_f and v = \langle f, x, e_f, \text{Env} \rangle) and since F15 (respectively, F16) is the only applicable rule for evaluating a function (respectively, a recursive function), then v = v'.

If Env \vdash e \rightsquigarrow v has been obtained using rule F17 (respectively, F18), then e = (e_1 e_2) and:

Env \vdash e_1 \rightsquigarrow \langle x, e_f, \text{Env}_f \rangle (respectively, Env \vdash e_1 \rightsquigarrow \langle f, x, e_f, \text{Env}_f \rangle)

Env \vdash e_2 \rightsquigarrow v_2

(x, v_2) \oplus \text{Env}_f \vdash e_f \rightsquigarrow v (respectively, (f, \langle f, x, e_f, \text{Env}_f \rangle) \oplus (x, v_2) \oplus \text{Env}_f \vdash e_f \rightsquigarrow v)

By induction hypothesis, if:

Env \vdash e_1 \rightsquigarrow v'_1 \text{ then } v'_1 = \langle x, e_f, \text{Env}_f \rangle (respectively v'_1 = \langle f, x, e_f, \text{Env}_f \rangle)

and if:

Env \vdash e_2 \rightsquigarrow v'_2 \text{ then } v'_2 = v_2

Hence:

(x, v'_2) \oplus \text{Env}_f = (x, v_2) \oplus \text{Env}_f

(respectively (f, v'_1) \oplus (x, v'_2) \oplus \text{Env}_f = (f, \langle f, x, e_f, \text{Env}_f \rangle) \oplus (x, v_2) \oplus \text{Env}_f)

The only applicable rule is F17 (respectively, F18) and, by induction hypothesis, since:

(x, v_2) \oplus \text{Env}_f \vdash e_f \rightsquigarrow v

(respectively, (f, \langle f, x, e_f, \text{Env}_f \rangle) \oplus (x, v_2) \oplus \text{Env}_f \vdash e_f \rightsquigarrow v)

we obtain v = v'.

If Env \vdash e \rightsquigarrow v has been obtained using rule F19, then e = (e_1 e_2), Env \vdash e_1 \rightsquigarrow v_1, Env \vdash e_2 \rightsquigarrow v_2, v_1 \in \text{IF}, v_2 \in \text{IE} and v = v_2. By induction hypothesis, if Env \vdash e_1 \rightsquigarrow v'_1, then v'_1 = v_1 \in \text{IF} and if Env \vdash e_2 \rightsquigarrow v'_2, then v'_2 = v_2 \in \text{IE}. Rule F19 is the only applicable rule for evaluating e and we obtain v' = v'_2 = v_2 = v.

If Env \vdash e \rightsquigarrow v has been obtained using rule F20, then e = (e_1 e_2), Env \vdash e_1 \rightsquigarrow v_1, v_1 \notin \text{IF} and v = \text{Err}_T. By induction hypothesis, if Env \vdash e_1 \rightsquigarrow v'_1, then v'_1 = v_1 \notin \text{IF}. Rule F20 is the only applicable rule for evaluating e and we obtain v' = v.

The proposition below establishes an equivalence between the semantics defined by the evaluation function [[\_]] and the operational semantics with call-by-value defined in Box 3.2 and Box 3.3.