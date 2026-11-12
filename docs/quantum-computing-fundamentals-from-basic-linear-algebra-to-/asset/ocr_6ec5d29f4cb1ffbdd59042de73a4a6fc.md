![](bbox=[0.07, 0.12, 0.46, 0.56])

FIGURE 15.4 Doubling the P

The tangent from P is always vertical if yP = 0.

If a point P is such that yP = 0, then the tangent line to the elliptic curve at P is vertical and does not intersect the elliptic curve at any other point. By definition, 2P = O for such a point P.

Recall that the field Fp uses the numbers from 0 to p – 1, and computations end by taking the remainder of division by p (i.e., the modulus operations). For example, in F23, the field is composed of integers from 0 to 22, and any operation within this field will result in an integer also between 0 and 22.

An elliptic curve with an underlying field of Fp can be formed by choosing the variables a and b within the field of Fp. The elliptic curve includes all points (x,y) that satisfy the elliptic curve equation modulo p (where x and y are numbers in Fp).

For example, y2 mod p = x3 + ax + b mod p has an underlying field of Fp if a and b are in Fp.

If x3 + ax + b contains no repeating factors, then the elliptic curve can be used to form a group. An elliptic curve group over Fp consists of the points on the corresponding elliptic curve, together with a special point O called the point at infinity. There are finitely many points on such an elliptic curve.