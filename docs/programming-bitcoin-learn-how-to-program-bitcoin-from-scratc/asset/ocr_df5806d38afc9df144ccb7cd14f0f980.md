One property of scalar multiplication is that it’s really hard to predict without calculating (see Figure 3-4).

![](bbox=[0.13, 0.15, 0.83, 0.47])

Figure 3-4. Scalar multiplication results for y² = x³ + 7 over F₂₂₃ for point (170,142)

Each point is labeled by how many times we’ve added the point. You can see that this is a complete scattershot. This is because point addition is nonlinear and not easy to calculate. Performing scalar multiplication is straightforward, but doing the opposite, point division, is not.

This is called the discrete log problem and is the basis of elliptic curve cryptography.

Another property of scalar multiplication is that at a certain multiple, we get to the point at infinity (remember, the point at infinity is the additive identity or 0). If we imagine a point G and scalar-multiply until we get the point at infinity, we end up with a set:

{ G, 2G, 3G, 4G, ... nG } where nG = 0

It turns out that this set is called a group, and because n is finite, we have a finite group (or more specifically, a finite cyclic group). Groups are interesting mathematically because they behave well with respect to addition:

G + 4G = 5G or aG + bG = (a + b)G