# 22.2 Maxwell’s Equations

Let $(x, y, z, t)$ be standard coordinates on $\mathbb{R}_0^3 \times \mathbb{R}$. In what follows, we think of $x, y, z$ as “space” variables and $t$ as a “time” variable. Using the differential operators in Section 10.6, an **electromagnetic field** in $\mathbb{R}_0^3 \times \mathbb{R}$ is characterized by **Maxwell’s equations**:

[M1] $\operatorname{div}(E) = \frac{1}{\epsilon_0} \rho$
[M2] $\operatorname{div}(B) = 0$
[M3] $\operatorname{curl}(E) = -\frac{\partial B}{\partial t}$
[M4] $\operatorname{curl}(B) = \mu_0 J + \mu_0 \epsilon_0 \frac{\partial E}{\partial t}$,

where the vector fields $E, B, J$ in $\mathfrak{X}(\mathbb{R}_0^3 \times \mathbb{R})$ are the **electrical field**, **magnetic field**, and **current density**, respectively; the function $\rho$ in $C^\infty(\mathbb{R}_0^3 \times \mathbb{R})$ is the **charge density**; and the real numbers $\epsilon_0$ and $\mu_0$ are the **permittivity of free space** and the **permeability of free space**, respectively. We interpret the operators div and curl in the above identities as applying only to the space variables, and likewise for other vector calculus operators to follow. Let us assume units have been chosen so that $c = 1$, where $c = \mu_0 \epsilon_0$ is the speed of light. Thus, [M4] becomes

[M4'] $\operatorname{curl}(B) = \mu_0 J + \frac{\partial E}{\partial t} = \frac{1}{\epsilon_0} J + \frac{\partial E}{\partial t}$.

We observe that $J$ and $\rho$ are not independent of each other:

$0 = \operatorname{div}(\operatorname{curl}(B))$ [Th 10.6.1(d)]
$= \operatorname{div}\left(\frac{\partial E}{\partial t} + \mu_0 J\right)$ [M4']
$= \frac{\partial(\operatorname{div}(E))}{\partial t} + \mu_0 \operatorname{div}(J)$
$= \frac{1}{\epsilon_0} \frac{\partial \rho}{\partial t} + \mu_0 \operatorname{div}(J)$. [M1]

Then $\mu_0 \epsilon_0 = 1$ gives

$\operatorname{div}(J) + \frac{\partial \rho}{\partial t} = 0,$ (22.2.1)

which is called the **continuity equation**.

The rest of this section is devoted to finding alternative expressions for Maxwell’s equations, especially in terms of differential forms. This is an opportunity to showcase some of the computational tools developed earlier.

Let us reformulate Maxwell’s equations using what are referred to as potentials. It follows from Theorem 10.6.3 and [M2] that there is a vector field $A$ in $\mathfrak{X}(\mathbb{R}_0^3 \times \mathbb{R})$, called the **magnetic potential**, such that

$B = \operatorname{curl}(A).$ (22.2.2)