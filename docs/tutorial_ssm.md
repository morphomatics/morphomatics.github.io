# Statistical shape modeling

Statistical shape models (SSMs) provide a principled way for extracting knowledge from empirically given collections of objects.
Instead of considering only a few quantities derived from a shape, such as volume, studying shapes in their entirety allows for a full geometric characterization and hence more differentiated assertions about the shapes.
SSMs describe the geometric variability in a collection in terms of a mean shape and a hierarchy of major modes explaining the main trends of shape variation.
Based on a notion of shape space, SSMs can be learned from consistently parametrized instances from the object class under study.
The resulting models provide a shape prior that can be used to constrain synthesis and analysis problems.
Moreover, their parameter space provides a compact representation that is amenable to learning algorithms (e.g. classification or clustering), evaluation, and exploration.

## Intrinsic mean

The simplest - yet also perhaps most fundamentally important - statistic is the sample mean, which estimates the center of a data set.
Because a Riemannian manifold $\mathcal{M}$ has a distance metric $\text{dist}_\mathcal{M}$, we can characterize the mean as the point closest to the data points $x_1,\ldots,x_N \in \mathcal{M}$.
This leads to the notion of (sample) Fréchet mean that is the minimizer of the sum-of-squared geodesic distances to the data: 

$$\mu = \arg\min_{x\in\mathcal{M}}  \sum_{i=1}^N \text{dist}_\mathcal{M}^2(x,x_i)$$

While closed-form solutions exist in flat spaces, solving this least-squares problem in general requires iterative optimization routines.
To this end, `morphomatics.stats.ExponentialBarycenter` implements the efficient Newton-type fixed-point iteration

$$ \mu_{k+i} = \exp_{\mu_k} \left( 1/N \sum_{i=1}^N \log_{\mu_k}(x_i) \right). $$

As it requires only a notion of exponential and logarithmic map, it can also be used to compute bi-invariant means in affine connection spaces.

## Principal geodesic analysis

For second order statistics `morphomatics.stats.PrincipalGeodesicAnalysis` provides (linearized) Principal Geodesic Analysis at $\mu$ that is an extension of the common Principal Component Analysis to Riemannian manifolds allowing for covariance analysis.
In particular, the solution is found algorithmically by eigendecomposition of the Gram matrix $C=(c_{ij})_{ij}$, with elements $c_{ij}=g^{\mu}{\left(\log_\mu{(x_i)}, \log_\mu{(x_j)}\right)}$.

## Statistical shape model

In order to avoid a systematic bias due to the choice of reference shape (cf. the introduction to [Shape Space](tutorial_shape_space.md)), we require it to agree with the mean of the training data.
This approach yields a cyclic dependency between the reference and the mean shape.
In `morphomatics.stats.StatisticalShapeModel` we provide an method to construct a statistical shape model for which the mean agrees with the reference based on an alternating optimization strategy.


<!-- Get shape coefficients -->


