Change of Basis
===============

Conversion from one basis to another.

----

Parametric Function
-------------------

Suppose we are given a sequence :math:`\left(c[k]\right)_{k\in{\mathbb{Z}}}` of coefficients, along with some real function :math:`\varphi:{\mathbb{R}}\rightarrow{\mathbb{R}},x\mapsto\varphi(x)` in the role of a synthesis function. Then, we may attempt to sum the weighted integer shifts of :math:`\varphi` to build the function :math:`f:{\mathbb{R}}\rightarrow{\mathbb{R}},x\mapsto f(x)=\sum_{k\in{\mathbb{Z}}}\,c[k]\,\varphi(x-k).` Our attempt is successful if the conditions of convergence and uniqueness of the parameters are met jointly.

*   *Convergence* The sum found in the expression of the parametric function :math:`f` ought to converge for any sequence :math:`c` of parameters.

*   *Uniqueness* The function :math:`f_{1},` built out of a first sequence :math:`c_{1}` of coefficients, and the function :math:`f_{2},` built out of the second sequence :math:`c_{2},` ought to be such that there exists at least one :math:`x\in{\mathbb{R}}` with :math:`f_{1}(x)\ne f_{2}(x)` whenever there exists at least one :math:`k\in{\mathbb{Z}}` with :math:`c_{1}[k]\ne c_{2}[k].`

Convergence and Uniqueness
--------------------------

The convergence of the one-sided infinite sum :math:`\sum_{k=1}^{\infty}\,s_{k}` of terms :math:`s_{k}` is notoriously fiddly. Many notions have been called for to capture what is meant by the word "convergence." For instance, a convergence can be qualified as conditional, unconditional, pointwise, almost uniform, almost everywhere, uniform, absolute, uniform-absolute (which, confusingly enough, is not the same as being at the same time uniform and absolute), among many other qualifiers. And then, there are quasi-convergent sums.

In our case, the form of the function :math:`f` features the double-sided infinite sum :math:`\sum_{k=-\infty}^{\infty}\,s[k]` of the terms :math:`s[k]=c[k]\,\varphi(x-k).` It has been found sensible to part such doubled-sided sums in pairs of one-sided sums to study separately their one-sided convergence; but even then, the parting can be made in several ways.

*   The principal-value approach considers either the limit :math:`s[0]+\lim_{K\rightarrow\infty}\sum_{k=1}^{K}\,\left(s[-k]+s[k]\right)` or the limit :math:`\lim_{K\rightarrow\infty}\sum_{k=-K}^{K}\,s[k],` which may not converge unconditionally, meaning that convergence may or may not happen, depending on the ordering of the terms.
*   The form :math:`\lim_{\left(K_{1},K_{2}\right)\rightarrow\left(\infty,\infty\right)}\sum_{k=-K_{1}}^{K_{2}}\,s[k],` with independence between :math:`K_{1}` and :math:`K_{2},` has links with uniform convergence.
*   The index-based parting considers the limits :math:`\lim_{K_{1}\rightarrow\infty}\sum_{k_{1}=-K_{1}}^{-1}\,s[k_{1}]+s[0]+\lim_{K_{2}\rightarrow\infty}\sum_{k_{2}=1}^{K_{2}}\,s[k_{2}].` It has links with uniform convergence, too.
*   The value-based parting :math:`\left(-\sup\{\left(-\sum_{k\in{\mathbb{A}}}\,s[k]\right)\in{\mathbb{R}}:{\mathbb{A}}\subseteq\{k\in{\mathbb{Z}}:s[k]<0\}\}\right.`:math:`\left.\mbox{}+\sup\{\left(\sum_{k\in{\mathbb{B}}}\,s[k]\right)\in{\mathbb{R}}:{\mathbb{B}}\subseteq\{k\in{\mathbb{Z}}:s[k]\geq0\}\}\right)` involves the notion of supremum over a set and has links with absolute convergence.

