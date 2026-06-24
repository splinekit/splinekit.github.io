Spline Statistics
=================

How to obtain the mean, the variance, and the image of a periodic one-dimensional spline.

----

Average *vs* Mean
-----------------

We take the average value of the function :math:`f:{\mathbb{R}}\rightarrow{\mathbb{R}},x\mapsto f(x)` over some set :math:`\Omega` to be the quantity :math:`\mu=\int_{\Omega}\,f(x)\,\frac{{\mathrm{d}}x}{\left|\Omega\right|},` where :math:`\left|\Omega\right|` is some measure of the size of :math:`\Omega.` To be sure, the precise definition of this measure would take us on a journey that would lead us far away from the familiar territory of calculus, so we shall assume for simplicity that :math:`\Omega` is but just one bounded and non-degenerate interval, and that :math:`f` is such that it can be integrated in the simplest of all ways, for instance as the limit of a Riemann sum.

There are cases where :math:`f` admits a favorable expression such that :math:`\mu` can be found explicitly, but these cases are few and far apart. Most functions show hostility to the treatment, and :math:`\mu` cannot be accessed through the tools of analysis. Even in such adverse cases, however, the Riemann sum still gives us an estimate of :math:`\mu` that is easily computed as :math:`\tilde{\mu}=\sum_{k'=0}^{N-1}\,f(k')\,\frac{1}{N},` with :math:`N` a positive integer. This surrogate computation demands that the interval :math:`\Omega=[0,N]` be partitioned into :math:`N` subintervals of unit length. (A linear change of variable over :math:`f` is all it takes to estimate the average over any arbitrary interval, with arbitrary rational length of the subintervals.)

An *average* is the name given to :math:`\mu` and a *mean* is the name given to :math:`\tilde{\mu}.` For finite :math:`N,` they differ for nearly every function one can think of; yet, remarkably, they do coincide in the cases where :math:`f` is a periodic polynomial spline of period :math:`K,` with

..  math::
        \int_{0}^{K}\,f(x)\,\frac{{\mathrm{d}}x}{K}=\mu=\tilde{\mu}=\sum_{k'=0}^{K-1}\,f(k')\,\frac{1}{K}

whenever :math:`f(x)=\sum_{k\in{\mathbb{Z}}}\,c[{k\bmod K}]\,\beta^{n}(x-\delta x-k),` where :math:`\left(c[k]\right)_{k=0}^{K-1}\in{\mathbb{R}}^{K}` is a :math:`K`-dimensional vector of spline coefficients and :math:`\beta^{n}` is a polynomial B-spline of nonnegative degree :math:`n\in{\mathbb{N}}.` Moreover, the equality between average and mean holds for any delay :math:`\delta x.`

Variance
--------

We take the variance of :math:`f` over :math:`\Omega` to be the quantity :math:`\sigma^{2}=\int_{\Omega}\,\left(f(x)-\mu\right)^{2}\,\frac{{\mathrm{d}}x}{\left|\Omega\right|}.` Whenever :math:`f` is a periodic spline, this variance can be computed exactly over one period. Even for splines, however, both the biased sample variance :math:`\sum_{k'=0}^{K-1}\,\left(f(k')-\tilde{\mu}\right)^{2}\,\frac{1}{K}` and the unbiased sample variance :math:`\sum_{k'=0}^{K-1}\,\left(f(k')-\tilde{\mu}\right)^{2}\,\frac{1}{K-1}` return only estimates of :math:`\sigma^{2}.`

Bounds
------

Consider the periodic spline :math:`f:{\mathbb{R}}\rightarrow{\mathbb{R}},x\mapsto f(x)=\sum_{k\in{\mathbb{Z}}}\,c[{k\bmod K}]\,\beta^{n}(x-\delta x -k),` along with its vector of samples at the integers :math:`\left(f(k')\right)_{k'=0}^{K-1}\in{\mathbb{R}}^{K}.` While it is easy enough to compute :math:`\tilde{f}_{\min}=\min_{k'=0}^{K-1}\,f(k')` and :math:`\tilde{f}_{\max}=\max_{k'=0}^{K-1}\,f(k'),` there is no guarantee that the spline does not undershoot :math:`\tilde{f}_{\min}` or overshoot :math:`\tilde{f}_{\max}` for spline degrees above :math:`1.` Even for linear splines, a guarantee would exist only for integer delays :math:`\delta x\in{\mathbb{Z}};` for non-integer delays, the guarantee disappears.

The ``splinekit`` library offers an ``image`` function that returns as an interval the enclosure of the image of the spline over the domain :math:`{\mathbb{R}}.` The infimum and the supremum of this interval are :math:`f_{\min}` and :math:`f_{\max},` respectively. While the computations are performed numerically and rely on the determination of the roots of polynomials of degree :math:`\left(n-1\right),` they nevertheless provide much more accurate and reliable estimates than :math:`\tilde{f}_{\min}` and :math:`\tilde{f}_{\max}` do.

----

Illustration
------------

We provide now a notebook where one can explore visually the notions we touched above. It allows for the creation of random splines, in the sense that their samples are independent identically distributed realizations of a random variable that follows some specified probability density function. We consider three of them.

*   *Uniform* The samples of the spline are drawn over some interval of values, without preference for any one value. The interval is chosen to ensure that the mean of the samples would be :math:`0` and that the sampled variance would be :math:`1` if there would be infinitely many samples.
*   *Gaussian* The samples of the spline are made to follow a Gaussian normal distribution. They show a tendency to cluster around :math:`0,` but large values can also be observed, albeit rarely.
*   *Cauchy* The samples of the spline take erratic values, so much so that it is known that neither their mean nor their variance converges to any definite value when the number of samples grows.

..  admonition:: Jupyter Lab notebook

    `Spline statistics <https://splinekit.github.io/splinekit-jupyterlite/lab/?path=periodic-spline/stats/spline_stats.ipynb&mode=single-document>`_
