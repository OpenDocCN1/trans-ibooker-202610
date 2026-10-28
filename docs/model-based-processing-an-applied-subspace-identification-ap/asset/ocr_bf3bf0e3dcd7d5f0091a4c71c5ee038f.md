# Example 6.5

Consider Example 6.1 and assume that we do not know the order $N_x$. We would create an extended Hankel matrix in this case with $K=7$ leading to

$$H_{4,4}=\begin{bmatrix}
1&2&|&2&4&|&4&8&|&8&16\\
1&2&|&2&4&|&6&10&|&13&22\\
1&0&|&1&0&|&3&2&|&6&6\\
-&-&-&-&-&-&-&-&-&-&-\\
2&4&|&4&8&|&8&16&|&16&32\\
2&4&|&6&10&|&13&22&|&28&48\\
1&0&|&3&2&|&6&6&|&13&16\\
-&-&-&-&-&-&-&-&-&-&-\\
4&8&|&8&16&|&16&32&|&32&64\\
6&10&|&13&22&|&28&48&|&58&102\\
3&2&|&6&6&|&13&16&|&27&38\\
-&-&-&-&-&-&-&-&-&-&-\\
8&16&|&16&32&|&32&64&|&64&128\\
13&22&|&28&48&|&58&102&|&119&214\\
6&6&|&13&16&|&27&38&|&56&86
\end{bmatrix}$$

+   1. Start with $[I_{12}\mid H_{4,4}\mid I_8]$ and perform elementary row operations to obtain $[P\mid Q\mid I_8]$.

$$\left[\begin{array}{cccccccccccc|cccccccc}
1&0&0&0&0&0&0&0&0&0&0&0&1&2&2&4&4&8&8&16\\
-1&1&0&0&0&0&0&0&0&0&0&0&0&0&0&2&2&5&6\\
-3/2&1/2&1&0&0&0&0&0&0&0&0&0&0&-2&-1&-4&0&-5&1/2&-7\\
-2&0&0&1&0&0&0&0&0&0&0&0&0&0&0&0&0&0&0&0\\
1/2&-5/2&0&0&1&0&0&0&0&0&0&0&0&0&2&2&0&1&-1/2&1\\
1&1&-1&0&-1&1&0&0&0&0&0&0&0&0&0&0&0&0&0&0\\
-4&0&0&0&0&1&0&0&0&0&0&0&0&0&0&0&0&0&0&0\\
-3&0&-1&0&-1&0&0&1&0&0&0&0&0&0&0&0&0&0&0&0\\
0&1&-2&0&-1&0&0&0&1&0&0&0&0&0&0&0&0&0&0&0\\
-8&0&0&0&0&0&0&0&0&1&0&0&0&0&0&0&0&0&0&0\\
-8&1&-2&0&-2&0&0&0&0&0&1&0&0&0&0&0&0&0&0&0\\
-1&2&-3&0&-2&0&0&0&0&0&0&1&0&0&0&0&0&0&0&0
\end{array}\right]$$

$P$ is the left block, $Q$ is the middle-right block, and $I_8$ is the rightmost identity block.

+   2. Determine the observability and controllability invariants using the row and column predecessor chain constructs of Eqs. (6.38) and (6.40), that is, from the underlined entries of the $Q$-matrix we see that (independent rows/columns annotated in parentheses).