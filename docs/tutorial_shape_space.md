# Shape space

A key component in the analysis of shape collections is the notion of a shape space, i.e. a space in which every point corresponds to a particular shape.
We will follow a deformation-based approach where a common deformable template encodes the typicality of the object class under study.
The shape variability in turn is encoded by deformations of the template (referred to as reference shape in the following).

## Discrete representation

To obtain a computational model, we require a digital representation of shapes and variations thereof.
To this end, we employ triangular surface meshes for which we provide the `Surface` class.
A surface mesh is specified by a pair of matrices:

$v = \begin{bmatrix} x_0 & y_0 & z_0 \\ x_1 & y_1 & z_1 \\ & \vdots & \end{bmatrix} \in \mathbb{R}^{n \times 3}
\text{ and }
f = \begin{bmatrix} i_0 & j_0 & k_0 \\ i_1 & j_1 & k_1 \\ & \vdots & \end{bmatrix} \in \mathbb{R}^{m \times 3},$

where $v$ holds the coordinates of $n$ vertices and $f$ lists which vertices (i.e. indices thereof w.r.t. $v$) form each of $m$ triangles.
For example, we can create a tetrahedron like this:

```py
import numpy as np
from morphomatics.geom import Surface

# 4 vertices
v = np.array([
    [0.57735, 0.57735, 0.57735],
    [-0.57735, 0.57735, -0.57735],
    [-0.57735, -0.57735, 0.57735],
    [0.57735, -0.57735, -0.57735]
])

# 4 triangles
# note: by sharing vertices (each is referenced 3 times), triangles are 'glued' together
f = np.array([
    [0, 3, 1],
    [1, 3, 2],
    [1, 2, 0],
    [0, 2, 3]
])

S = Surface(v, f)
```

In order to encode deformations, hence shape variations, we focus on simplicial maps, i.e. deformations that map triangles onto triangles and are entirely determined by the images of the vertices.
Given a triangulated reference shape $(\bar{v}, \bar{f})$, each surface in a collection can be represented by a mesh with same connectivity $f_i \equiv \bar{f}$ and mapped vertices $v_i = \phi_i(\bar{v})$.
The task of determining the $\phi_i$ is known as correspondence problem.
There is a multitude of approaches to establish correspondence ranging from fully automatic to expert guided ones.
The best choice is typically application dependent and we assume that this step has been carried out during pre-process. 


## Shape distance

Codifying shapes as simplicial deformations allows to interpret them as elements in the space of vertex coordinates $\mathbb{R}^{n \times 3}$.
This configuration space not only encodes the geometric form of objects but also their scale, position and orientation within the 3D space they are embedded in.

We can endow this space with a rich geometric structure by equipping it with a non-trivial metric that quantifies shape (dis)similarity.
In particular, for shape spaces we require the metric to be invariant under rotations and translations.
We can further adopt a physically-based perspective and design the metric to favor natural deformations promising an improved consistency and compact encoding of constraints.

The different approaches available within morphomatics are sub-classes of `ShapeSpace` (in `morphomatics.manifold.ShapeSpae`) that in turn inherits from _pymanopt_'s class `Manifold`. 
The available shape spaces are:

* __Point distribution model__ (see `morphomatics.manifold.PointDistributionModel`)
    A linearized, i.e. Euclidean, shape space mainly for comparison purposes.
    Rotational and translational effects are reduced via Procrustes alignment to the reference shape. 

* __Differential coordinates model__ (see `morphomatics.manifold.DifferentialCoords`)
    A Riemannian shape space that is able to account for the nonlinearity in shape variation by employing a differential representation that puts the local geometric variability into focus.
    The differential coordinates are endowed with a Lie group structure that comes with both:
    Excellent theoretical properties and closed-form expressions yielding simple and efficient algorithms.
    While translation invariant, rotational effects are reduced via Procrustes alignment to the reference shape.

* __Fundamental coordinates model__ (see `morphomatics.manifold.FundamentalCoords`)
   A surface-theoretic approach that is invariant under Euclidean motion and thus alignment-free.
   The rich structure of the derived shape space assures valid shape instances even in presence of strong nonlinear variability.
   The representation builds upon metric distortion and curvature of shapes as elements of Lie groups that allow for closed-form evaluation of Riemannian operations.

## Geodesic calculus

Due to the lack of familiar properties such as a vector space structure, calculus in non-Euclidean shape spaces can be computationally challenging.
Remarkably, the shape space provided here account for the non-linearity in shapes and at the same time offer fast and numerically robust processing.
For example, computing geodesic distances or Riemannian exponential/logarithmic maps does not require iterative approximation schemes.

```py
S_1: Surface
S_2: Surface
M: ShapeSpace

# TODO: load surfaces, init space

# map surfaces to shape space coordinates
c_1 = M.to_coords(S_1.v)
c_2 = M.to_coords(S_2.v)

# perform computations, e.g. ...

# ... compute distance
M.dist(c_1,c_2)

# ... interpolate surfaces (mid-point on geodesic)
diff = M.log(c_1, c_2)
mean = M.exp(c_1, 0.5*diff)

# get vertex coordinates of mean
v = M.from_coords(mean)

```
