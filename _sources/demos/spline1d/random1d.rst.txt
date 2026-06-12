Random One-Dimensional Splines
==============================

Display of a random one-dimensional spline that evolves dynamically.

----

Spline Real Functions
---------------------

One-dimensional uniform polynomial splines are functions that map a real number to a real number. They follow a recipe whereby they are made of the sum of integer-shifted and weighted synthesis functions called B-splines. The weights are numbers that parameterize the spline and give its versatility. The synthesis functions endow the spline with its unique characteristics—its smoothness, typically. Formally, we write a spline function :math:`f` as

..  math::
        f:{\mathbb{R}}\rightarrow{\mathbb{R}},\;x\mapsto f(x)=\sum_{k\in{\mathbb{Z}}}\,c[k]\,\beta^{n}(x-k).

There, :math:`c` are the weights; moreover, :math:`\beta^{n}:{\mathbb{R}}\rightarrow{\mathbb{R}}` is the polynomial B-spline, where the superscript (not the power) :math:`n\in{\mathbb{N}}` gives the nonnegative integer degree of the synthesis function. By linearity (sigma-linearity is not needed here because B-splines have a finite support), the spline :math:`f` inherits the properties of continuity and differentiability of the B-spline :math:`\beta^{n}.`

Let us now set :math:`n=3` and focus on the integer translates of a cubic B-spline. It turns out that the spline built out of them will be made of piecewise (cubic) polynomials that join in such a way that a general cubic spline can always be continuously differentiated twice (but not necessarily three times) and can always be (possibly discontinuously) differentiated thrice (but not necessarily four times); and this, for any arbitrary choice of coefficients.

Put simply, cubic splines are rather smooth.

In this formulation, the freedom of choice of the coefficients is what gives splines their plasticity and allows them to be used as valuable continuously defined models of data. In particular, the model comes in handy whenever one desires to evaluate the spline :math:`f` at some arbitrary coordinate :math:`x\in{\mathbb{R}},` with the purpose of accessing the value :math:`f(x).` Even more, the true gradient :math:`\dot{f}` and the true derivatives of higher order such as :math:`\{\ddot{f}, \dddot{f},\ldots\}` can also be accessed exactly, without having to resort to finite-difference approximations. This goes a long way to remove one layer of concern regarding the design of any algorithm that would rely on such differential quantities.

We give now a piece of code that animates a spline whose coefficients are drawn at random according to a normal Gaussian distribution, independently from one another.

..  admonition:: Jupyter Lab notebook

    `Random 1D spline <https://splinekit.github.io/splinekit-jupyterlite/notebooks/index.html?path=spline_random_1d.ipynb>`_
