Splines and Engineering Functions
=================================

Relation of the B-spline basis :math:`\beta` to :math:`\sin,` :math:`\cos,` :math:`\exp,` :math:`{\mathrm{erf}},` and the Gaussian function.


----

Sine
----

The alternating sum of the integer shifts of the first derivative of a B-spline approaches the sine function as the degree tends to infinity. More precisely, it holds that

..  math::
        \sin(\nu\,\pi)=-\frac{1}{4}\,\lim_{n\rightarrow\infty}\left(\frac{\pi}{2}\right)^{n}\,\sum_{k\in{\mathbb{Z}}}\,\left(-1\right)^{k}\,\dot{\beta}^{n}(\nu-k).

We give now a piece of code where we verify this property visually over the single period :math:`\nu\in[-1,1].`

..  admonition:: Jupyter Lab notebook

    `Sine from splines <https://splinekit.github.io/splinekit-jupyterlite/notebooks/index.html?path=bspline_vs_sin.ipynb>`_

----

Cosine
------

The alternating sum of the half-integer shifts of the first derivative of a B-spline approaches the cosine function as the degree tends to infinity. More precisely, it holds that

..  math::
        \cos(\nu\,\pi)=-\frac{1}{4}\,\lim_{n\rightarrow\infty}\left(\frac{\pi}{2}\right)^{n}\,\sum_{k\in{\mathbb{Z}}}\,\left(-1\right)^{k}\,\dot{\beta}^{n}(\nu+\frac{1}{2}-k).

We give now a piece of code where we verify this property visually over the single period :math:`\nu\in[-1,1].`

..  admonition:: Jupyter Lab notebook

    `Cosine from splines <https://splinekit.github.io/splinekit-jupyterlite/notebooks/index.html?path=bspline_vs_cos.ipynb>`_

----

*Interlude—Trigonometric Exercise*
----------------------------------

*To compute the spline approximation of the sine function, we have given above an explicit approach that relies on a sum of several evaluations of the* ``splinekit.grad_b_spline`` *function.  Conversely, to compute the spline approximation of the cosine function, we have given above an approach that relies on the higher-level* ``splinekit.PeriodSpline1D`` *class.*

*   *As first exercise, we propose that one computes the spline approximation of the sine by relying on the high-level class. (Hint: it is as simple as commenting out a single line of the spline approximation of the cosine.)*
*   *As second exercise, we propose that one computes the spline approximation of the cosine by relying on an explicit approach. The second exercise is more difficult that the first one.*

..  admonition:: Jupyter Lab notebook

    `Solutions of the trigonometric exercises <https://splinekit.github.io/splinekit-jupyterlite/notebooks/index.html?path=bspline_trigo_exo.ipynb>`_

----

Exponential
-----------

The exponential function can be approximated as a weighted sum of the derivatives of B-splines. More precisely, let an arbitrary order of differentiation be :math:`q\in{\mathbb{N}}` and let the degree of the B-spline be large enough to allow for :math:`q`-times differentiation, by setting it to be :math:`n=p+q` with :math:`p\in{\mathbb{N}}.` Then, it holds for :math:`x\in{\mathbb{R}}` that the B-spline :math:`\beta^{n}` is such that

..  math::
        {\mathrm{e}}^{x}=\frac{1}{2^{q+1}}\,\lim_{p\rightarrow\infty}a^{-p-1}\,\sum_{k\in{\mathbb{Z}}}\,\left(1+\sqrt{2}\right)^{k}\,\frac{{\mathrm{d}}^{q}\beta^{p+q}(\left(a\,x-k\right)/2)}{{\mathrm{d}}x^{q}},

where we have introduced the constant :math:`a=1/{\mathrm{arcsinh}}(1).`

We give now a piece of code where we verify this property visually over the range :math:`x\in[-2,2].`

..  admonition:: Jupyter Lab notebook

    `Exponential from splines <https://splinekit.github.io/splinekit-jupyterlite/notebooks/index.html?path=bspline_vs_exp.ipynb>`_

----

Gauss Error Function
--------------------

Polynomial B-splines are bump-like functions that converge to a Gaussian when the degree grows. More precisely, :math:`\forall x\in{\mathbb{R}}` it holds that

..  math::
        \frac{1}{\sigma_{0}\,\sqrt{2\,\pi}}\,{\mathrm{e}}^{-\frac{x^{2}}{2\,\sigma_{0}^{2}}}=\lim_{n\rightarrow\infty}\beta^{n}(x\,\sqrt{n+1})\,\sqrt{n+1},

where we have indroduced the constant :math:`\sigma_{0}=1/\sqrt{12}.` Therefore, it is without surprise that the integral of a spline converges to the :math:`{\mathrm{erf}}` function as the degree grows, too. Indeed, one has :math:`\forall x\in{\mathbb{R}}` that

..  math::
        {\mathrm{erf}}(x)=-1+2\,\lim_{n\rightarrow\infty}\int_{0}^{x\,\sqrt{\frac{n+1}{6}}}\,\beta^{n}(y)\,{\mathrm{d}}y.

We give now a piece of code where we verify this property visually over the range :math:`x\in[-3,3].`

..  admonition:: Jupyter Lab notebook

    `Erf from splines <https://splinekit.github.io/splinekit-jupyterlite/notebooks/index.html?path=bspline_vs_erf.ipynb>`_

----

*Interlude—Gaussian Exercise*
-----------------------------

*   *As exercise, we propose to modify the code of the* :math:`{\mathrm{erf}}` *case to verify visually that the B-spline of degree* :math:`n\in{\mathbb{N}}` *converges to a Gaussian when the degree grows. As icing on the cake, we suggest to perform the visualization over the support of the approximating B-spline.*

..  admonition:: Jupyter Lab notebook

    `Solution of the Gaussian exercises <https://splinekit.github.io/splinekit-jupyterlite/notebooks/index.html?path=bspline_gauss_exo.ipynb>`_


