B-Spline Members
================

Illustration of the B-spline basis vector :math:`\beta` and its variational forms :math:`\dot{\beta}` and :math:`\int\beta,` one degree at a time.

----

Polynomial B-spline
-------------------

A polynomial B-spline is a function that is characterized by its degree, has a finite support, is even-symmetric and blob-shaped. Here, we plot a B-spline over its support, indexed by its degree.

..  admonition:: Jupyter Lab notebook

    `Polynomial B-spline <https://splinekit.github.io/splinekit-jupyterlite/lab/?path=bspline/members/bspline_member.ipynb&mode=single-document>`_

----

Polynomial B-Spline Derivatives
-------------------------------

B-spline polynomial functions are as smooth as one can get them to be. As they are made of pieces of polynomials between knots, they are infinitely differentiable over the interior of each piece. At the location of a knot, where two different polynomials meet, they are not infinitely differentiable anymore, but they are functions that are differentiable as many times as possible while honoring a property known as the partition of unity. Here, we plot a B-spline and all its finite derivatives.

..  admonition:: Jupyter Lab notebook

    `Polynomial B-spline derivatives <https://splinekit.github.io/splinekit-jupyterlite/lab/?path=bspline/members/bspline_member_diff.ipynb&mode=single-document>`_

----

Polynomial B-Spline Integral
----------------------------

The integral of a B-spline of some degree, from minus infinity to some upper limit, turns out to be a spline too, albeit one of a higher degree. Let us verify graphically another property of B-splines, namely, that they have a unit integral. Here, we plot the integral of a B-spline, letting the upper limit of integration span precisely the support of the B-spline.

..  admonition:: Jupyter Lab notebook

    `Polynomial B-spline integral <https://splinekit.github.io/splinekit-jupyterlite/lab/?path=bspline/members/bspline_member_integral.ipynb&mode=single-document>`_
