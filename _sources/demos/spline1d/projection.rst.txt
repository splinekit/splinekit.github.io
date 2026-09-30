Projection
==========

How to let a new spline with a new degree and delay best approximate a given spline of a given degree and delay.

----

Formulation
-----------

Let a given piecewise polynomial periodic spline be specified by its positive integer period :math:`K\in{\mathbb{N}}+1,` its nonnegative integer degree :math:`n_{0}\in{\mathbb{N}},` and its delay :math:`\delta x_{0}\in{\mathbb{R}}.` It is the function

..  math::
    f_{0}:{\mathbb{R}}\rightarrow{\mathbb{R}},x\mapsto f_{0}(x)=\sum_{k\in{\mathbb{Z}}}\,c_{0}[{k\bmod K}]\,\beta^{n_{0}}(x-\delta x_{0}-k),

where :math:`\left(c_{0}[k]\right)_{k=0}^{K-1}` is the vector of its spline coefficients and :math:`\beta^{n_{0}}:{\mathbb{R}}\rightarrow{\mathbb{R}}` is a B-spline of degree :math:`n_{0}.` We want to find the vector :math:`\left(c[k]\right)_{k=0}^{K-1}` of spline coefficients that minimizes the continuous least-squares criterion

..  math::
    J=\frac{1}{2}\,\int_{0}^{K}\,\left(f(x)-f_{0}(x)\right)^{2}\,{\mathrm{d}}x,

where :math:`f` is the spline :math:`f:{\mathbb{R}}\rightarrow{\mathbb{R}},x\mapsto f(x)=\sum_{k\in{\mathbb{Z}}}\,c[{k\bmod K}]\,\beta^{n}(x-\delta x-k)` with same period :math:`K` but with possibly different degree :math:`n\in{\mathbb{N}}` and delay :math:`\delta x\in{\mathbb{R}}.`

By analogy with the linear-algebra terminology, the spline :math:`f` is said to be the *projection* of the spline :math:`f_{0}` onto the space of splines of degree :math:`n` and delay :math:`\delta x.`

Solution
--------

The coefficients that minimize the criterion :math:`J` are

..  math::
    c[k]=\sum_{p\in{\mathbb{Z}}}\,\left(\sum_{q\in{\mathbb{Z}}}\,\left(b^{2\,n+1}\right)^{-1}[q]\,c_{0}[{\left(p-q\right)\bmod K}]\right)\,\beta^{n+n_{0}+1}(\left(k-\left(\delta x_{0}-\delta x\right)\right)-p),

for all :math:`k\in[0\ldots K-1].` Letting :math:`{\mathbf{[\![}}\cdot\,{\mathbf{]\!]}}` be the notation for the Iverson bracket, the sequence :math:`\left(\left(b^{2\,n+1}\right)^{-1}[q]\right)_{q\in{\mathbb{Z}}}` is such that :math:`\sum_{q\in{\mathbb{Z}}}\,\left(b^{2\,n+1}\right)^{-1}[q]\,\beta^{2\,n+1}(k-q)={\mathbf{[\![}}0=k\,{\mathbf{]\!]}}.`

We now propose a few lines of code that first create a random spline of specified period, degree, and delay, and then determine which spline of arbitrary new degree and delay best approximates it.

..  admonition:: Jupyter Lab notebook

    `Projection of a spline <https://splinekit.github.io/splinekit-jupyterlite/lab/?path=periodic-spline/projection/spline_projection.ipynb&mode=single-document>`_