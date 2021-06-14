# Bi-invariant similarity measures
Given two distributions of samples, it is often necessary to quantify the difference between them. 
Two popular indices that can be used for data from Euclidean spaces are the Hotelling $T^2$ statistic and Bhattacharyya 
distance. While the first quantifies differences between the means only, the latter takes also differing covariance 
structures into account.
Remarkably, both can be generalized to data from Lie groups in a way that respects 
the group's fundamental properties: The _bi-invariant_ Hotelling $T^2$ statistic and Bhattacharyya distance are 
invariant under translations of the data from both left and right. For shape analysis 
this leads (amongst others) to an analysis that is independent of
the choice of reference.
For the definitions see **[Bi-invariant Two-Sample Tests in Lie Groups for Shape Analysis.](https://arxiv.org/abs/2008.12195)**
In this paper, there is also shown how a bi-invariant two sample test for differences in mean shape can
be constructed from these notions.

## Example
In the following, we show how to use bi-invariant similarity measures in _Morphomatics_. For this, we choose the Lie group 
$\textnormal{GL}^+(3)$ of 3-by-3 matrices with positive determinant. For bi-invariant statistics in $\textnormal{GL}^+(3)$
we need a non-metric (i.e., non-Riemannian) structure. When using it, a geodesic $\gamma$ passing through a matrix $A \in \textnormal{GL}^+(3)$ 
with tangent vector $X \in \mathbb{R}^{3,3}$ is given by the matrix exponential:

\[
\gamma(t) = Ae^{tX}.
\]

Since the matrix exponential is a fundamental _group_ property of $\textnormal{GL}^+(3)$, this gives the desired 
connection of group and geometric properties. (This works—far more generally—in any finite-dimensional Lie group.)

Thus, we can create two sample sets $E, F \subset \textnormal{GL}^+(3)$ with the identity matrix $I \in \textnormal{GL}^+(3)$ as mean as follows 
('groupexp' is the matrix exponential).

```py
import numpy as np

from morphomatics.stats import BiinvariantStatistics
from morphomatics.manifold import GLp3

# initialize Lie group of 3-by-3 matrices with positive determinant
G = GLp3()
# initialize module for bi-invariant statistics
bistat = BiinvariantStatistics(G)
# identity matrix
I = G.identity()

# sample 2 data sets around I
E = []
F = []
for i in range(3):
    for j in range(3):
        # create tangent vector
        e = np.zeros((1, 3, 3))
        e[0, i, j] = 1
        # shoot geodesic along tangent vector
        E.append(G.groupexp(e))
        E.append(G.groupexp(-e))
        F.append(G.groupexp(.2 * e))
        F.append(G.groupexp(-.2 * e))
```

We can run the following commands for these data sets.

```py
>>> bistat.hotellingT2(E, E)
0

>>> bistat.bhattacharyya(E, E)
0

>>> bistat.hotellingT2(E, F)
6.528013620729162e-29

>>> bistat.bhattacharyya(E, F)
4.2998015026234615
```

As expected the difference from a data set to itself is zero for both indices.
Furthermore, since $E$ and $F$ have mean $I$, their bi-invariant Hotelling $T^2$ statistic is still (numerically) zero.
On the other hand, the Bhatacharyya distance between them is positive, since their covariance structures differ.  

We can also test the invariance under translations of both indices. For $\textnormal{GL}^+(3)$ left and right 
translations ('lefttrans' and 'righttrans' in _Morphomatics_) by an element $B \in \textnormal{GL}^+(3)$ are simply multiplications with B from left and right, 
repectively.
```py
# random element
B = G.rand()
BE = []
BF = []
# left translate all elements of E and F by B
for e in E:
    BE.append(G.lefttrans(e, B))
for f in F:
    BF.append(G.lefttrans(f, B))
EB = []
FB = []
# right translate all elements of E and F by B
for e in E:
    EB.append(G.righttrans(e, B))
for f in F:
    FB.append(G.righttrans(f, B))
```
Now, we can compute both notions on the left/right translated data sets and compare the results with the original ones.
```py
>>> np.abs(bistat.hotellingT2(BE, BF) - bistat.hotellingT2(E, F))
4.015762545572897e-30

>>> np.abs(bistat.hotellingT2(EB, FB) - bistat.hotellingT2(E, F)) 
4.772821282573183e-30   

>>> np.abs(bistat.bhattacharyya(BE, BF) - bistat.bhattacharyya(E, F))
1.6813706956873606e-31

>>> np.abs(bistat.bhattacharyya(EB, FB) - bistat.bhattacharyya(E, F))
9.698928476829599e-32
```
Because of the bi-invariance property both indices give the same values for the translated data.