The various ways to part a double-sided infinite sum into a pair of one-sided infinite sums, combined with the various notions in which such one-sided sums may—or may not—exhibit a convergence of their own, make for a complicated state of affairs. However, when :math:`\varphi` is a polynomial B-spline :math:`\beta^{n}` of nonnegative integer degree :math:`n\in{\mathbb{Z}},` the situation is crystal-clear.

1.  Because of the finite support of :math:`\beta^{n},` the sum is finite and all worries about the convergence of double-sided infinite sums evaporate.
2.  Because :math:`\beta^{n}` is nonnegative and upper-bounded, the quantity :math:`f(x)` is well-defined for all :math:`x\in{\mathbb{R}}.`
3.  Because :math:`\{\beta^{n}(\cdot-k)\}_{k\in{\mathbb{Z}}}` turns out to be a so-called Riesz basis, uniqueness is guaranteed when the coefficients are square-summable.

Multiplicity of the Representations
-----------------------------------

The representation of :math:`f` is not unique. For instance, one can create a new sequence of coefficients by multiplying every given coefficient by some non-vanishing number; at the same time, one divides the synthesis function by the same number to create a new synthesis function. Together, the new coefficients and the new synthesis function will do the same job as the given ones. Another example in which we thwart uniqueness is to create yet another new sequence of coefficients by the addition of a constant integer offset to every index of the given sequence, and to compensate it by the creation of yet another synthesis function as an offset-shifted version of the original one.

The two proposed examples are trivial. A less trivial approach arises for those synthesis functions :math:`\varphi_{1}` that admit being themselves expressed as a sum of weighted and integer-shifted synthesis functions :math:`\varphi_{2},` as in

..  math::
        \varphi_{1}:{\mathbb{R}}\rightarrow{\mathbb{R}},x\mapsto \varphi_{1}(x)=\sum_{k'\in{\mathbb{Z}}}\,h[k']\,\varphi_{2}(x-k'),

where the sequence :math:`h` of weights relates the two synthesis functions. If, in addition, the technical conditions for the interchange of the order of summations allows it, then we can write that

..  math::
    \begin{array}{rcl}
    f(x)&=&\sum_{k\in{\mathbb{Z}}}\,c_{1}[k]\,\varphi_{1}(x-k)\\
    &=&\sum_{k\in{\mathbb{Z}}}\,c_{1}[k]\,\sum_{k'\in{\mathbb{Z}}}\,h[k']\,\varphi_{2}(x-k-k')\\
    &=&\sum_{k'\in{\mathbb{Z}}}\,\left(\sum_{k\in{\mathbb{Z}}}\,c_{1}[k]\,h[k'-k]\right)\,\varphi_{2}(x-k').
    \end{array}

Assuming that the discrete convolution :math:`c_{2}[k']=\sum_{k\in{\mathbb{Z}}}\,c_{1}[k]\,h[k'-k]` is well-defined for all :math:`k'\in{\mathbb{Z}},` we have thus established a new representation of :math:`f` in the synthesis function :math:`\varphi_{2},` with coefficients :math:`c_{2}.` We shall refer to this mechanism as a *change of basis*.

Available Bases
---------------

The `splinekit` library proposes four bases in which a given spline :math:`f:{\mathbb{R}}\rightarrow{\mathbb{R}},x\mapsto f(x)` can be expressed.

Basic
^^^^^

The B-spline synthesis function :math:`\beta^{n}` and its associated basis :math:`\{\beta^{n}(\cdot-k)\}_{k\in{\mathbb{Z}}}` provide the most natural way to express the given spline, as

..  math::
    f(x)=\sum_{k\in{\mathbb{Z}}}\,c[k]\,\beta^{n}(x-k).

There, the spline coefficients :math:`c` are those that are stored and maintained by the library.

Cardinal
^^^^^^^^

The cardinal B-spline synthesis function :math:`\eta^{n}` and its associated basis :math:`\{\eta^{n}(\cdot-k)\}_{k\in{\mathbb{Z}}}` can be used to express the given spline as

..  math::
    f(x)=\sum_{k\in{\mathbb{Z}}}\,f(k)\,\eta^{n}(x-k).

There, the spline samples :math:`\left(f(k)\right)_{k\in{\mathbb{Z}}}` parameterize the spline. It is a common occurence that one desires to interpolate a finite-dimensional vector :math:`{\mathbf{y}}=\left(y[k]\right)_{k=0}^{K-1}\in{\mathbb{R}}^{K}` of sampled data, in which case one imposes that :math:`f(k)=y[k]` for :math:`k\in[0\ldots K-1]` before to apply a change of basis from a cardinal representation to a basic representation. The corresponding change-of-basis algorithm deployed in this library is particularly efficient.

Dual
^^^^

The dual B-spline synthesis function :math:`\mathring{\beta}^{n,n}` and its associated basis :math:`\{\mathring{\beta}^{n,n}(\cdot-k)\}_{k\in{\mathbb{Z}}}` can be used to express the given spline as

..  math::
    f(x)=\sum_{k\in{\mathbb{Z}}}\,g[k]\,\mathring{\beta}^{n,n}(x-k)

in terms of the coefficients :math:`g.`

Orthonormal
^^^^^^^^^^^

The orthonormal B-spline synthesis function :math:`\phi^{n}` and its associated basis :math:`\{\phi^{n}(\cdot-k)\}_{k\in{\mathbb{Z}}}` can be used to express the given spline as

..  math::
    f(x)=\sum_{k\in{\mathbb{Z}}}\,a[k]\,\phi^{n}(x-k).

in terms of the coefficients :math:`a.`

Relation Between the Coefficients
---------------------------------

The coefficients :math:`\{c,f,g,a\}` are related as in the table below.

..  math::
    \scriptstyle
    \begin{array}{l|llll}\hline\hline
    \hfill\mathrm{From}&\mathrm{Basic}&\mathrm{Cardinal}&\mathrm{Dual}&\mathrm{Orthonormal}\\
    \mathrm{To}\\\hline
    \mathrm{Basic}&c&c=\left(b^{n}\right)^{-1}*f&c=\left(b^{2\,n+1}\right)^{-1}*g&c=\left(b^{2\,n+1}\right)^{-\frac{1}{2}}*a\\
    \mathrm{Cardinal}&f=b^{n}*c&f&f=b^{n}*\left(b^{2\,n+1}\right)^{-1}*g&f=b^{n}*\left(b^{2\,n+1}\right)^{-\frac{1}{2}}*a\\
    \mathrm{Dual}&g=b^{2\,n+1}*c&g=b^{2\,n+1}*\left(b^{n}\right)^{-1}*f&g&g=\left(b^{2\,n+1}\right)^{\frac{1}{2}}*a\\
    \mathrm{Orthonormal}&a=\left(b^{2\,n+1}\right)^{\frac{1}{2}}*c&a=\left(b^{2\,n+1}\right)^{\frac{1}{2}}*\left(b^{n}\right)^{-1}*f&a=\left(b^{2\,n+1}\right)^{-\frac{1}{2}}*g&a\\
    \hline\hline
    \end{array}

In the table, :math:`b^{n}` corresponds to the sequence :math:`\left(\beta^{n}(k)\right)_{k\in{\mathbb{Z}}}` of the integer samples of a B-spline of degree :math:`n.` This sequence has a finite support, with :math:`b^{n}[k]=0` for :math:`k\not\in[-\left\lfloor\frac{n}{2}\right\rfloor\ldots\left\lfloor\frac{n}{2}\right\rfloor].` In return, the support of the sequence :math:`\left(b^{n}\right)^{-1}` in infinite; the same goes for the supports of :math:`\left(\left(b^{n}\right)^{-\frac{1}{2}}\right)_{k\in{\mathbb{R}}}` and :math:`\left(\left(b^{n}\right)^{\frac{1}{2}}\right)_{k\in{\mathbb{R}}}.` In this library, those sequences of infinite support are handled algorithmically, without approximation.

Notebook
--------

We give now a piece of code that illustrates visually how a periodic random spline of a specified degree and period can be expressed in each one of the four bases.

..  admonition:: Jupyter Lab notebook

    `Change of basis <https://splinekit.github.io/splinekit-jupyterlite/notebooks/index.html?path=padding_bases.ipynb>`_

