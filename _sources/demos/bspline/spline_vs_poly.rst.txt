Splines, B-Splines, and Polynomials
===================================

How to build a monomial out of B-splines; which polynomial results from the sum of monomial-weighted integer-shifted B-splines; and convolution of a monomial with a B-spline.

----

Splines
-------

The ``splinekit`` library focuses on regular polynomial splines of nonnegative integer degree. These splines are real functions that map the domain :math:`{\mathbb{R}}` onto the image :math:`{\mathbb{R}}.`

*    *Piecewise Polynomials* What makes this particular mapping remarkable is that there exists some partition of the domain into intervals, each interval being such that, there, the piece of spline is a polynomial of a degree that never exceeds the degree of the spline—possibly, a different polynomial over each interval.

*    *Smoothness* Moreover, for the mapping to be called a spline, the polynomial pieces must match in the following sense: Consider three adjacent unit intervals of the partition of :math:`{\mathbb{R}}`, namely, a central interval with another interval to the left and another one to the right. Now, remember that an independent polyomial is associated to each of the three intervals. With splines, at the boundary between the left interval and the central one, the respective polynomials must take the same value and the same value of their derivatives of all orders except, possibly, the derivative of the order equal to the degree of the spline. The same goes at the central-right boundary.

*    *Regular* For regular splines, each interval is assumed to have a unit diameter.

Splines of degree :math:`n` are :math:`\left(n-1\right)` times continuously differentiable and :math:`n` times differentiable. They are the smoothest piecewise-polynomial functions one can get. It turns out that regular splines always admit a convenient representation through an expression that involves basis functions; in the most practical form, regular splines are written as

..  math::
        f:{\mathbb{R}}\rightarrow{\mathbb{R}},x\mapsto f(x)=\sum_{k\in{\mathbb{Z}}}\,c[k]\,\beta^{n}(x-\delta x-k),

where :math:`c` are the so-called spline coefficients and where the B-spline :math:`\beta^{n}` is the basis function of the degree :math:`n,` this degree being typeset as a superscript (not a power). The distinction between a B-spline (with a capital B) and a spline is that the first one is a nonparametric basis, the second one being parameterized by :math:`c` and the global delay :math:`\delta x.` To simplify the discussion, we assume henceforth that this delay is :math:`\delta x=0.`

In this description, there is nothing to prevent one to call a spline any true (non-piecewise) polynomial. We are going now to establish relations between polynomials and spline coefficients.

----

Polynomials
-----------

Polynomial of Degree :math:`\left(-1\right)`
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The real function that maps every real number to zero is the polynomial :math:`\pi_{-1}:{\mathbb{R}}\rightarrow{\mathbb{R}},x\mapsto\pi_{-1}(x)=0.` By convention, the polynomial :math:`\pi_{-1}` is said to be of degree :math:`\left(-1\right).` Trivially, it is also a spline since one can build it by letting :math:`c[k]=0` for all :math:`k\in{\mathbb{Z}}.` This is true for any spline degree; in what follows, we silently disregard this particular case and consider only polynomials (and monomials) of nonnegative degree.

Polynomials of Degree :math:`0` and Partition of Unity
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

A polynomial of degree :math:`0` is a real function :math:`\pi_{0}:{\mathbb{R}}\rightarrow{\mathbb{R}}\setminus\{0\},x\mapsto\pi_{0}(x)=a_{0}` that maps every real number to the nonzero constant :math:`a_{0}.` If we want to build a spline that does the same, we need to find a series :math:`c_{0}` of spline coefficients such that

..  math::
        a_{0}=\sum_{k\in{\mathbb{Z}}}\,c_{0}[k]\,\beta^{n}(x-k).

Fortunately, for any degree :math:`n` and for any argument :math:`x\in{\mathbb{R}},` B-splines satisfy the partition-of-unity condition according to which

..  math::
        1=\sum_{k\in{\mathbb{Z}}}\,\beta^{n}(x-k).

It is then enough to identify :math:`c_{0}[k]=a_{0}` for all :math:`k\in{\mathbb{Z}}` to ascertain that a polynomial of degree :math:`0` can indeed be represented by a spline.

Partition of Monomials
^^^^^^^^^^^^^^^^^^^^^^

