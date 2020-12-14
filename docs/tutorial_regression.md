# Manifold-valued regression

Regression analysis is a reliable statistical approach to estimate the relationship between observed variables, e.g. shapes and their co-varying parameters.
For geometric data we again required intrinsic approaches that account for and fully leverage the geometry of the data space. 
The most widely used is to approximate the observed temporal shape data by geodesics, i.e. generalized straight lines.
Geodesic models are attractive as they feature a compact representation (similar to the slope and intercept term in linear regression) and therefore allow for computationally efficient inference.
However, non-monotonous shape changes, e.g. present in time-series of cardiac shape motion or anatomical changes in the human brain over the course of decades, do generally not adhere to constraints of geodesicity.

_Morphomatics_ provides nonlinear regression for manifold-valued (in particular shape) data based on Riemannian spline models.
This framework is very flexible allowing to model geodesics, generalized polynomials, and Bézier splines (possibly closed).
Employing constructive algorithms the provided models still allow for efficient and exact evaluation.

## Least squares estimation

Every Riemannian manifold $M$ (in particular a manifold of shapes) comes with a distance function $d$. Given parameter-data pairs $(t_1, p_1),\dots,(t_n,p_n) \in \mathbb{R} \times M$ a widely used notion
to measure how well a (parametrized) curve $\alpha$ estimates the data is the sum-of-squared error

\[
\mathcal{E}(\alpha) := \sum_{i=1}^n d(\alpha(t_i), p_i)^2,
\]

that is, the sum of the squared distances from the curve to the data points.
A minimizer of $\mathcal{E}$ (from a class of trajectories) is called least-squares estimator of the data (in this class). 

## Geodesic regression

Generalizing the linear ansatz, _geodesic regression_ assumes that the relationship of a manifold-valued observed variable and a (single) co-varying parameter is well approximated by a generalized straight line.
It is thus estimated by the geodesic least-squares estimator. The following example for data from the manifold of 3x3 rotation matrices shows how geodesic regression is computed in Morphomatics.

```py
import numpy as np

from morphomatics.manifold import SO3
from morphomatics.stats import RiemannianRegression

"""Geodesic regression for data in SO(3)"""

M = SO3()

# z-axis is axis of rotation
I = np.eye(3)
R = np.array([[np.cos(np.pi / 6), -np.sin(np.pi / 6), 0], [np.sin(np.pi / 6), np.cos(np.pi / 6), 0], [0, 0, 1]])

# 6 points in SO(3). The extra dimension is not needed here but comes into play when the data consists of 
# tuples of matrices.
Y = np.zeros((6, 1, 3, 3))
Y[0, 0] = M.exp(M.geopoint(I, R, -2 / 3), np.array([[0, 0, 0.1], [0, 0, 0], [-0.1, 0, 0]]))
Y[1, 0] = M.exp(M.geopoint(I, R, -1 / 3), np.array([[0, 0, 0], [0, 0, 0.2], [0, -0.2, 0]]))
Y[2, 0] = I
Y[3, 0] = M.exp(M.geopoint(I, R, 1 / 3), np.array([[0, 0, 0], [0, 0, 0.2], [0, -0.2, 0]]))
Y[4, 0] = M.exp(M.geopoint(I, R, 2 / 3), np.array([[0, 0, 0.1], [0, 0, 0], [-0.1, 0, 0]]))
Y[5, 0] = R

# corresponding time points
t = np.array([0, 1/5, 2/5, 3/5, 4/5, 1])

# geodesic has degree 1
degrees = np.array([1])

# solve
regression = RiemannianRegression(M, Y, t, degrees)

# geodesic least-squares estimator
gam = regression.trend

# evaluate geodesic at 100 equidistant points
X = gam.eval()

```
To visualize the regressed geodesic, the 100 rotation matrices that were sampled from it as well as the 6 data matrices
were applied to the vector $q=\begin{bmatrix} 1 & 0 & 0 \end{bmatrix}^T$. The results all lie on the unit sphere;
they are shown in the following image.
![Geodesic Regression](geodesic_regression.png)
**Figure 1** Geodesic regression in SO(3) visualized by applying each matrix to the vector $q=\begin{bmatrix} 1 & 0 & 0 \end{bmatrix}^T$.
The yellow spheres are the rotations of q by the data points. Applying each of the 100 samples of the optimal geodesic yields the colored curve 
(color indicating parametrization).


## Higher-order regression

If the relationship between the observed variable and its co-varying parameter is nonlinear (e.g., when there are saturation effects), then geodesic regression is not adequate.
Instead, higher-order models are necessary that generalize polynomial regression. In Morphomatics, _manifold-valued Bézier curves_ are used for this. 
Apart from being generalized polynomials, they can be constructed explicitly, which allows for fast computations. After model selection, i.e., choosing a degree $k$, the least squares estimator 
within the class of Bézier curves of degree $k$ is the result of the regression.

