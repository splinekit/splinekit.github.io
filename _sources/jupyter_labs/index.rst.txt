Jupyter Labs [toc]
==================

..  list-table::

    * - **Jupyter Labs**
      - Jupyter Lab notebooks with examples of use of the ``splinekit`` library.


The notebooks below are distributed in compressed format. Their execution relies on the availability of a workable ``Jupyter Lab`` environment and of an installed version of the ``splinekit`` library, along with its dependencies. To install everything in one shot, launch a terminal and issue the command::

    pip install numpy scipy sympy matplotlib jupyterlab ipywidgets splinekit


Module ``bsplines``
===================

:download:`bspline_bases <bspline_bases.ipynb.gz>`
Illustration of the cardinal B-spline basis :math:`\eta,` the dual B-spline
basis :math:`\mathring{\beta},` and the orthonormal B-spline basis :math:`\phi.`

:download:`bspline_interactive_shape <bspline_interactive_shape.ipynb.gz>`
Illustration of the B-spline basis :math:`\beta` and its variational forms
:math:`\dot{\beta}` and :math:`\int\beta,` one degree at a time.

:download:`bspline_inverse_sequence <bspline_inverse_sequence.ipynb.gz>`
Illustration of the B-spline inverse sequence :math:`\left(b^{-1}\right)` and
its periodized version.

:download:`bspline_m_scale <bspline_m_scale.ipynb.gz>`
Illustration of the multiresolution embedding of the B-spline bases :math:`\beta.`

:download:`bspline_numeric_stability <bspline_numeric_stability.ipynb.gz>`
Four approaches to the computation of the B-spline basis :math:`\beta,` and
discussion of their relative merits in terms of speed and numerical accuracy.

:download:`bspline_poles <bspline_poles.ipynb.gz>`
Visualization of the so-called B-spline poles and their relation to the annihilating sequences for B-splines.

:download:`bspline_random1d <bspline_random1d.ipynb.gz>`
Discussion of how splines differ from B-splines and display of a random one-dimensional spline that evolves dynamically.

:download:`bspline_random2d <bspline_random2d.ipynb.gz>`
A random two-dimensional spline curve that evolves dynamically.

:download:`bspline_static_shape <bspline_static_shape.ipynb.gz>`
Illustration of the B-spline basis :math:`\beta` and its variational forms
:math:`\dot{\beta}` and :math:`\int\beta,` several degrees jointly.

:download:`bspline_vs_exponentials <bspline_vs_exponentials.ipynb.gz>`
Relation of the B-spline basis :math:`\beta` to the :math:`\exp,` the
:math:`{\mathrm{Erf}},` and the Gaussian functions.

:download:`bspline_vs_pi <bspline_vs_pi.ipynb.gz>`
How B-splines :math:`\beta` lead to the mathematical constant :math:`\pi.`

:download:`bspline_vs_polynomials <bspline_vs_polynomials.ipynb.gz>`
How to build a monomial out of B-splines; which polynomial results from the sum of monomial-weighted integer-shifted B-splines; and convolution of a monomial with a B-spline.

:download:`bspline_vs_trigonometry <bspline_vs_trigonometry.ipynb.gz>`
Relation of the B-spline basis :math:`\beta` to the :math:`\sin` and
:math:`\cos` functions.

Module ``spline_padding``
=========================

:download:`padding <padding.ipynb.gz>`
How to extend a finite-length vector of data to a virtually infinite-length sequence.

Class ``PeriodicSpline1D``
==========================

:download:`periodicspline1d_bounds <periodicspline1d_bounds.ipynb.gz>`
How to obtain a periodic one-dimensional spline of arbitrary degree that bounds another spline, by above or by below.

:download:`periodicspline1d_creator <periodicspline1d_creator.ipynb.gz>`
How to create a periodic one-dimensional spline that interpolates data samples.

:download:`periodicspline1d_evaluate <periodicspline1d_evaluate.ipynb.gz>`
How to evaluate a periodic one-dimensional spline at just one argument or at a series of arguments.

:download:`periodicspline1d_polynomials <periodicspline1d_polynomials.ipynb.gz>`
How to access the polynomial pieces of a piecewise-polynomial periodic one-dimensional spline.

