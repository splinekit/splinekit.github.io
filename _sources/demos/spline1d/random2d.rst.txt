Random Two-Dimensional Spline Curves
====================================

Display of a random two-dimensional spline curve that evolves dynamically.

----

2D Curve with 1D Spline Cartesian Components
--------------------------------------------

A generic one-dimensional real function is notated :math:`f:{\mathbb{R}}\rightarrow{\mathbb{R}}.` We want now to extend such a function by one extra dimension. There are two particularly simple ways to do so.

*   Extend the dimension of the functional *domain*, as in :math:`f:{\mathbb{R}}^{2}\rightarrow{\mathbb{R}}.` This approach will typically be followed to handle intensity images, with a two-dimensional coordinate (the position) being mapped to a grayscale value.
*   Extend the dimension of the functional *range*, as in :math:`{\mathbf{f}}:{\mathbb{R}}\rightarrow{\mathbb{R}}^{2}.` This approach will typically be followed to handle curves, with a one-dimensional curvilinear coordinate being mapped to a two-dimensional coordinate of the plane. This approach is the one we shall be following here.

Let us decide that we want to represent a curve of the plane by a two-component Cartesian coordinate, each component being itself a spline. If we let :math:`t` be the curvilinear coordinate, then the curve is

..  math::
        {\mathbf{f}}:{\mathbb{R}}\rightarrow{\mathbb{R}}^{2},\;t\mapsto{\mathbf{f}}(t)=\left(\begin{array}{cc}\sum_{k\in{\mathbb{Z}}}\,c_{1}[k]\,\beta^{n}(t-k)\\\sum_{k\in{\mathbb{Z}}}\,c_{2}[k]\,\beta^{n}(t-k)\end{array}\right).

There, the free spline coefficients :math:`c_{1}` parameterize the first component of the 2D coordinate while :math:`c_{2}` parameterize the second one; moreover, the function :math:`\beta^{n}` is the polynomial B-spline of degree :math:`n.`

About Smoothness
----------------

An interesting question that arises is as follows: Is the curve smooth? Certainly, being splines, its Cartesian components are! To answer in a practical way our question about the smoothness of the curve as a whole (as opposed to the smoothness of its Cartesian components in isolation), we provide below a piece of code that displays :math:`{\mathbf{f}}(t)` for :math:`t` over an evolving running window. There, we let the two series :math:`c_{1}` and :math:`c_{2}` of spline coefficients be independent samples of a random variable that follows a Gaussian normal distribution.

..  admonition:: Jupyter Lab notebook

    `Random 2D spline <https://splinekit.github.io/splinekit-jupyterlite/notebooks/index.html?path=spline_random_2d.ipynb>`_

Kinks
^^^^^

The plot makes apparent that there are surprisingly many locations where the curvature is very pronounced, so that the curve is possibly singular at such locations even though the components are never singular. Those places where curve singularities exist are called *kinks*; they emerge from the joint interplay between the gradients and the second-order derivatives of the two Cartesian components of the curve.

The osculating circle has a signed radius. When the radius is negative, the circle is on one side of the curve, and is on the other side when the radius is positive. Thus, there are three mechanisms at work to let the radius meet or cross the zero value and, possibly, give birth to kinks.

*   A case in which the radius vanishes happens when the curve spirals inwards to a limit point and then spirals outwards, *without* a change in the direction of rotation. This corresponds to a zero of even multiplicity in the function :math:`\rho` that maps the curvilinear coordinate :math:`t` to the radius :math:`\rho(t).`
*   It can also happen that :math:`\rho` has zeros of odd multiplicity, in which case the radius does change sign at those zeros. There, the osculating circle is transfered from one side of the curve to the other one, a typical situation in which kinks are observed, often in a configuration where the curve starts to spirals inwards, meets a reversal point, and spirals outwards *with* a change in the direction of rotation.
*   Finally, the radius also moves from one side of the curve to the other whenever :math:`\lim_{\tau\downarrow0}\rho(t-\tau)=\pm\infty` and :math:`\lim_{\tau\downarrow0}\rho(t+\tau)=\mp\infty.` In those cases, the curve straightens, and the osculating circle becomes a straight line at the location where :math:`\rho` is singular. No kink arises in this benign situation and the change of side of the osculating circle is graceful.

Finally, fake kinks are observed when the radius of the oscillating circle does not truly vanish, but just happens to become smaller than the threshold at which kinks are detected. This mostly corresponds to zeros of even multiplicity, without the zero value been actually met.
