B-Spline Poles
==============

Visualization of the so-called B-spline poles and their relation to the annihilating sequences for B-splines.

----

Poles
-----

The so-called *poles* are the poles of the reciprocal of the z-transform of the sequence made of the samples at the integers of the polynomial B-spline :math:`\beta^{n}` of nonnegative integer degree :math:`n\in{\mathbb{N}}.` More precisely, poles are numbers :math:`z\in{\mathbb{C}}\setminus\{0\}` such that

..  math::
        \frac{1}{\sum_{k\in{\mathbb{Z}}}\,\beta^{n}(k)\,z^{-k}}\not\in{\mathbb{C}}.

It so happens that the B-spline poles are in fact real (their imaginary part is always zero), and negative; they exist for :math:`n\in{\mathbb{N}}+2.` Because B-splines are even-symmetric functions, their poles come in reciprocal pairs, the poles of interest being those that lie in the open interval :math:`(-1,0).` In this interval, :math:`\left\lfloor n/2\right\rfloor` of them can be found. They play an important role in spline interpolation, specifically in the process that converts data samples into spline coefficients.

----

Imbrication of Poles
--------------------

Poles of successive degrees are imbricated. We illustrate here this property for a few degrees.

..  admonition:: Jupyter Lab notebook

    `Imbrication of poles <https://splinekit.github.io/splinekit-jupyterlite/notebooks/index.html?path=bspline_poles.ipynb>`_

----

Annihilating Sequences
----------------------

Consider the real function

..  math::
        f:{\mathbb{R}}\rightarrow{\mathbb{R}},x\mapsto f(x)=\sum_{k\in{\mathbb{Z}}}\,c[k]\,\beta^{n}(x-k).

Every function :math:`f` built this way is said to be a spline. Remarkably, the sum always converges at any :math:`x\in{\mathbb{R}}` because B-splines :math:`\beta^{n}` have a finite support, so that the convergence happens for any sequence of finite-valued coefficients :math:`c,` without further restrictions. Now, for any :math:`k\in{\mathbb{Z}}` let :math:`c[k]=z_{n,m}^{k},` where :math:`z_{n,m}` is the :math:`m`-th pole associated to the B-spline of integer degree :math:`n\geq2,` with :math:`m\in[1\ldots\left\lfloor n/2\right\rfloor].` Then, from the fact that the sequence :math:`z_{n,m}^{k}` annihilates B-splines, it follows for any integer :math:`q\in{\mathbb{Z}}` that :math:`f(q)=\sum_{k\in{\mathbb{Z}}}\,z_{n,m}^{k}\,\beta^{n}(q-k)=z_{n,m}^{q}\,\sum_{k\in{\mathbb{Z}}}\,\beta^{n}(k)\,z_{n,m}^{-k}=0.` Likewise, the sequence :math:`z_{n,m}^{-k}` satisfies the same annihilating property because B-splines are even-symmetric.

One often deploys splines with the purpose of interpolating data samples. In practice, given a :math:`Q`-dimensional vector :math:`{\mathbf{y}}\in{\mathbb{R}}^{Q}` of samples, it is desired to find a specific sequence of coefficients :math:`c` that results in a spline :math:`f` satisfying the interpolation constraint :math:`\left(f(q)\right)_{q=0}^{Q-1}={\mathbf{y}}.` Unfortunately, the coefficients are not unique. Indeed, suppose that one has found a sequence :math:`c` that satisfies the interpolation constraint. Then, other sequences :math:`c'` can be established as

