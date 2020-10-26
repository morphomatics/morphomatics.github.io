# Manifold-valued regression

Regression analysis is a reliable statistical approach to estimate the relationship between observed variables, e.g. shapes and their co-varying parameters.
For geometric data we again required intrinsic approaches that account for and fully leverage the geometry of the data space. 
The most widely used is to approximate the observed temporal shape data by geodesics, i.e. generalized straight lines.
Geodesic models are attractive as they feature a compact representation (similar to the slope and intercept term in linear regression) and therefore allow for computationally efficient inference.
However, non-monotonous shape changes, e.g. present in time-series of cardiac shape motion or anatomical changes in the human brain over the course of decades, do generally not adhere to constraints of geodesicity.

_Morphomatics_ provides nonlinear regression for manifold-valued (in particular shape) data based on Riemannian spline models.
This framework is very flexible allowing to model geodesics, generalized polynomials, and composite Bezeier splines (possibly closed).
Employing constructive algorithms the provided models still allow for efficient and exact evaluation.

## Least squares estimation

Sum-of-squares Riemannian distances

## Geodesic regression

TODO: example code

## Higher-order regression

TODO: exampe code