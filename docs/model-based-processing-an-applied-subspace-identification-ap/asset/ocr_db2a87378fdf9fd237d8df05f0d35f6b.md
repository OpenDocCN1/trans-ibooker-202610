𝒜 := P × ℋ_{K,K}† × Q ⇒ A = 𝒜(1 : N_x, 1 : N_x) (6.25)

where recall that ℋ_{K,K}† is the shifted Hankel matrix of Eq. (6.10) and N_u is the dimension of the input vector u with N_y that of the output or measurement vector y with the notation 1 : N → 1, 2, …, N or equivalently select the corresponding rows (columns) 1-to-N of the matrix.

Consider the following example to demonstrate this approach.

# Example 6.1 Suppose we have a known N_y × N_u transfer function matrix (unstable) of Example 3.1 with N_y = N_u = 2 as before. The impulse response sequence is given by

H(z) = [1 1; 1 1] z^{-1} + [-2 -4; -3 -1] z^{-2} + [4 10; 7 1] z^{-3} + [-8 -22; -15 -1] z^{-4} + [16 46; 31 1] z^{-5}

Constructing the corresponding Hankel and shifted Hankel matrices for this problem, we have

H_{2,2} = [1 1 | -2 -4; 1 1 | -3 -1; - - - - -; -2 -4 | 4 10; -3 -1 | 7 1]; H_{2,2}† = [-2 -4 | 4 10; -3 -1 | 7 1; - - - - -; 4 10 | -8 -22; 7 1 | -15 -1]

Performing elementary row and column operations on ℋ_{2,2}, we obtain

P = [1 0 0 0; -1 0 -1/2 0; 1 -1 0 0; -2/3 -1/6 -1/6 -1/6]; Q = [1 -1 2 9; 0 1 0 1; 0 0 1 3; 0 0 0 1]

The input B ⇒ 𝒫(1 : 4, 1 : 2) and output C ⇒ Q(1 : 2, 1 : 4) matrices are found as

Pℋ_{2,2} = B { [1 1 | 2 9; 0 1 | 0 1; 0 0 | 1 3; 0 0 | 0 1] } = [B | AB]; ℋ_{2,2}Q = [1 0 0 0; 1 0 -1 0; - - - -; -2 -2 0 0; -3 -2 1 -5] = [C; --; CA] with C overbrace