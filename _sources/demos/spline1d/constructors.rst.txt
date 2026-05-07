Constructors of Periodic One-Dimensional Splines
================================================

How to create a periodic one-dimensional spline that interpolates data samples.

----

Periodic One-Dimensional Spline Objects
---------------------------------------

The class ``PeriodicSpline1D`` of the ``splinekit`` library handles continuously defined real functions :math:`f:{\mathbb{R}}\rightarrow{\mathbb{R}},x\mapsto f(x).` As maintained by the library, these functions have a few remarkable properties.

*   The function :math:`f` is periodic, with positive integer period :math:`K\in{\mathbb{N}}+1` such that :math:`\forall x\in{\mathbb{R}}:f(x)=f(x+K).`
*   The function :math:`f` is a uniform polynomial spline of nonnegative degree :math:`n\in{\mathbb{N}}` and shift :math:`\delta x\in{\mathbb{R}},` which we write as

..  math::
        f(x)=\sum_{k\in{\mathbb{Z}}}\,c[{k\bmod K}]\,\beta^{n}(x-\delta x -k).

There, :math:`{\mathbf{c}}` is a vector :math:`\left(c[k]\right)_{k=0}^{K-1}` of :math:`K` arbitrary coefficients and :math:`\beta^{n}` is a polynomial B-spline. (B-splines have a finite support and the infinite sum is necessarily well-behaved.) Because the function :math:`f` is a weighted sum of B-splines shifted by :math:`\delta x+k,` it inherits from these B-splines important properties such as :math:`n`-times differentiablility and :math:`\left(n-1\right)`-times continuous differentiablility. Moreover, because B-splines have the highest order of approximation for their support, the function :math:`f` is guaranteed to offer a faithful (in a mathematically precise sense) and computationally efficient representation of data. Finally, the determination of :math:`f(x)` at any :math:`x\in{\mathbb{R}}` happens in finitely many arithmetic steps.

Defining Parameters
^^^^^^^^^^^^^^^^^^^

The functions we consider are characterized by parameters that can be chosen freely and independently.

*   Period :math:`K`
*   Degree :math:`n`
*   Delay :math:`\delta x`
*   Spline coefficients :math:`{\mathbf{c}}`

A large variety of functions can be synthesized by the tuning of these parameters; in particular, by the tuning of the spline coefficients. However, it rarely occurs in practice that these coefficients be known beforehand. It is much more common that one is given a vector :math:`\left(y[q]\right)_{q=0}^{K-1}=\left(y_{q}\right)_{q=1}^{K}={\mathbf{y}}\in{\mathbb{R}}^{K}` of :math:`K` samples, and that it is desired that the synthesized spline interpolates the samples, a requirement that we write as

..  math::
        \left(f(q)\right)_{q=0}^{K-1}=\left(y[q]\right)_{q=0}^{K-1}.

Regularizations
^^^^^^^^^^^^^^^

Alternatively, it is also sometimes desirable that

..  math::
        \left(f(q)\right)_{q=0}^{K-1}\approx\left(y[q]\right)_{q=0}^{K-1},

with the approximation being such that it balances the requirement of interpolation with some *a priori* requirement on the continuously defined :math:`f.` The ``splinekit`` library offers two mechanisms that result in (desirable) approximate interpolation.

*   The first mechanism acknowleges the fact that exact interpolation cannot be achieved (for generic data) in the very specific case of an even period :math:`K\in2\,{\mathbb{N}}+2` combined with a half-integer delay :math:`\delta x\in{\mathbb{Z}}+1/2.` In this unstable edge case, stability is recovered by the addition of a constant corrective term to all even samples and by the subtraction of this term from all odd samples. The corrective term is chosen so that the residual difference between :math:`\left(f(q)\right)_{q=0}^{K-1}` and :math:`\left(y[q]\right)_{q=0}^{K-1}` is minimized in a discrete least-squares sense under the constraint that :math:`f` is well-defined. This mechanism is activated by setting ``regularized = True`` in the ``PeriodicSpline1D.from_samples`` constructor. It can be thought of as a doctoring of the samples that would enforce that the component at the highest discrete frequency of their discrete Fourier transform vanishes.
*   The second mechanism is made available in the ``PeriodicSpline1D.from_smoothed_samples`` constructor. It acknowledges the fact that the samples may exhibit a local variability that, sometimes, one is inclined to attribute to noise. In this case, the user can choose to restore smoothness to :math:`f` beyond the smoothness inherited by the B-splines, at some cost in the exactitude of interpolation. More precisely, if we let :math:`\left(\lambda[m]\right)_{m=0}^{n}` be a vector that represents the weight of variational regularization, then the criterion being minimized is

..  math::
        J=\sum_{k=0}^{K-1}\,\left(f(k)-y[k]\right)^{2}+\sum_{m=0}^{n}\,\lambda[m]\,\int_{0}^{K}\,\left(\frac{{\mathrm{d}}^{m}f(x)}{{\mathrm{d}}x^{m}}\right)^{2}\,{\mathrm{d}}x.

From Noiseless Samples
""""""""""""""""""""""

We now propose a few lines of code that create and display a spline that interpolates random samples.

..  admonition:: Jupyter Lab notebook

    `Interpolation <https://splinekit.github.io/splinekit-jupyterlite/notebooks/index.html?path=spline_interpolation.ipynb>`_

From Noisy Samples
""""""""""""""""""""""

We now propose a few lines of code that create and display a smooth spline that performs the approximate interpolation of random samples.

..  admonition:: Jupyter Lab notebook

    `Smoothed interpolation <https://splinekit.github.io/splinekit-jupyterlite/notebooks/index.html?path=spline_approximation.ipynb>`_
