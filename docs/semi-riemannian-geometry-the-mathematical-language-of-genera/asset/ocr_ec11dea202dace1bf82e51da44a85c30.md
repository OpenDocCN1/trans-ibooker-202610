# Chapter 3

# Bilinear Functions

The inner product is an example of a bilinear function that likely is familiar to the reader. In this book, we are more interested in a less restrictive type of bilinear function called a scalar product, but we will not ignore the inner product altogether. In order to decide whether a bilinear function is a scalar product, we need to determine whether it is symmetric and nondegenerate. These properties of bilinear functions will be the focus of the present chapter.

# 3.1  Bilinear Functions

Let $V$ be a vector space. A function

$$\mathfrak{b}: V \times V \longrightarrow \mathbb{R}$$

is said to be **bilinear (on $V$)** if it is linear in both arguments; that is,

$$\mathfrak{b}(cu+v,w)=c\mathfrak{b}(u,w)+\mathfrak{b}(v,w)$$

and

$$\mathfrak{b}(u,cv+w)=c\mathfrak{b}(u,v)+\mathfrak{b}(u,w)$$

for all vectors $u,v,w$ in $V$ and all real numbers $c$. In the literature, a bilinear function is sometimes called a **quadratic form**. We often denote

$$\mathfrak{b}(\cdot,\cdot)\quad\text{by}\quad\langle\cdot,\cdot\rangle,$$

writing $\langle v,w\rangle$ in place of $\mathfrak{b}(v,w)$.