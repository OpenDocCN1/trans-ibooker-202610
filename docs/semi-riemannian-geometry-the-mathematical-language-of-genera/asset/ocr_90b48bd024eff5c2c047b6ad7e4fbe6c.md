and the corresponding vector product is

$$(3r^2-1,2r,0)\times(0,0,1)=(2r,-3r^2+1,0),$$

which never equals $(0,0,0)$. By Theorem 11.2.2, $\varphi$ is an immersion. The self-intersection is parametrized by

$$\varphi(1,s)=(0,0,s)=\varphi(-1,s)$$

for all real numbers $s$. We have from Theorem 11.2.1(b) that

$$d_{(1,s)}(\varphi)(\mathbb{R}^2)=\operatorname{span}\{(2,2,0),(0,0,1)\}$$

and

$$d_{(-1,s)}(\varphi)(\mathbb{R}^2)=\operatorname{span}\{(2,-2,0),(0,0,1)\}.$$

This shows that along the line of self-intersection, there are two candidates for “tangent plane” at each point.

![](bbox=[0.307, 0.399, 0.693, 0.621])

**Figure 11.2.2.** Diagram for Example 11.2.4

The upshot of the preceding examples is that $\varphi$ being an immersion is necessary for $\varphi(U)$ to be “smooth”, but not sufficient. At a minimum, we need to add the requirement that $\varphi(U)$ does not self-intersect, or equivalently, that $\varphi$ is injective. Further examples (that will not be presented) reveal additional deficiencies inherent in defining a “smooth surface” to be the image of some type of parametrized surface.

We now take a different approach to the problem that can be loosely described as follows: a “smooth surface” is defined to be a topological subspace of $\mathbb{R}^3$ that can be covered in a piecewise fashion by a collection of parametrized surfaces in such a way that the pieces “fit together nicely”. We need to make all this precise.

Let $M$ be a topological subspace of $\mathbb{R}^3$. A **chart (on $M$)** is a pair $(U,\varphi)$, where $U$ is an open set in $\mathbb{R}^2$ and $\varphi:U\to\mathbb{R}^3$ is a parametrized surface such that: