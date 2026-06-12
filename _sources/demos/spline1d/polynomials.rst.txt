Polynomial Pieces of Splines
============================

How to access the polynomial pieces of a piecewise-polynomial periodic one-dimensional spline.

----

Roadmap
-------

Uniform polynomial splines are piecewise-polynomial functions. In what follows, we are going to provide a detailed derivation that returns the polynomial pieces. We start by defining B-splines and splines; we then propose a numerically stable way to evaluate splines. Then, we twist the evaluation procedure to get the expression of each polynomial piece.

Construction of the B-Spline
----------------------------

We first give the fundamental building blocks of uniform polynomial splines.

Polynomial Simple Element
^^^^^^^^^^^^^^^^^^^^^^^^^

Let :math:`n\in{\mathbb{N}}` be a nonnegative integer degree. Let :math:`x\in{\mathbb{R}}` be some argument. Define the polynomial simple element :math:`\varsigma^{n}` of degree :math:`n` (the degree being a superscript here, not a power) as the mapping :math:`\varsigma^{n}:{\mathbb{R}}\rightarrow{\mathbb{R}},x\mapsto\varsigma^{n}(x)` computed as

..  math::
        \varsigma^{n}(x)=\frac{1}{2\,n!}\,{\mathrm{sgn}}(x)\,x^{n}.

There, the signum function maps positive real numbers to :math:`+1,` negative real numbers to :math:`\left(-1\right),` and zero to :math:`0.`

Derivative of a Polynomial Simple Element
"""""""""""""""""""""""""""""""""""""""""

The :math:`m`-th derivative of a polynomial simple element of degree :math:`n,` with :math:`m\in[0\ldots n],` is

..  math::
        \frac{{\mathrm{d}}^{m}\varsigma^{n}(x)}{{\mathrm{d}}x^{m}}=\varsigma^{n-m}(x).

Derivatives of orders that exceed the degree can also be defined, albeit only in a distributional sense.

B-Spline
^^^^^^^^

We give now a bootstrap definition of the B-spline :math:`\beta^{n}` of nonnegative integer degree :math:`n\in{\mathbb{N}}` (the degree being a superscript here, not a power). It is the mapping :math:`\beta^{n}:{\mathbb{R}}\rightarrow{\mathbb{R}},x\mapsto\beta^{n}(x)` computed as

..  math::
        \beta^{n}(x)=\sum_{k=0}^{n+1}\,\left(-1\right)^{k}\,{n+1\choose k}\,\varsigma^{n}(x+\frac{n+1}{2}-k).

This function is made of polynomial pieces because of the term :math:`x^{n}` in the definition of :math:`\varsigma^{n}.` Moreover, the support of each piece has unit length, because the argument of :math:`\varsigma^{n}` in the definition of :math:`\beta^{n}` is shifted by integer :math:`k.`

In accord with the definition, the B-spline of degree zero is

..  math::
        \begin{array}{rcl}
        \beta^{0}(x)&=&\varsigma^{0}(x+\frac{1}{2})-\varsigma^{0}(x-\frac{1}{2})\\
        &=&\left\{\begin{array}{ll}1,&\left|x\right|<\frac{1}{2}\\\frac{1}{2},&\left|x\right|=\frac{1}{2}\\0,&\frac{1}{2}<\left|x\right|.\end{array}\right.
        \end{array}

Among many important properties, for all degrees :math:`n\in{\mathbb{N}}` and for all :math:`x\in{\mathbb{R}},` the B-splines thus defined

*   have a closed support, with :math:`{\mathrm{supp}}\{\beta^{n}\}=[-\left(n+1\right)/2,\left(n+1\right)/2];`
*   are even-symmetric, with :math:`\beta^{n}(x)=\beta^{n}(-x);`
*   satisfy the partition-of-unity condition :math:`\sum_{k\in{\mathbb{Z}}}\,\beta^{n}(x-k)=1.`

These properties hold pointwise true, also for :math:`n=0` and :math:`x=\pm1/2.`

Construction of a Spline
------------------------

Build now a weighted sum of integer shifts of B-splines, with the weights being provided by the so-called spline coefficients :math:`c.` For convenience, we shall restrict ourselves to periodized weights. Let us call this weighted sum a spline—without the B of B-spline. It is yet another mapping :math:`f:{\mathbb{R}}\rightarrow{\mathbb{R}},x\mapsto f(x);` it is computed as

..  math::
        f(x)=\sum_{k\in{\mathbb{Z}}}\,c[{k\bmod K}]\,\beta^{n}(x-\delta x-k),

where :math:`K\in{\mathbb{N}}+1` is its positive integer period, and where :math:`\delta x\in{\mathbb{R}}` is some global delay. The infinite sum is guaranteed to converge because B-splines have a closed support.

Evaluation of Splines
^^^^^^^^^^^^^^^^^^^^^

To evaluate a spline, one needs a procedure to evaluate B-splines. Unfortunately, the bootstrap formulation above reveals that a B-spline is inherently made of additive terms of alternating sign :math:`\left(-1\right)^{k},` which results in numerical difficulties. While we keep the bootstrap definition for splines of degree zero, we propose to alleviate the numerical difficulties for splines of positive degrees by computing :math:`f(x)` through the equivalent formulation

..  math::
        f(x)={\mathbf{c}}_{r}^{{\mathsf{T}}}\,{\mathbf{W}}^{n}\,{\mathbf{v}}^{n}(\chi).

This linear-algebra formulation takes its legitimation in the fact that the bootstrap definition of B-splines as well as the evaluation of splines proposed above are both finite sums of weighted terms.

Evaluation Matrix
"""""""""""""""""

