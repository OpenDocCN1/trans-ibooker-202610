**TransD** [225] is an attempt to improve TransR by reducing the number of required parameters by removing the need for matrix vector multiplications. The core of this model is to use two vectors for representation of entities and relations. Assuming that $h, r, t$ encode the semantics, and $h_p, r_p, t_p$ constructs projection, the projection of entities in relation-specific spaces is defined as follows:

```
$M_{rh} = r_p h_p^T + I^{m \times n}$  (9)
```

```
$M_{rt} = r_p t_p^T + I^{m \times n},$  (10)
```

In this definition, $I$ is a matrix where the values of the diagonal elements are 1 and 0 elsewhere. The head and tail entities are computed as:

```
$h_{\perp} = M_{rh} h$  (11)
```

```
$t_{\perp} = M_{rt} t$  (12)
```

The score of the triple (h,r,t) is then computed based on these projections:

```
$f_r(h,t) = -||h_{\perp} + r - t_{\perp}||_2^2$  (13)
```

# **RotatE.** [417] is one of the early models which uses rotation than translation. The model is mainly designed with the objective of reasoning relational patterns, which was not mainly addressed by other translational models. RotatE is designed to reason new knowledge based on the Euler formula $e^{i\theta} = \cos(\theta) + i \sin(\theta)$. Based on its score function, for every correct triple $(h, r, t)$ there should be the relation of $h_j r_j = t_j$ which holds $\forall j \in \{0, \ldots, d\}$. $h_j, r_j, t_j$ are the $j$-th elements of the embedding vectors of $\mathbf{h}, \mathbf{r}, \mathbf{t} \in \mathbb{C}^d$. Since it deals with complex space, $r_i$ is set to 1 i.e. $|r_j| = \sqrt{Re(r_j)^2 + Im(r_j)^2} = 1$. The model performs a rotation of the $j$-th element $h_j$ of the head vector $\mathbf{h}$ by the $j$-th element $r_j = e^{i\theta_r^j}$ of a relation vector $\mathbf{r}$ to get the $j$-th element $t_j$ of the tail vector $\mathbf{t}$, where $\theta_{r_j}$ is the phase of the relation $r$. Therefore, the score function of RotatE is designed as a rotation using $\circ$ which is a Hadmard product of two vectors:

```
$f_{h,t}^r = ||\mathbf{h} \circ \mathbf{r} - \mathbf{t}||,$  (14)
```

In this way, the RotatE model becomes capable of encoding symmetric, inverse, and composition relation patterns. Due to this capability, its performance is high and due to the high quality of the newly discovered links in the reasoning process, it outperforms all the previous models.

# **Semantic Matching Models.** As discussed before, the second category of embedding models in reasoning over KGs determines the plausibility of a triple by comparing the similarity of the latent features of the entities and relations. A number of KGE models fall into this category; we will discuss a few of the best performing ones.