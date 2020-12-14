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

```
import numpy as np

from morphomatics.manifold import SO3
from morphomatics.stats import RiemannianRegression

"""Geodesic regression for data in SO(3)"""

M = SO3()

# z-axis is axis of rotation
I = np.eye(3)
R = np.array([[np.cos(np.pi / 6), -np.sin(np.pi / 6), 0], [np.sin(np.pi / 6), np.cos(np.pi / 6), 0], [0, 0, 1]])

# data 
Y = np.zeros((4, 1, 3, 3))
Y[0, 0] = I
Y[1, 0] = M.exp(M.geopoint(I, R, 1 / 3), np.array([[0, 0, 0], [0, 0, 0.2], [0, -0.2, 0]]))
Y[2, 0] = M.exp(M.geopoint(I, R, 2 / 3), np.array([[0, 0, 0.1], [0, 0, 0], [-0.1, 0, 0]]))
Y[3, 0] = R

# corresponding time points
t = np.array([0, 1/3, 2/3, 1, 4/3, 5/3,  2])

# cubic curve model
degrees = np.array([1])

# solve
regression = RiemannianRegression(M, Y, t, degrees)

# optimal geodesic
gam = regression.trend

# evalute gam at 100 equidistant time points
p = gam.eval()
```

## Higher-order regression

If the relationship between the observed variable and its co-varying parameter is nonlinear (e.g., when there are saturation effects), then geodesic regression is not adequate.
Instead, higher-order models are necessary that generalize polynomial regression. In Morphomatics, _manifold-valued Bézier curves_ are used for this. 
Apart from being generalized polynomials, they can be constructed explicitly, which allows for fast computations. After model selection, i.e., choosing a degree $k$, the least squares estimator 
within the class of Bézier curves of degree $k$ is the result of the regression.

In the following, we show regression with cubic Bézier curves for the same data as in the example above.

```
import numpy as np

from morphomatics.manifold import SO3
from morphomatics.stats import RiemannianRegression

"""Cubic regression for data in SO(3)"""

M = SO3()

# z-axis is axis of rotation
I = np.eye(3)
R = np.array([[np.cos(np.pi / 6), -np.sin(np.pi / 6), 0], [np.sin(np.pi / 6), np.cos(np.pi / 6), 0], [0, 0, 1]])

# data 
Y = np.zeros((4, 1, 3, 3))
Y[0, 0] = I
Y[1, 0] = M.exp(M.geopoint(I, R, 1 / 3), np.array([[0, 0, 0], [0, 0, 0.2], [0, -0.2, 0]]))
Y[2, 0] = M.exp(M.geopoint(I, R, 2 / 3), np.array([[0, 0, 0.1], [0, 0, 0], [-0.1, 0, 0]]))
Y[3, 0] = R

# corresponding time points
t = np.array([0, 1/3, 2/3, 1, 4/3, 5/3,  2])

# degree of a cubic Bézier curve is 3
degrees = np.array([3])

# solve
regression = RiemannianRegression(M, Y, t, degrees)

# optimal cubic Bézier curve
bet = regression.trend

# evalute bet at 100 equidistant time points
p = bet.eval()
```

## Spline regression

TODO: example code