Define the B-spline rational evaluation matrix :math:`{\mathbf{W}}^{n}\in{\mathbb{Q}}^{\left(n+1\right)\times\left(n+1\right)}` as having a rational component at its :math:`\left(r+1\right)`-th row and :math:`\left(c+1\right)`-th column given by

..  math::
        w_{r+1,c+1}^{n}=\frac{1}{c!}\,\left(\left.\frac{{\mathrm{d}}^{c}\beta^{n}(x)}{{\mathrm{d}}x^{c}}\right|_{x=\frac{n-1}{2}-r}+\frac{1}{2}\,\delta[c-n]\,\left(-1\right)^{n-r}\,{n+1\choose r+1}\right),

where :math:`\delta[\cdot]` is a discrete Dirac delta and where the B-spline derivatives are computed as in the bootstrap method. The B-spline rational evaluation matrix depends on :math:`n` only and can be precomputed. Since all terms are rationals, this can be done by taking advantage of Python's ``fractions`` module to perform exact computations. Once done, the matrix is cached as ``float`` numbers. In doing so, we let :math:`{\mathbf{W}}^{n}` carry the burden of the delicate balancing of positive and negative terms that was plaguing the bootstrap computation of B-splines.

Ancillary Variables
"""""""""""""""""""

Define

..  math::
        \xi=\frac{n-1}{2}-x+\delta x\in{\mathbb{R}}

..  math::
        r=\left\lceil\xi\right\rceil\in{\mathbb{Z}}

..  math::
        \chi=r-\xi\in[0,1)

Vector of Spline Coefficients
"""""""""""""""""""""""""""""

Define

..  math::
        {\mathbf{c}}_{r}=\left(c[{\left(k-r\right)\bmod K}]\right)_{k=0}^{n}\in{\mathbb{R}}^{n+1}

Vandermonde Vector
""""""""""""""""""

Define

..  math::
        {\mathbf{v}}^{n}(\chi)=(1,\left(\chi^{k}\right)_{k=1}^{n})\in{\mathbb{R}}^{n+1}

Spline Pieces
^^^^^^^^^^^^^

Although the freedom offered by the choice of the degree :math:`n,` the delay :math:`\delta x,` and the spline coefficients :math:`c` is large and allows one to tune :math:`f` in many ways, it remains that :math:`f` is not entirely arbitrary but inherits from :math:`\beta^{n}` the property of being necessarily a piecewise-polynomial function with unit-length pieces. Our purpose now is to establish the list of polynomials that is associated to a spline of a certain degree, delay, period, and spline coefficients. To do so, instead of considering the argument :math:`x` to be the free variable and instead of letting the ancillary variables depend on :math:`x` (as we did just above), we now change our perspective and let :math:`r\in[0\ldots K-1]` become a *free* index by which we are going to select each polynomial piece; moreover, we let :math:`\chi\in[0,1)` become a *free* variable, too. We shall take :math:`\chi` to be the argument of each polynomial piece. Under this alternate view of the evaluation of splines, where :math:`x` is *not* free anymore but depends on :math:`\{n,\delta x,r,\chi\},` we can establish that

..  math::
        {\mathbf{c}}_{r}^{{\mathsf{T}}}\,{\mathbf{W}}^{n}\,{\mathbf{v}}^{n}(\chi)=\left[\left({\mathbf{W}}^{n}\right)^{{\mathsf{T}}}\,{\mathbf{c}}_{r}\right]_{1}+\sum_{k=1}^{n}\,\left[\left({\mathbf{W}}^{n}\right)^{{\mathsf{T}}}\,{\mathbf{c}}_{r}\right]_{k+1}\,\chi^{k}

describes the polynomial of the free variable :math:`\chi\in[0,1),` indexed by :math:`r,` that coincides with the value of the spline over :math:`x\in[\frac{n-1}{2}+\delta x-q\,K-r,\frac{n+1}{2}+\delta x-q\,K-r),` where :math:`q\in{\mathbb{Z}}` is there to take the periodicity of the spline into account.

We now propose a few lines of code that create and display a random spline and extract its polynomial pieces.

..  admonition:: Jupyter Lab notebook

    `Polynomial pieces of a spline <https://splinekit.github.io/splinekit-jupyterlite/notebooks/index.html?path=spline_polynomials.ipynb>`_
