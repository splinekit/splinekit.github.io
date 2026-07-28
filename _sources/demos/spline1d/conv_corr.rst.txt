Convolutions and Correlations
=============================

How to convolve and correlate two piecewise-polynomial periodic one-dimensional splines.

----

Terminology
-----------

Notations
^^^^^^^^^

*   The operator :math:`*` indicates a convolution.
*   The operator :math:`\star` indicates a crosscorrelation.
*   The superscript :math:`\mbox{}^{*}` indicates a complex conjugate.
*   The superscript :math:`\mbox{}^{\vee}` indicates a mirror, with :math:`\forall x\in{\mathbb{R}}:f^{\vee}(x):=f(-x).`

Definitions
^^^^^^^^^^^

*   The mean value of a :math:`K`-periodic function :math:`f` is defined as :math:`{\mathrm{E}}\{f\}=\frac{1}{K}\,\int_{0}^{K}\,f(x)\,{\mathrm{d}}x.`
*   The variance of a :math:`K`-periodic function :math:`f` is defined as :math:`{\mathrm{Var}}\{f\}=\frac{1}{K}\,\int_{0}^{K}\,\left(f(x)-{\mathrm{E}}\{f\}\right)^{2}\,{\mathrm{d}}x.`

Convolutions
^^^^^^^^^^^^

Ordinary Convolution
""""""""""""""""""""

Let a one-dimensional function be :math:`h:{\mathbb{R}}\rightarrow{\mathbb{C}}` and let another one-dimensional function be :math:`f:{\mathbb{R}}\rightarrow{\mathbb{C}}.` Their *convolution* is defined as the function

..  math::
    g:{\mathbb{R}}\rightarrow{\mathbb{C}},x\mapsto g(x)=\left(h*f\right)(x):=\int_{-\infty}^{\infty}\,h(y)\,f(x-y)\,{\mathrm{d}}y.

Caution must be taken that the integration process does converge. For instance, it turns out that it does not converge when both :math:`h` and :math:`f` are periodic; thus, convolution must be defined differently in this particular case.

Periodic Convolution
""""""""""""""""""""

Let a one-dimensional :math:`K`-periodic function be :math:`h:{\mathbb{R}}\rightarrow{\mathbb{C}}` and let another one-dimensional :math:`K`-periodic function be :math:`f:{\mathbb{R}}\rightarrow{\mathbb{C}},` with :math:`K\in{\mathbb{N}}+1,` :math:`h(x)=h(x+K),` and :math:`f(x)=f(x+K)` for all :math:`x\in{\mathbb{R}}.` Their *periodic convolution* is defined as the function

..  math::
    g:{\mathbb{R}}\rightarrow{\mathbb{C}},x\mapsto g(x)=\left(h*f\right)(x):=\int_{0}^{K}\,h(y)\,f(x-y)\,{\mathrm{d}}y.

Caution must be taken that the integration process does converge; for simplicity, we shall henceforth restrict ourselves to periodic functions :math:`h` and :math:`f` that are nonsingular. Note that the notation for a convolution does not differ from that for a periodic convolution; both make use of the same operator :math:`*.` More often than not, however, the context allows one to easily distinguish between them.

*   The convolution operator is commutative, with :math:`h*f=f*h.`
*   The convolution operator is associative, with :math:`h_{1}*\left(h_{2}*f\right)=\left(h_{1}*h_{2}\right)*f.`
*   These properties apply to true functions. Proceed with caution upon their application to generalized functions; for instance, integration (the convolution with a unit step) and differentiation (the convolution with the derivative of a Dirac delta function) do not commute.

Correlations
^^^^^^^^^^^^

Crosscorrelation
""""""""""""""""

The *crosscorrelation* of :math:`f:{\mathbb{R}}\rightarrow{\mathbb{C}}` and :math:`g:{\mathbb{R}}\rightarrow{\mathbb{C}}` is defined as the function

..  math::
    R_{fg}:{\mathbb{R}}\rightarrow{\mathbb{C}},x\mapsto R_{fg}(x)=\left(f\star g\right)(x):=\int_{-\infty}^{\infty}\,f^{*}(y)\,g(x+y)\,{\mathrm{d}}y.

When the two functions are periodic of the same integer period :math:`K,` the same caveats apply as for their convolution. Their *periodic crosscorrelation* is the function

..  math::
    R_{fg}:{\mathbb{R}}\rightarrow{\mathbb{C}},x\mapsto R_{fg}(x)=\left(f\star g\right)(x):=\int_{0}^{K}\,f^{*}(y)\,g(x+y)\,{\mathrm{d}}y.

Note that one must pay attention to distinguish the convolution operator :math:`*` from the crosscorrelation operator :math:`\star.`

*   The crosscorrelation operator satisfies a conjugate-reflective symmetry, with :math:`f\star g=\left(g\star f\right)^{*\vee}.`
*   One has that :math:`f*g=f^{*\vee}\star g.` Equivalently, one has that :math:`f\star g=f^{*\vee}*g.`

Autocorrelation
"""""""""""""""

The crosscorrelation :math:`R_{ff}=f\star f` is called the autocorrelation of :math:`f.` By the Wiener-Khinchin theorem, the :math:`\nu`-th coefficient of the Fourier series of the autocorrelation of a :math:`K`-periodic real function :math:`f:{\mathbb{R}}\rightarrow{\mathbb{R}}` is related to the Fourier series of the function as :math:`F\{R_{ff}\}[\nu]=K\,\left|F\{f\}[\nu]\right|^{2}.`

Periodic Normalized Crosscorrelation
""""""""""""""""""""""""""""""""""""