In the following, we show regression with cubic Bézier curves for the same data as in the example above. 

```py
import numpy as np

from morphomatics.manifold import SO3
from morphomatics.stats import RiemannianRegression

"""Regression with cubic curves for data in SO(3)"""

M = SO3()

# z-axis is axis of rotation
I = np.eye(3)
R = np.array([[np.cos(np.pi / 6), -np.sin(np.pi / 6), 0], [np.sin(np.pi / 6), np.cos(np.pi / 6), 0], [0, 0, 1]])

Y = np.zeros((6, 1, 3, 3))
Y[0, 0] = M.exp(M.geopoint(I, R, -2 / 3), np.array([[0, 0, 0.1], [0, 0, 0], [-0.1, 0, 0]]))
Y[1, 0] = M.exp(M.geopoint(I, R, -1 / 3), np.array([[0, 0, 0], [0, 0, 0.2], [0, -0.2, 0]]))
Y[2, 0] = I
Y[3, 0] = M.exp(M.geopoint(I, R, 1 / 3), np.array([[0, 0, 0], [0, 0, 0.2], [0, -0.2, 0]]))
Y[4, 0] = M.exp(M.geopoint(I, R, 2 / 3), np.array([[0, 0, 0.1], [0, 0, 0], [-0.1, 0, 0]]))
Y[5, 0] = R

# corresponding time points
t = np.array([0, 1/5, 2/5, 3/5, 4/5, 1])

# cubic Bézier curve have degree 3
degrees = np.array([3])

# solve
regression = RiemannianRegression(M, Y, t, degrees)

# cubic least-squares estimator
bet = regression.trend

# evaluate the curve at 100 equidistant points
X = bet.eval()

```
The results can be visulized as for geodesic regression. Note that the cubic curve describes the data a lot better than the geodesic.

![Cubic Regression](cubic_regression.png)
**Figure 2** Regression with cubic Bézier curves in SO(3) visualized by applying each matrix to the vector $q=\begin{bmatrix} 1 & 0 & 0 \end{bmatrix}^T$.
The yellow spheres are the rotations of q by the data points. Applying each of the 100 samples of the optimal geodesic yields the colored curve 
(color indicating parametrization).

## Spline regression

Generalized Bézier curves can be joined together in a differentiable way. The resulting _Bézier splines_ are very flexible
and, thus, allow to describe many phenomena. In particular, since there are closed Bézier splines,
they can be used to model cyclic behavior like the motion of the heart.

```py
import numpy as np

from morphomatics.manifold import SO3
from morphomatics.stats import RiemannianRegression

"""Regression with closed splines for data in SO(3)"""

M = SO3()

# z-axis is axis of rotation
I = np.eye(3)
Rz = np.array([[np.cos(np.pi / 6), -np.sin(np.pi / 6), 0], [np.sin(np.pi / 6), np.cos(np.pi / 6), 0], [0, 0, 1]])
# x-axis is axis of rotation
Rx = np.array([[1, 0, 0], [0, np.cos(np.pi / 6), -np.sin(np.pi / 6)], [0, np.sin(np.pi / 6), np.cos(np.pi / 6)]])

Y = np.zeros((4, 1, 3, 3))
Y[0, 0] = I
Y[1, 0] = Rz
Y[2, 0] = Rx @ Rz
Y[3, 0] = Rx

# spline consisting of 2 cubic segments (first and last segment must be at least cubic) 
degrees = np.array([3, 3])

# time points between 0 and 2 because we have two segments
t = np.array([1/3, 2/3, 4/3, 5/3])

# solve for closed spline
regression = RiemannianRegression(M, Y, t, degrees, iscycle=True)

# least-squares estimator within the class of closed Bézier splines with two cubic segments
bet = regression.trend

# evaluate the spline at 100 equidistant points
X = bet.eval(time=np.linspace(0, bet.nsegments, 100))

```

Again, the results can be visualized on the sphere. Note that the computed closed spline interpolates the data points in this case.
The reason for this is that we have only 4 data points and the same number of degrees of freedom. In general though, regression assumes noisy measurements and,
thus, least-squares optimization and not interpolation is the goal.


![Spline Regression](spline_regression.png)
**Figure 3** Regression with closed Bézier splines consisting of 2 cubic segments in SO(3). Results are visualized by 
applying each matrix to the vector $q=\begin{bmatrix} 1 & 0 & 0 \end{bmatrix}^T$.
The yellow spheres are the rotations of q by the data points. Applying each of the 100 samples of the optimal geodesic yields the colored curve 
(color indicating parametrization).