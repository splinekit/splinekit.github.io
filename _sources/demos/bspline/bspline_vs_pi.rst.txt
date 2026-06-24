B-Spline *vs* :math:`\pi`
=========================

How B-splines :math:`\beta` lead to the mathematical constant :math:`\pi.`

----

Value of a B-Spline at the Origin
---------------------------------

Consider the B-spline :math:`\beta^{n}` of degree :math:`n` and let us focus on the value :math:`\beta^{n}(0)` that this function takes at the origin. Then, it turns out that

..  math::
        \pi=6\,\lim_{n\rightarrow\infty}\frac{1}{n+1}\,\left(\beta^{n}(0)\right)^{-2}.

In the following Jupyter Lab notebook, we plot a visual representation of that fact.

..  admonition:: Jupyter Lab notebook

    `Value of a B-spline at the origin <https://splinekit.github.io/splinekit-jupyterlite/lab/?path=bspline/vs-pi/bspline_vs_pi_at0.ipynb&mode=single-document>`_

----

Alternating Sum of Samples
--------------------------

Another relation between B-splines and :math:`\pi` arises if one computes the sum of all integer samples of B-splines, with an alternating sign. Then, it turns out that

..  math::
        \pi=2\,\lim_{n\rightarrow\infty}\left(\frac{1}{2}\,\sum_{k\in{\mathbb{Z}}}\,\left(-1\right)^{k}\,\beta^{n}(k)\right)^{-\frac{1}{n+1}}.

In the following Jupyter Lab notebook, we plot a visual representation of that fact.

..  admonition:: Jupyter Lab notebook

    `Alternating sum of samples <https://splinekit.github.io/splinekit-jupyterlite/lab/?path=bspline/vs-pi/bspline_vs_pi_as_sum.ipynb&mode=single-document>`_
