Extrema
=======

How to access the minima and the maxima of a piecewise-polynomial periodic one-dimensional spline.

----

Minima and Maxima
-----------------

Given a periodic spline, we want to establish a set of intervals of largest diameter that give its extrema. We enforce that the diameter of the enclosure of all returned intervals is not larger than the period of this spline. For ease of use, the set is returned as a list of two lists. The first list contains the minima, and the second list contains the maxima.

We now propose a few lines of code that create and display a curated spline over which we illustrate and discuss several configurations, represented by a mixture of degenerate and proper intervals. In our control of the configurations, we retain the same spline coefficients but we let the degree of the spline vary.

..  admonition:: Jupyter Lab notebook

    `Extrema of a curated spline <https://splinekit.github.io/splinekit-jupyterlite/lab/?path=periodic-spline/extrema/spline_curated_xtrm.ipynb&mode=single-document>`_

In the next notebook, we examine random splines of specified period, degree, and delay. Here, it is the values of the spline samples that we maintain across configurations.

..  admonition:: Jupyter Lab notebook

    `Extrema of a random spline <https://splinekit.github.io/splinekit-jupyterlite/lab/?path=periodic-spline/extrema/spline_xtrm.ipynb&mode=single-document>`_
