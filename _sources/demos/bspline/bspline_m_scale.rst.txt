M-Scale Relation of B-Splines
=============================

Illustration of the multiresolution embedding of the B-spline :math:`\beta.`

----

A polynomial B-spline is a real function notated :math:`\beta^{n}:{\mathbb{R}}\rightarrow{\mathbb{R}},x\mapsto\beta^{n}(x),` where its nonnegative integer degree :math:`n\in{\mathbb{N}}` is typeset as a superscript (not a power). This function has many remarkable properties; here, we focus on multiresolution ones. In particular, it can be established that B-splines satisfy the M-scale equality

..  math::
        \beta^{n}({\color{blue}x})=\frac{1}{M^{n}}\,\sum_{k=0}^{\left(M-1\right)\,\left(n+1\right)}\,h^{n}_{M}[k]\,\beta^{n}({\color{blue}M\,x}+\frac{\left(M-1\right)\,\left(n+1\right)}{2}-k),

where :math:`M\in{\mathbb{N}}+1` is a positive integer that gives the scale parameter.

This equality, also called the M-scale relation, tells us that a B-spline at its nominal scale (the left-hand-side member) can be expressed as a finite weighted sum of shifted B-splines of same degree at scale :math:`M` (the right-hand-side member). Indeed, the free variable :math:`\color{blue}x` is multiplied by :math:`\color{blue}M` on the right, but not on the left. The weights are the coefficients :math:`h^{n}_{M}[k]/M^{n}` while the shifts :math:`(\frac{\left(M-1\right)\,\left(n+1\right)}{2}-k)` are integer-valued when at least one of :math:`M` or :math:`n` is odd.

The M-scale relation has numerous applications. It is invaluable in the development of algorithms that rely on a multiresolution approach to gradually establish the solution of certain problems. It also has fundamental links with wavelets, which allows for the decomposition of a signal into scale-related local contributions.

In the following Jupyter Lab notebook, we show the contribution of each term of the right-hand-side of the M-scale equality.

..  admonition:: Jupyter Lab notebook

    `M-scale relation of B-splines <https://splinekit.github.io/splinekit-jupyterlite/notebooks/index.html?path=bspline_m_scale.ipynb>`_
