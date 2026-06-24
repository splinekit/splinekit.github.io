Plots of Periodic One-Dimensional Splines
=========================================

How to plot a periodic one-dimensional spline.

----

Periodic One-Dimensional Spline Objects
---------------------------------------

The objects maintained by the class ``PeriodicSpline1D`` of the ``splinekit`` library are parametric mathematical *functions* that map a number to another number. The input number being mapped is called the *argument* of the mapping and lives in a functional space called its *domain*. Here, it is the set of real numbers, approximated by ``float`` values. The output number lives in a functional space called the *image* (a.k.a. the range) of the mapping. Here, the image is again the set of real numbers, again approximated by ``float`` values. Collecting everything together, one notates the function :math:`f,` the domain :math:`{\mathbb{R}},` the image :math:`{\mathbb{R}},` and the argument :math:`x` as

..  math::
        f:{\mathbb{R}}\rightarrow{\mathbb{R}},x\mapsto f(x).

Plots
-----

Very often, it is desired that such functions be represented as a plot that illustrates how the arguments within some finite interval of the domain are mapped to numbers in some finite interval of the image. We now propose a piece of code that creates and displays a spline, with all the bells and whistles afforded by the ``splinekit`` library.

..  admonition:: Jupyter Lab notebook

    `Plots <https://splinekit.github.io/splinekit-jupyterlite/lab/?path=periodic-spline/plot/spline_plot.ipynb&mode=single-document>`_
