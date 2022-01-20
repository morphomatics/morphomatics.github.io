# Morphomatics: Geometric morphometrics in non-Euclidean shape spaces

Morphomatics is an open-source Python library for (statistical) shape analysis developed within the [geometric data analysis and processing](https://www.zib.de/visual/geometric-data-analysis-and-processing) research group at Zuse Institute Berlin.
It contains prototype implementations of intrinsic manifold-based methods that are highly consistent and avoid the influence of unwanted effects such as bias due to arbitrary choices of coordinates.

The [source code](https://github.com/morphomatics/morphomatics) is freely available on GitHub.
Walk through the tutorials live on [![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/morphomatics/morphomatics.github.io/HEAD?filepath=docs%2Ftutorials)

##Citation

If you use Morphomatics in your academic projects, we politely ask you to aknowledge Morphomatics in your manuscript and to cite each of the publications (listed below) that are relevant for your use.
You may cite the library in general using this BibTeX entry:

```bibtex
@misc{Morphomatics,
  title = {Morphomatics: Geometric morphometrics in non-Euclidean shape spaces},
  author = {Felix Ambellan and Martin Hanik and Christoph von Tycowicz},
  note = {https://morphomatics.github.io/},
  year = {2021},
  doi = {10.12752/8544}
}
```

Of course, if you want, we will also list a reference to your work/project/etc. here. 


## Riemannian shape spaces

We provide a novel Riemannian framework for shape analysis based on differential coordinates that naturally belong to Lie groups and effectively describe local changes in shape.
Performing intrinsic calculus on this representation allows for fast computations while, at the same time, accounts for the nonlinearity in shape variation.
The rich structure of the derived shape space yields highly discriminative shape descriptors providing a compact representation that is amenable to learning algorithms.

References:

> Christoph von Tycowicz, Felix Ambellan, Anirban Mukhopadhyay, and Stefan Zachow:  
> **[An Efficient Riemannian Statistical Shape Model using Differential Coordinates.](https://opus4.kobv.de/opus4-zib/files/6117/ZIBReport_16-69.pdf)**  
> Medical Image Analysis, Volume 43, January 2018.</br>
> [![DOI](https://img.shields.io/badge/DOI-10.1016/j.media.2017.09.004-yellow)](http://dx.doi.org/10.1016/j.media.2017.09.004) [![Preprint](https://img.shields.io/badge/Preprint-ZIB--Report_16--69-silver)](https://opus4.kobv.de/opus4-zib/files/6117/ZIBReport_16-69.pdf)

<!--  -->
> Felix Ambellan, Stefan Zachow, and Christoph von Tycowicz:  
> **[Rigid Motion Invariant Statistical Shape Modeling Based on Discrete Fundamental Forms.](https://doi.org/10.1016/j.media.2021.102178)**  
> Medical Image Analysis, Volume 73, January 2021.</br>
> [![DOI](https://img.shields.io/badge/DOI-10.1016/j.media.2021.102178-yellow)](https://doi.org/10.1016/j.media.2021.102178) [![Preprint](https://img.shields.io/badge/arXiv-2111.06850-red)](http://arxiv.org/abs/2111.06850)

## Regression in Riemannian manifolds

References:

> Martin Hanik, Hans-Christian Hege, Anja Hennemuth, Christoph von Tycowicz:  
> **[Nonlinear Regression on Manifolds for Shape Analysis using Intrinsic Bézier Splines.](http://arxiv.org/abs/2007.05275)**  
> Proc. Medical Image Computing and Computer Assisted Intervention (MICCAI), 2020. </br>
> [![DOI](https://img.shields.io/badge/DOI-10.1007/978--3--030--59719--1__60-yellow)](http://dx.doi.org/10.1007/978-3-030-59719-1_60) [![Preprint](https://img.shields.io/badge/arXiv-2007.05275-red)](http://arxiv.org/abs/2007.05275)


## Similarity measures for sample distributions in Lie groups

> Martin Hanik, Hans-Christian Hege, Christoph von Tycowicz:  
> **[Bi-invariant Two-Sample Tests in Lie Groups for Shape Analysis.](https://arxiv.org/abs/2008.12195)**  
> Shape in Medical Imaging, 2020. </br>
> [![DOI](https://img.shields.io/badge/DOI-10.1007/978--3--030--61056--2__4-yellow)](http://dx.doi.org/10.1007/978-3-030-61056-2_4) [![Preprint](https://img.shields.io/badge/arXiv-2008.12195-red)](http://arxiv.org/abs/2008.12195)

## Applications

Several applications have been successively developed based on algorithms from Morphomatics:

* __Predicting Cognitive Scores__ **[[Hanik et al. 21]](http://dx.doi.org/10.1007/s11682-021-00585-7)** [(arXiv)](http://arxiv.org/abs/2106.09408)
* __Geodesic B-Score__ **[[Ambellan et al. 21]](http://dx.doi.org/10.1007/978-3-030-78191-0_14)** [(arXiv)](http://arxiv.org/abs/2104.01107)
* __Thin-Volume Visualization__ **[[Herter et al. 21]](http://dx.doi.org/10.1111/cgf.14296)**
* __Jerash Silver Scroll Unfolding__ **[[Baum et al. 21]](http://dx.doi.org/10.1016/j.daach.2021.e00186)**


## Contributors

* [Christoph von Tycowicz](https://www.tycowicz.de)
* [Felix Ambellan](https://www.zib.de/members/ambellan)
* [Martin Hanik](https://www.zib.de/members/hanik)

<!--
## Install

* buckle up!
-->