..  math::
        \begin{eqnarray}
        \forall q\in{\mathbb{Z}}:f(q)&=&\sum_{k\in{\mathbb{Z}}}\,c[k]\,\beta^{n}(q-k)\\
        &=&\sum_{k\in{\mathbb{Z}}}\,\underbrace{\left(c[k]+\sum_{m=1}^{\left\lfloor n/2\right\rfloor}\,\left(\lambda_{m}^{-}\,z_{n,m}^{-k}+\lambda_{m}^{+}\,z_{n,m}^{k}\right)\right)}_{c'[k]}\,\beta^{n}(q-k),
        \end{eqnarray}

where :math:`\lambda_{m}^{-}` and :math:`\lambda_{m}^{+}` are unconstrained real numbers which, altogether, provide :math:`2\,\left\lfloor n/2\right\rfloor` degrees of liberty.

We show now examples of nontrivial odd-symmetric and even-symmetric splines that interpolate the zero-valued data samples :math:`{\mathbf{y}}={\mathbf{0}}.` (We have retained contributions only of the pole closest to :math:`(-1)` for simplicity.) The samples at the integers of an original spline of corresponding degree will coincide with the samples of the sum of the original spline and any linear combination of those zero-interpolating splines.

..  admonition:: Jupyter Lab notebook

    `Annihilating sequences <https://splinekit.github.io/splinekit-jupyterlite/notebooks/index.html?path=bspline_annihilate.ipynb>`_

----

Pole Values
-----------

The ``splinekit`` library relies on precomputed poles stored in a cache for all degrees up to :math:`n=94.` If, for some reason, the cache is emptied, the library then relies on direct expressions for the determination of poles of degree :math:`n\in\{2,3,4,5,6,7,8,9\}.` For higher degrees, it relies on ``sympy`` to search poles in terms of roots of polynomials with rational coefficients. However, this evaluation is extremely costly timewise. For instance, on a 2021 desktop computer with libraries from 2025, the one-second threshold is exceeded at even degree :math:`22` and odd degree :math:`27,` the one-minute threshold is exceeded at even degree :math:`32` and odd degree :math:`35,` and the one-hour threshold is exceeded at even degree :math:`40` and odd degree :math:`45.`

Explicit Expressions
^^^^^^^^^^^^^^^^^^^^

Explicit expressions of the poles of B-splines of low degree are known. Why the Abel-Ruffini theorem states that it is not possible to express (in terms of radicals) the roots of arbitrary polynomial equations of high degree, one has to remember that poles do not result from arbitrary equations but from specific ones. Yet, no explicit expression is known for the poles associated to degrees beyond nonic.

Quadratic Pole
""""""""""""""

..  math::
        \left\{\begin{array}{rcl}0&=&\sum_{k=-1}^{1}\,\beta^{2}(k)\,z^{-k}\\z_{2,1}&=&-3+2\,\sqrt{2}\end{array}\right.

Cubic Pole
""""""""""

..  math::
        \left\{\begin{array}{rcl}0&=&\sum_{k=-1}^{1}\,\beta^{3}(k)\,z^{-k}\\z_{3,1}&=&-2+\sqrt{3}\end{array}\right.

Quartic Poles
"""""""""""""

..  math::
        \left\{\begin{array}{rcl}0&=&\sum_{k=-2}^{2}\,\beta^{4}(k)\,z^{-k}\\z_{4,1}&=&-19+\sqrt{304}+\sqrt{664-\sqrt{438976}}\\z_{4,2}&=&-19-\sqrt{304}+\sqrt{664+\sqrt{438976}}\end{array}\right.

Quintic Poles
"""""""""""""

..  math::
        \left\{\begin{array}{rcl}0&=&\sum_{k=-2}^{2}\,\beta^{5}(k)\,z^{-k}\\z_{5,1}&=&-\frac{13}{2}+\sqrt{\frac{105}{4}}+\sqrt{\frac{135}{2}-\sqrt{\frac{17745}{4}}}\\z_{5,2}&=&-\frac{13}{2}-\sqrt{\frac{105}{4}}+\sqrt{\frac{135}{2}+\sqrt{\frac{17745}{4}}}\end{array}\right.

Sextic Poles
""""""""""""

..  math::
        \left\{\begin{array}{rcl}0&=&\sum_{k=-3}^{3}\,\beta^{6}(k)\,z^{-k}\\\theta&=&\frac{1}{3}\arccos(-\frac{668791}{\sqrt{447872715451}})\\c&=&\sqrt{\frac{122416}{9}}\,\cos{\theta}\\s&=&\sqrt{\frac{122416}{3}}\,\sin{\theta}\\\lambda_{1}&=&\frac{361}{3}-2\,c\\\lambda_{2}&=&\frac{361}{3}+c-s\\\lambda_{3}&=&\frac{361}{3}+c+s\\z_{6,1}&=&\sqrt{\lambda_{1}^{2}-1}-\lambda_{1}\\z_{6,2}&=&\sqrt{\lambda_{2}^{2}-1}-\lambda_{2}\\z_{6,3}&=&\sqrt{\lambda_{3}^{2}-1}-\lambda_{3}\end{array}\right.

Septimic Poles
""""""""""""""

..  math::
        \left\{\begin{array}{rcl}0&=&\sum_{k=-3}^{3}\,\beta^{7}(k)\,z^{-k}\\\theta&=&\frac{1}{3}\arccos(-\frac{738}{\sqrt{556549}})\\c&=&\sqrt{301}\,\cos{\theta}\\s&=&\sqrt{903}\,\sin{\theta}\\\lambda_{1}&=&20-2\,c\\\lambda_{2}&=&20+c-s\\\lambda_{3}&=&20+c+s\\z_{7,1}&=&\sqrt{\lambda_{1}^{2}-1}-\lambda_{1}\\z_{7,2}&=&\sqrt{\lambda_{2}^{2}-1}-\lambda_{2}\\z_{7,3}&=&\sqrt{\lambda_{3}^{2}-1}-\lambda_{3}\end{array}\right.

Octic Poles
"""""""""""

..  math::
        \left\{\begin{array}{rcl}0&=&\sum_{k=-4}^{4}\,\beta^{8}(k)\,z^{-k}\\\theta&=&\frac{1}{3}\arccos\sqrt{\frac{3191438329707}{3435302785852}}\\\lambda_{0}&=&\sqrt{656944+\sqrt{106853376}\,\cos\theta}\\\lambda&=&\frac{1300071}{2}-\lambda_{0}^{2}\\\rho&=&1638\,\lambda_{0}-\sqrt{4\,\lambda^{2}-250737}\\\nu_{1}&=&\sqrt{\frac{2641593}{2}+\lambda-\rho}\\\nu_{2}&=&\sqrt{\frac{2641593}{2}+\lambda+\rho}\\\mu_{1}&=&-819+\lambda_{0}+\nu_{1}\\\mu_{2}&=&-819+\lambda_{0}-\nu_{1}\\\mu_{3}&=&-819-\lambda_{0}+\nu_{2}\\\mu_{4}&=&-819-\lambda_{0}-\nu_{2}\\z_{8,1}&=&\mu_{1}+\sqrt{\mu_{1}^{2}-1}\\z_{8,2}&=&\mu_{2}+\sqrt{\mu_{2}^{2}-1}\\z_{8,3}&=&\mu_{3}+\sqrt{\mu_{3}^{2}-1}\\z_{8,4}&=&\mu_{4}+\sqrt{\mu_{4}^{2}-1}\end{array}\right.

Nonic Poles
"""""""""""

..  math::
        \left\{\begin{array}{rcl}0&=&\sum_{k=-4}^{4}\,\beta^{9}(k)\,z^{-k}\\\theta&=&\frac{1}{3}\arccos\sqrt{\frac{607973645}{699281408}}\\\lambda_{0}&=&\sqrt{\frac{53265}{16}+\sqrt{146160}\,\cos\theta}\\\lambda&=&\frac{48397}{16}-\lambda_{0}^{2}\\\rho&=&\frac{251}{2}\,\lambda_{0}-\sqrt{4\,\lambda^{2}-7936}\\\nu_{1}&=&\sqrt{\frac{55699}{8}+\lambda-\rho}\\\nu_{2}&=&\sqrt{\frac{55699}{8}+\lambda+\rho}\\\mu_{1}&=&-\frac{251}{4}+\lambda_{0}+\nu_{1}\\\mu_{2}&=&-\frac{251}{4}+\lambda_{0}-\nu_{1}\\\mu_{3}&=&-\frac{251}{4}-\lambda_{0}+\nu_{2}\\\mu_{4}&=&-\frac{251}{4}-\lambda_{0}-\nu_{2}\\z_{9,1}&=&\mu_{1}+\sqrt{\mu_{1}^{2}-1}\\z_{9,2}&=&\mu_{2}+\sqrt{\mu_{2}^{2}-1}\\z_{9,3}&=&\mu_{3}+\sqrt{\mu_{3}^{2}-1}\\z_{9,4}&=&\mu_{4}+\sqrt{\mu_{4}^{2}-1}\end{array}\right.
