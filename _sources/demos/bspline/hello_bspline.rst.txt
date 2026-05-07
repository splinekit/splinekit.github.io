Hello B-Spline Family!
======================

Illustration of the B-spline basis :math:`\beta` and its variational forms
:math:`\dot{\beta}` and :math:`\int\beta,` several degrees jointly.

----

A Family of B-Splines
---------------------

B-splines are functions that are even-symmetric and blob-shaped. In the Jupyter Lab notebook below, we plot the first ten members of the family of B-splines, as ordered by their degree.

..  admonition:: Jupyter Lab notebook

    `Hello B-spline <https://splinekit.github.io/splinekit-jupyterlite/notebooks/index.html?path=bspline_hello.ipynb>`_

----

Gradient
--------

B-splines of degree :math:`n` are :math:`n`-times differentiable, and continuously differentiable :math:`\left(n-1\right)` times. We plot in the next Jupyter Lab the gradient (*i.e.*, the first derivative) of the B-splines of degree :math:`1` to :math:`9.`

..  admonition:: Jupyter Lab notebook

    `Hello B-spline gradient <https://splinekit.github.io/splinekit-jupyterlite/notebooks/index.html?path=bspline_hello_grad.ipynb>`_

----

Integral
--------

B-splines are functions that have a finite :ref:`support<def-support>`. In their specific case, this means that these functions take the value zero for all arguments outside of some bounded interval. More precisely, :math:`\forall x\in{\mathbb{R}}\setminus[-\frac{n+1}{2},\frac{n+1}{2}]:\beta^{n}(x)=0` for :math:`n\in{\mathbb{N}}.` Moreover, B-splines never have any singularity. Consequently, their integral is always well-defined. We plot in the next Jupyter Lab the integral :math:`\int_{-\infty}^{x}\,\beta^{n}(y)\,{\mathrm{d}}y` of the B-splines of degree :math:`0` to :math:`9.`

..  admonition:: Jupyter Lab notebook

    `Hello B-spline integral <https://splinekit.github.io/splinekit-jupyterlite/notebooks/index.html?path=bspline_hello_integral.ipynb>`_
