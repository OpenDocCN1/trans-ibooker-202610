To specify the orders \( r, s, \) and \( b,\) one can make use of the patterns of \( v_i \) shown in Eq. (3.4), especially the difference equation \( v_i - \sum_{j=1}^r \delta_j v_{i-j} = 0 \) for \( i > b+s \) and \( v_i = 0 \) for \( i < b.\) This can be done via the _corner method,_ which is given below.

**Corner Method.** The corner method is designed to show the patterns of the impulse response weights \( v_j \) of a TFM. A two-way table is built where the rows are numbered \( 0, 1, 2, \dots \) and the columns \( 1, 2, 3, \dots \). Each element of this table is the determinant of a matrix of standardized impulse function values \( u(B) = v(B)/v_{\max},\) where \( v_{\max} = \max_j \{|v_j|\}.\) The \( (i, j)\)th element of the two-way table is the determinant of the \( j \times j \) matrix

```
\( M(i, j) = \left[ \begin{array}{cccc} u_i & u_{i-1} & \cdots & u_{i-j+1} \\ u_{i+1} & u_i & \cdots & u_{i-j+2} \\ \vdots & \vdots & \ddots & \vdots \\ u_{i+j-1} & u_{i+j-2} & \cdots & u_i \end{array} \right], \)
```

where \( u_i = 0 \) if \( i < 0.\) From the patterns of \( v_i \) in Eq. (3.4), we see that (a) \( |M(i, j)| = 0 \) for \( i \le b - 1,\) because all elements of the first row \( M(i, j) \) are zero for \( i \le b - 1,\) (b) \( |M(i, j)| = 0 \) for \( j > r \) and \( i > b + s,\) because the rows of \( M(i, j) \) are linearly dependence introduced by the difference equation of \( v_i,\) and (c) \( |M(i, j)| \neq 0 \) for \( i = b, \dots, b + s,\) because of the presence of \( w_{i-b}.\) Consequently, the table of corner method should exhibit the pattern shown in Figure 3.1 from which the values of \( (r, s, b) \) can be identified.

Specifically, from Table 3.1, we can identity \( b, r, s \) as follows:

+   1. \( b \) denotes the first row of the table with some non-zero entries.

+   2. There exists a rectangle of zeros with upper-left corner located at \( (s + b + 1, r + 1),\) from which \( r \) and \( s \) can be identified.

To demonstrate, Table 3.2 shows the corner method for the TF \( v(B) = \frac{(0.4+0.4B)B^3}{1-0.5B} \) of the left panel of Figure 3.1. From the table, it is easily seen that \( b = 3, r + 1 = 2,\) and \( s + b + 1 = 5.\) Therefore, as expected, \( (r, s, b) = (1, 1, 3).\)

**TABLE 3.1 Theoretical Pattern of Impulse Response Weights of a TFM**

| (i,j) | 1 | 2 | \(\cdots\) | \(r - 1\) | \(r\) | \(r + 1\) | \(r + 2\) | \(\cdots\) |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 0 | 0 | 0 | \(\cdots\) | 0 | 0 | 0 | 0 | \(\cdots\) |
| 1 | 0 | 0 | \(\cdots\) | 0 | 0 | 0 | 0 | \(\cdots\) |
| \(\vdots\) | \(\vdots\) | \(\vdots\) | \(\vdots\) | \(\vdots\) | \(\vdots\) | \(\vdots\) | \(\vdots\) | \(\vdots\) |
| \(b - 1\) | 0 | 0 | \(\cdots\) | 0 | 0 | 0 | 0 | \(\cdots\) |
| \(b\) | X | X | \(\cdots\) | X | X | X | X | \(\cdots\) |
| \(\vdots\) | \(\vdots\) | \(\vdots\) | \(\vdots\) | \(\vdots\) | \(\vdots\) | \(\vdots\) | \(\vdots\) | \(\vdots\) |
| \(s + b\) | X | X | \(\cdots\) | X | X | X | X | \(\cdots\) |
| \(s + b + 1\) | * | * | \(\cdots\) | * | X | 0 | 0 | \(\cdots\) |
| \(s + b + 2\) | * | * | \(\cdots\) | * | X | 0 | 0 | \(\cdots\) |
| \(\vdots\) | \(\vdots\) | \(\vdots\) | \(\vdots\) | \(\vdots\) | \(\vdots\) | \(\vdots\) | \(\vdots\) | \(\vdots\) |

* Denotes an arbitrary real number.