Crosscorrelation measures how well :math:`f` matches :math:`g` in terms of their relative shift. Unfortunately, the measure is not easy to interpret because it depends on the units in which the functions are expressed. By contrast, the *normalized crosscorrelation* proposes a closely related measure that returns values in the interval :math:`[-1,1]` when the functions :math:`f:{\mathbb{R}}\rightarrow{\mathbb{R}}` and :math:`g:{\mathbb{R}}\rightarrow{\mathbb{R}}` are real. For periodic functions, the *periodic normalized crosscorrelation* is defined as the function

..  math::
    \rho_{fg}:{\mathbb{R}}\rightarrow{\mathbb{R}},x\mapsto \rho_{fg}(x)=\frac{1}{K}\,\int_{0}^{K}\,\frac{f(y)-E\{f\}}{\sqrt{{\mathrm{Var}}\{f\}}}\,\frac{g(x+y)}{\sqrt{{\mathrm{Var}}\{g\}}}\,{\mathrm{d}}y.

This definition returns exactly the same mapping as would two competing definitions, a first one in which no mean value would be subtracted from :math:`f(y)` but instead the mean value :math:`E\{g\}` would be subtracted from :math:`g(x+y),` and a second one in which appropriate mean values would be subtracted at the same time from :math:`f(y)` and from :math:`g(x+y).`

When compared to crosscorrelation, one downside of the normalized crosscorrelation is that it is not well-defined when :math:`{\mathrm{Var}}\{f\}\,{\mathrm{Var}}\{g\}=0.`

Pearson Correlation Coefficient
"""""""""""""""""""""""""""""""

The Pearson correlation coefficient is the normalized crosscorrelation evaluated at the origin, as :math:`\rho_{fg}(0).`

----

Convolution and Filtering of Splines
------------------------------------

The periodic convolution between a periodic :math:`h` and a periodic :math:`f` is best understood as the (non-periodic) convolution between the windowed version :math:`h_{[0,K)}` of :math:`h` and the periodic :math:`f.` Indeed, if we let :math:`{\mathbf{[\![}}\cdot\,{\mathbf{]\!]}}` denote the Iverson bracket, then it holds that

..  math::
    \begin{array}{rcl}
    \left(h*f\right)(x)&=&\int_{0}^{K}\,h(y)\,f(x-y)\,{\mathrm{d}}y\\
    &=&\int_{-\infty}^{\infty}\,\underbrace{{\mathbf{[\![}}0\leq y<K\,{\mathbf{]\!]}}\,h(y)}_{h_{[0,K)}(y)}\,f(x-y)\,{\mathrm{d}}y.
    \end{array}

In other words, the periodic convolution :math:`h*f` is identical to the non-periodic, classic filtering of the (still periodic) signal :math:`f` by a filter of finite, non-periodic causal impulse response :math:`h_{[0,K)}.`

Let :math:`h` be a periodic polynomial spline of period :math:`K,` degree :math:`n_{1},` and delay :math:`\delta x_{1}.` In addition, let :math:`f` be a periodic polynomial spline of period :math:`K,` degree :math:`n_{2},` and delay :math:`\delta x_{2}.` Then, :math:`h*f` turns out to be a spline of period :math:`K,` degree :math:`n_{1}+n_{2}+1,` and delay :math:`\delta x_{1}+\delta x_{2}.`

We now propose a few lines of code that create and display two random splines and convolves them.

*   The first spline plays the role of the filter with continuously defined impulse response :math:`h.` Its coefficients are built as the absolute value of independent realizations of a random variable that follows a Cauchy distribution; moreover, the spline is normalized in such a way that it has a unit integral over one period. This promotes lowpass filters; if chance has it, one of the Cauchy coefficients will dwarf the other ones and the impulse response will be unimodal, with a pronounced bulge.
*   The second spline plays the role of the signal :math:`f` being filtered. Its coefficients are built as independent realizations of a random variable that follows a Gaussian distribution.

..  admonition:: Jupyter Lab notebook

    `Convolution of periodic splines <https://splinekit.github.io/splinekit-jupyterlite/lab/?path=periodic-spline/conv_corr/spline_convolve.ipynb&mode=single-document>`_

----

Crosscorrelation of Splines
---------------------------

We also propose a few lines of code that create and display two random splines and compute their crosscorrelation, with or without normalization. The Pearson correlation coefficient is also printed. The spline coefficients of the functions been cross-correlated are built as independent realizations of a random variable that follows a Cauchy distribution.

..  admonition:: Jupyter Lab notebook

    `Crosscorrelation of periodic splines <https://splinekit.github.io/splinekit-jupyterlite/lab/?path=periodic-spline/conv_corr/spline_correlate.ipynb&mode=single-document>`_

----

Autocorrelation of a Spline
---------------------------

We finally propose a few lines of code that create and display a random spline :math:`f` and compute its autocorrelation, with or without normalization. The spline coefficients are built as independent realizations of a random variable that follows a uniform distribution.

It is noteworthy to make a few observations.

*   The autocorrelation (whether normalized or not) is independent of the delay of the spline.
*   The autocorrelation (whether normalized or not) has a central symmetry, with :math:`\rho_{ff}=\rho_{ff}^{\vee}` and :math:`R_{ff}=R_{ff}^{\vee}.` Combined with :math:`K`-periodicity, the central symmetry implies that the continuously defined autocorrelation is also necessarily symmetric around :math:`\frac{K}{2}.`
*   In general, the normalized autocorrelation takes a unit value at the origin (*i.e.*, :math:`\rho_{ff}(0)=1` unless :math:`{\mathrm{Var}}\{f\}=0`).

..  admonition:: Jupyter Lab notebook

    `Autocorrelation of a periodic spline <https://splinekit.github.io/splinekit-jupyterlite/lab/?path=periodic-spline/conv_corr/spline_autocorr.ipynb&mode=single-document>`_