The generic polynomial :math:`\pi_{n}:{\mathbb{R}}\rightarrow{\mathbb{R}},x\mapsto\pi_{n}(x)=a_{0}+\sum_{m=1}^{n}\,a_{m}\,x^{m}` is a weighted linear sum of a constant term :math:`1` and of canonic monomials :math:`x^{m},` with respective weights :math:`a_{0}` and :math:`a_{m}.` If we want to discover which series :math:`c` of spline coefficients is needed to represent :math:`\pi_{n}` as a spline, all we need to know is which series :math:`c_{m}^{n}` of spline coefficients will represent the monomial :math:`x^{m}` as the spline :math:`\sum_{k\in{\mathbb{Z}}}\,c_{m}^{n}[k]\,\beta^{n}(x-k),` assuming that :math:`0<m\leq n.` Then, the finite-support property of B-splines will alleviate concerns about the convergence of the sums involved and will allow us to write that

..  math::
        \begin{eqnarray}
        \pi_{n}(x)&=&a_{0}\,\left(\sum_{k\in{\mathbb{Z}}}\,\beta^{n}(x-k)\right)+\sum_{m=1}^{n}\,a_{m}\,\left(\sum_{k\in{\mathbb{Z}}}\,c_{m}^{n}[k]\,\beta^{n}(x-k)\right)\\
        &=&\sum_{k\in{\mathbb{Z}}}\,\left(a_{0}+\sum_{m=1}^{n}\,a_{m}\,c_{m}^{n}[k]\right)\,\beta^{n}(x-k).
        \end{eqnarray}

The identification :math:`c[k]=a_{0}+\sum_{m=1}^{n}\,a_{m}\,c_{m}^{n}[k]` will finally result in the desired representation :math:`\pi_{n}(x)=\sum_{k\in{\mathbb{Z}}}\,c[k]\,\beta^{n}(x-k).`

It turns out that :math:`c_{m}^{n}[k]` can itself be expressed as a polynomial in :math:`k.` We give now a piece of code that returns the list of polynomials in :math:`k` that one must use to weigh B-splines of degree :math:`n` to build a monomial of nonnegative degree :math:`m\leq n`, as in

..  math::
        x^{m}=\sum_{k\in{\mathbb{Z}}}\,c_{m}^{n}[k]\,\beta^{n}(x-k).

..  admonition:: Jupyter Lab notebook

    `Partition of monomials <https://splinekit.github.io/splinekit-jupyterlite/notebooks/index.html?path=bspline_partition_mono.ipynb>`_

Spline Coefficients Made of Discrete Monomials
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

As has been revealed just above, those spline coefficients that let a spline build a monomial have an expression that can be nontrivial. We now ask the converse question: What is the function reconstructed by a spline built with (trivial) monomial coefficients? The answer is a nontrivial polynomial of a degree equal to that of the monomial coefficients. Formally, we have that

..  math::
        \sum_{k\in{\mathbb{N}}\setminus\{0\}}\,\left(-k\right)^{0}\,\beta^{n}(x+k)+\beta^{n}(x)+\sum_{k\in{\mathbb{N}}\setminus\{0\}}\,k^{0}\,\beta^{n}(x-k)=1

for a weighting monomial of degree :math:`0,` and

..  math::
        \sum_{k\in{\mathbb{Z}}}\,k^{m}\,\beta^{n}(x-k)=\pi_{m}^{n}(x)

for :math:`m\in[1\ldots n],` where :math:`\pi_{m}^{n}(x)` is some polynomial in the free variable :math:`x.` We give now a piece of code that returns :math:`\pi_{m}^{n}(x)` for the spline degree :math:`n` and the nonnegative monomial degrees :math:`m\leq n.`

..  admonition:: Jupyter Lab notebook

    `Spline from monomials <https://splinekit.github.io/splinekit-jupyterlite/notebooks/index.html?path=bspline_spline_poly.ipynb>`_

----

Monomial Convolved with a B-Spline
----------------------------------

The convolution bewteen, on one hand, a monomial :math:`\left(\cdot\right)^{m}` of nonnegative integer degree :math:`m` and, on the other hand, a B-spline :math:`\beta^{n}` of nonnegative integer degree :math:`n,` turns out to be the polynomial :math:`\varpi_{n}^{m}` of degree :math:`m` defined by

..  math::
        \int_{\mathbb{R}}\,y^{m}\,\beta^{n}(x-y)\,{\mathrm{d}}y=\varpi_{m}^{n}(x).

We give now a piece of code that returns :math:`\varpi_{m}^{n}.`

..  admonition:: Jupyter Lab notebook

    `Monomial convolved with a B-spline <https://splinekit.github.io/splinekit-jupyterlite/notebooks/index.html?path=bspline_convolve_mono.ipynb>`_
