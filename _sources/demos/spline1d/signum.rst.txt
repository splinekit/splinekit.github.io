Signs, Zeros, and Zero-Crossings
================================

How to access the signed pieces of a piecewise-polynomial periodic one-dimensional spline and how to take advantage of them.

----

Terminology
-----------

Signum Function
^^^^^^^^^^^^^^^

The signum function is the complex-to-complex mapping

..  math::
    {\mathrm{sgn}}:{\mathbb{C}}\rightarrow{\mathbb{C}},z\mapsto{\mathrm{sgn}}(z)=\left\{\begin{array}{ll}0,&z=0\\{\mathrm{e}}^{{\mathrm{j}}\,\arg z},&z\neq0.\end{array}\right.

It maps the origin of the complex plane to itself, and it maps every other point to its closest point on the unit circle. Those points :math:`z\in{\mathbb{C}}` such that :math:`{\mathrm{sgn}}(z)=1` are said to be positive, and those points :math:`z\in{\mathbb{C}}` such that :math:`{\mathrm{sgn}}(z)=\left(-1\right)` are said to be negative; the set of the real numbers can then be partitioned as :math:`{\mathbb{R}}=\{z\in{\mathbb{C}}:{\mathrm{sgn}}(z)=-1\}\cup\{0\}\cup\{z\in{\mathbb{C}}:{\mathrm{sgn}}(z)=1\}.`

Zeros
^^^^^

Consider the piecewise polynomial spline :math:`f:{\mathbb{R}}\rightarrow{\mathbb{R}}` of nonnegative integer degree :math:`n.` If an interval :math:`{\mathbb{X}}` can be found such that it is of maximal diameter and such that :math:`\forall x\in{\mathbb{X}}:f(x)=0,` then this interval is said to be a *zero* of the spline.

Zero-Crossings
^^^^^^^^^^^^^^

Spline of Null Degree
"""""""""""""""""""""

Consider a spline of degree :math:`n=0.` If a proper interval :math:`{\mathbb{X}}` can be found such that :math:`\forall x\in{\mathbb{X}}:f(x)=0` and such that :math:`f(\inf{\mathbb{X}})\,f(\sup{\mathbb{X}})<0,` then the interval is said to be a *zero-crossing* of the spline.

If two proper intervals :math:`{\mathbb{A}}` and :math:`{\mathbb{B}}` can be found such that they are mutually disjoint, such that :math:`\sup{\mathbb{A}}=\inf{\mathbb{B}},` and such that :math:`\forall a\in{\mathbb{A}},\forall b\in{\mathbb{B}}:f(a)\,f(b)<0,` then, the degenerate interval :math:`{\mathbb{X}}=\{x\in{\mathbb{R}}:\sup{\mathbb{A}}=x=\inf{\mathbb{B}}\}` is said to be a *zero-crossing* of the spline.

Spline of Positive Degree
"""""""""""""""""""""""""

Consider a spline of degree :math:`n>0.` If three intervals :math:`{\mathbb{A}},` :math:`{\mathbb{X}},` and :math:`{\mathbb{B}}` can be found such that they are mutually disjoint, such that :math:`\left(\sup{\mathbb{A}}=\inf{\mathbb{X}}\right)\wedge\left(\sup{\mathbb{X}}=\inf{\mathbb{B}}\right),` such that :math:`\forall x\in{\mathbb{X}}:{\mathrm{sgn}}(f(x))=0,` and such that :math:`\forall a\in{\mathbb{A}},\forall b\in{\mathbb{B}}:f(a)\,f(b)<0,` then, :math:`{\mathbb{X}}` is said to be a *zero-crossing* of the spline.

Descending or Ascending Zero-Crossings
""""""""""""""""""""""""""""""""""""""

A *descending* zero-crossing is such that :math:`\lim_{x\uparrow\inf{\mathbb{X}}}{\mathrm{sgn}}(f(x))>\lim_{x\downarrow\sup{\mathbb{X}}}{\mathrm{sgn}}(f(x)).`

An *ascending* zero-crossing is a zero-crossing that is not descending.

----

Spline Signed Pieces
--------------------

Given a periodic spline of period :math:`K,` we want to establish a set of intervals that partitions the main period :math:`[0,K)` in such a way that each element of the set is an interval of largest diameter over which the spline takes a constant-valued sign. For ease of use, the set is returned as a list.

We now propose a few lines of code that create and display a random spline and extract its constant-sign pieces.

..  admonition:: Jupyter Lab notebook

    `Constant-sign pieces of a spline <https://splinekit.github.io/splinekit-jupyterlite/lab/?path=periodic-spline/sgn/spline_sgn.ipynb&mode=single-document>`_

----

Spline Zeros and Zero-Crossings
-------------------------------

Given a periodic spline, we want to establish a set of intervals of largest diameter that give either its zeros or its zero-crossings. We enforce that the diameter of the enclosure of all returned intervals is not larger than the period of this spline. For ease of use, the set is returned as a list.

We now propose a few lines of code that create and display a curated spline over which we illustrate and discuss several configurations, represented by a mixture of degenerate and proper intervals. In our control of the configurations, we retain the same spline coefficients but we let the degree of the spline vary.

..  admonition:: Jupyter Lab notebook

    `Zeros of a curated spline <https://splinekit.github.io/splinekit-jupyterlite/lab/?path=periodic-spline/sgn/spline_curated_zeros.ipynb&mode=single-document>`_

In the next notebook, we examine random splines of specified period, degree, and delay. Here, it is the values of the spline samples that we maintain across configurations.

..  admonition:: Jupyter Lab notebook

    `Zeros of a random spline <https://splinekit.github.io/splinekit-jupyterlite/lab/?path=periodic-spline/sgn/spline_zeros.ipynb&mode=single-document>`_
