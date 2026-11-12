Let us summarize the steps. First, we have the setup process that both Alice and Bob complete:

+   1. A prime of the form \(E[L^c A^a] * E[L^{cb}] * f \pm 1\).
+   2. A super singular elliptic curve E over \(fp^2\) (a finite field).
+   3. Fixed elliptic points on the curve E, those points being \(P_A, Q_A, P_B, Q_B\) (remember P and Q as points in an elliptic curve).
+   4. The order of \(P_A\) and \(Q_A\) is \((lA)^{eA}\). The order of \(P_B\) and \(Q_B\) is \((lB)^{eB}\).

This will allow both Alice and Bob to have the necessary material for the key exchange. Next, we have a series of steps in the key exchange. Some are performed by Alice and some by Bob.

Alice generates two random integers: \(m_A, n_A < (lA)^{eA}\). Alice generates \(R_A := m_A * (P_A) + n_A (Q_A)\). Recall from Chapter 2, "Complex Numbers," that the symbol := means "is defined to be." Alice uses the point \(R_A\), which was created in the preceding step, to create an isogeny mapping \(E \rightarrow E_A\), which we will call \(\phi_A\), and curve \(E_A\), which is isogenous to the curve E. Alice now applies the mapping, \(\phi_A\), that was just created to \(P_B\) and \(Q_B\) (remember Bob created these) to form two points on the curve \(E_A\). Those points are \(\phi_A P_B\) and \(\phi_A Q_B\). Alice sends three things to Bob: \(E_A, \phi_A P_B, \text{and } \phi_A Q_B\). Now it is time for Bob to perform his steps so that the two parties have a shared secret key. Bob will essentially do the same things Alice did; only the subscripts will be the reverse of what Alice used:

+   1. Bob generates two random integers: \(m_B, n_B (lB)^{eB}\).
+   2. Bob generates \(R_B := m_B * (P_B) + n_B (Q_B)\).
+   3. Bob uses the point \(R_B\), which was created in the preceding step, to create an isogeny mapping \(E \rightarrow E_B\), which we will call \(\phi_B_A\), and curve \(E_B\), which is isogenous to the curve E.
+   4. Bob now applies the mapping, \(\phi_b\), that was just created to \(P_A\) and \(Q_A\) (remember Alice created these) to form two points on the curve \(E_B\). Those points are \(\phi_B P_A\) and \(\phi_B Q_A\).
+   5. Bob now sends three things to Alice: \(E_B, \phi_A P_B, \text{and } \phi_B Q_A\).

Now both Alice and Bob have exchanged materials, so they can create a shared secret key.

Alice can use the parts she received from Bob as well as items she generated, as follows:

+   1. \(m_A (\phi_B (P_A)) + n_A (\phi_B (Q_A)) = S_{BA}\).
+   2. Alice uses \(S_{BA}\) to create an isogeny mapping, which we will call \(\psi_{BA}\).
+   3. Alice uses \(\psi_{BA}\) to create an elliptic curve, \(E_{BA}\), which is isogenous to E.
+   4. Alice computes the j-invariant of the curve \(E_{BA}\). We can call this j-invariant K (you will see why in just a moment).