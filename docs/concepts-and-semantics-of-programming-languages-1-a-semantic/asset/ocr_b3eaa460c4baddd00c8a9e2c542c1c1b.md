# Construction of tree $\nabla_2$:

$(F_1) \overline{\text{Env}_0 \vdash 2 \rightsquigarrow 2}$
$(F'_{11}) \overline{\text{Env}_3 \vdash x \rightsquigarrow 2} (F_1) \overline{\text{Env}_3 \vdash 1 \rightsquigarrow 1}$
$(F_4) \overline{\text{Env}_3 \vdash x - 1 \rightsquigarrow 1}$
$(F'_{11}) \dots (F_1) \overline{\text{Env}_5 \vdash 0 \rightsquigarrow 0}$
$(F_4) \dots \overline{\text{Env}_5 \vdash z = 0 \rightsquigarrow false} \nabla_3$
$(F_8) \overline{(k, [f, \text{Env}_3]) \oplus \text{Env}_4 \vdash e_4 \rightsquigarrow 4} \text{Env}_5$

# Construction of tree $\nabla_3$:

$(F'_{15}) \overline{\text{Env}_2 \vdash e_2 \rightsquigarrow F_g}$
$(F'_{11}) \overline{\text{Env}_5 \vdash g \rightsquigarrow F_g} (F'_{14}) \overline{(z, [z - 1, \text{Env}_5]) \oplus \text{Env}_g \vdash e_3 \rightsquigarrow F'_k}$
$(F'_{16}) \dots \nabla_4$
$(F'_{16}) \dots$
$\text{Env}_5 \vdash e_6 \rightsquigarrow 4$
with $F'_k = \langle k, e_4, \text{Env}_6 \rangle$.

# Construction of tree $\nabla_4$:

$(F'_{14}) \overline{\text{Env}_1 \vdash e_1 \rightsquigarrow \langle y, y + x, \text{Env}_1 \rangle} F_p$
$(F'_{11}) \overline{\text{Env}_3 \vdash f \rightsquigarrow F_p}$
$(F'_{11}) \overline{\text{Env}_5 \vdash k \rightsquigarrow F_p}$
$(F'_{11}) \overline{\text{Env}_7 \vdash k \rightsquigarrow F_p} \nabla_6$
$(F'_{16}) \overline{\text{Env}_7 \vdash (k x) \rightsquigarrow 4} \nabla_5$
$(F_7) \dots$
$(k, [k, \text{Env}_5]) \oplus \text{Env}_6 \vdash e_4 \rightsquigarrow 4 \text{Env}_7$

# Construction of tree $\nabla_5$:

$(F_1) \overline{\text{Env}_0 \vdash 2 \rightsquigarrow 2}$
$(F'_{11}) \overline{\text{Env}_3 \vdash x \rightsquigarrow 2} (F_1) \overline{\text{Env}_3 \vdash 1 \rightsquigarrow 1}$
$(F_4) \overline{\text{Env}_3 \vdash x - 1 \rightsquigarrow 1}$
$(F'_{11}) \dots (F_1) \overline{\text{Env}_5 \vdash 1 \rightsquigarrow 1}$
$(F_4) \dots \overline{\text{Env}_5 \vdash z - 1 \rightsquigarrow 0}$
$(F'_{11}) \dots \overline{\text{Env}_7 \vdash z \rightsquigarrow 0} (F_1) \overline{\text{Env}_7 \vdash 0 \rightsquigarrow 0}$
$(F_4) \dots \overline{\text{Env}_7 \vdash z = 0 \rightsquigarrow true}$