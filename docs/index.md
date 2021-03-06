# Morphomatics: Geometric morphometrics in non-Euclidean shape spaces

Morphomatics is an open-source Python library for (statistical) shape analysis developed within the [geometric data analysis and processing](https://www.zib.de/visual/geometric-data-analysis-and-processing) research group at Zuse Institute Berlin.
It contains prototype implementations of intrinsic manifold-based methods that are highly consistent and avoid the influence of unwanted effects such as bias due to arbitrary choices of coordinates.

The [source code](https://github.com/morphomatics/morphomatics) is freely available on GitHub.

##Citation

If you use Morphomatics in your academic projects, we politely ask you to aknowledge Morphomatics in your manuscript and to cite each of the publications (listed below) that are relevant for your use.
You may cite the library in general using this BibTeX entry:

```bibtex
@misc{Morphomatics,
  title = {Morphomatics: Geometric morphometrics in non-Euclidean shape spaces},
  author = {Felix Ambellan and Martin Hanik and Christoph von Tycowicz},
  note = {https://morphomatics.github.io/},
  year = {2021},
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
> Medical Image Analysis, Volume 43, January 2018.

<!--  -->
> Felix Ambellan, Stefan Zachow, and Christoph von Tycowicz:  
> **[A Surface-Theoretic Approach for Statistical Shape Modeling.](https://opus4.kobv.de/opus4-zib/files/7449/ZIBReport_19-20.pdf)**  
> Proc. Medical Image Computing and Computer Assisted Intervention (MICCAI), Lecture Notes in Computer Science, 2019.

## Manifold-valued regression

References:

> Martin Hanik, Hans-Christian Hege, Anja Hennemuth, Christoph von Tycowicz:  
> **[Nonlinear Regression on Manifolds for Shape Analysis using Intrinsic Bézier Splines.](http://arxiv.org/abs/2007.05275)**  
> Proc. Medical Image Computing and Computer Assisted Intervention (MICCAI), 2020. 

## Contributors

* [Christoph von Tycowicz](https://www.tycowicz.de)
* [Felix Ambellan](https://www.zib.de/members/ambellan)
* [Martin Hanik](https://www.zib.de/members/hanik)

<!--
## Install

* buckle up!
-->
