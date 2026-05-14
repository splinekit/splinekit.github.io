Upper and Lower Bounds of Splines
=================================

How to obtain a periodic one-dimensional spline of arbitrary degree that bounds another spline, by above or by below.

----

Upper Bound
-----------

Let a periodic uniform polynomial spline of positive degree :math:`n\in{\mathbb{N}}+1` be the mapping :math:`f:{\mathbb{R}}\rightarrow{\mathbb{R}},x\mapsto f(x)` computed as

..  math::
        f(x)=\sum_{k\in{\mathbb{Z}}}\,c[{k\bmod K}]\,\beta^{n}(x-\delta x-k).

There, :math:`K\in{\mathbb{N}}+1` is its positive integer period and :math:`\delta x\in{\mathbb{R}}` is a global delay. The coefficients :math:`c` are called the spline coefficients and are free ingredients that allow one to shape the spline. The function :math:`\beta^{n},` with :math:`n` a superscript (not a power), is a B-spline of degree :math:`n.` It has the closed support :math:`{\mathrm{supp}}\{\beta^{n}\}=[-\frac{n+1}{2},\frac{n+1}{2}]` and is nonnegative, with :math:`\forall x\in{\mathbb{R}}:\beta^{n}(x)\geq0.` It is also made of :math:`n+1` polynomial pieces of unit length.

Our purpose here is to build a spline :math:`g` of same period :math:`K` but arbitrary degree :math:`m\in{\mathbb{N}}+1,` with its delay :math:`\delta y` and spline coefficients :math:`u` chosen such that :math:`\forall x\in{\mathbb{R}}:g(x)\geq f(x);` moreover, we would like that the bounding spline :math:`g` be reasonably close to :math:`f` and that its determination be favorable, computationally. We provide now the main steps that lead to this goal. We first consider a local bound over a single interval of unit length; then, we merge the local bounds to obtain a global one.

Upper Bound over a Unit Interval
--------------------------------

Consider the periodized interval :math:`{\mathbb{X}}_{r}=\bigcup_{p\in{\mathbb{Z}}}\,[\frac{n-1}{2}+\delta x-r-p\,K,\frac{n-1}{2}+\delta x-r-p\,K+1),` indexed by :math:`r\in[0\ldots K-1].` Over this periodized unit-length interval, it turns out that the spline :math:`f` computed as above can also be computed from linear-algebra operations as

..  math::
        \forall x\in{\mathbb{X}}_{r}:f(x)={\mathbf{c}}_{r}^{{\mathsf{T}}}\,{\mathbf{W}}^{n}\,{\mathbf{v}}^{n}(\chi),

where

*   the vector :math:`{\mathbf{c}}_{r}\in{\mathbb{R}}^{n+1}` contains :math:`n+1` coefficients selected from the :math:`K` spline coefficients of :math:`f` as :math:`{\mathbf{c}}_{r}=\left(c[{\left(k-r\right)\bmod K}]\right)_{k=0}^{n};`
*   the (invertible) matrix :math:`{\mathbf{W}}^{n}\in{\mathbb{R}}^{\left(n+1\right)\times\left(n+1\right)}` depends on :math:`n` only and is precomputed.
*   the Vandermonde vector :math:`{\mathbf{v}}^{n}\in{\mathbb{R}}^{n+1}` has degree :math:`n` and is such that :math:`\forall\chi\in{\mathbb{R}}:{\mathbf{v}}^{n}(\chi)=(1,\left(\chi^{k}\right)_{k=1}^{n});`
*   the quantity :math:`\chi\in[0,1)` is defined as :math:`\chi={\mathrm{frac}}(x-\frac{n-1}{2}-\delta x),` with :math:`{\mathrm{frac}}(x)=\left(x-\left\lfloor x\right\rfloor\right).`

For convenience, let us moreover notate :math:`\delta y=\frac{n-m}{2}+\delta x,` :math:`{\mathbf{a}}=\left({\mathbf{W}}^{n}\right)^{{\mathsf{T}}}\,{\mathbf{c}}_{r},` and :math:`{\mathbf{u}}_{r}=\left({\mathbf{W}}^{m}\right)^{-{\mathsf{T}}}\,{\mathbf{h}}` for some vector :math:`{\mathbf{h}}` that will be specified on a case-by-case basis.

Bound of Higher Degree
^^^^^^^^^^^^^^^^^^^^^^

Assume for the moment that the degree :math:`m` of the bounding spline :math:`g` is such that :math:`m>n>0.` We want to discover :math:`{\mathbf{u}}_{r}\in{\mathbb{R}}^{m+1}` such that

..  math::
        \forall x\in{\mathbb{X}}_{r}:g(x)={\mathbf{u}}_{r}^{{\mathsf{T}}}\,{\mathbf{W}}^{m}\,{\mathbf{v}}^{m}({\mathrm{frac}}(x-\frac{m-1}{2}-\delta y))\geq{\mathbf{c}}_{r}^{{\mathsf{T}}}\,{\mathbf{W}}^{n}\,{\mathbf{v}}^{n}(\chi)=f(x).

We claim that

..  math::
        {\mathbf{h}}=\left({\mathbf{[\![}}k\leq n\,{\mathbf{]\!]}}\,a_{k+1}\right)_{k=0}^{m}

satisfies our requirements, where :math:`{\mathbf{[\![}}\cdot\,{\mathbf{]\!]}}` denotes the Iverson symbol. Indeed, some algebraic manipulations reveal that the difference :math:`\left(g(x)-f(x)\right)` vanishes over :math:`x\in{\mathbb{X}}_{r}.`

Bound of Same Degree
^^^^^^^^^^^^^^^^^^^^

Assume now that :math:`m=n>0.` Then, the trivial choice :math:`{\mathbf{u}}_{r}={\mathbf{c}}_{r}` results in :math:`\forall x\in{\mathbb{X}}_{r}:f(x)=g(x)\geq f(x).`

Bound of Smaller Degree
^^^^^^^^^^^^^^^^^^^^^^^

Assume finally that :math:`n>m>0.` In this case, letting :math:`{\mathbf{e}}_{m+1}` be the :math:`\left(m+1\right)`-th canonical basis, we claim that

..  math::
        {\mathbf{h}}=\left(a_{k+1}\right)_{k=0}^{m}+{\mathbf{e}}_{m+1}\,\sum_{k=m+1}^{n}\,\max(0,a_{k+1})

satisfies our requirements. Indeed, for all :math:`x\in{\mathbb{X}}_{r},` some algebraic manipulations lead to

..  math::
        \begin{array}{rcl}
        0&\leq&{\mathbf{u}}_{r}^{{\mathsf{T}}}\,{\mathbf{W}}^{m}\,{\mathbf{v}}^{m}({\mathrm{frac}}(x-\frac{m-1}{2}-\delta y))-{\mathbf{c}}_{r}^{{\mathsf{T}}}\,{\mathbf{W}}^{n}\,{\mathbf{v}}^{n}(\chi)\\
        &=&\chi^{m}\,\sum_{k=m+1}^{n}\,\left\{\begin{array}{ll}-a_{k+1}\,\chi^{k-m},&a_{k+1}<0\\a_{k+1}\,\left(1-\chi^{k-m}\right),&0\leq a_{k+1}.\end{array}\right.
        \end{array}

Global Bound
^^^^^^^^^^^^

We have found above a series of :math:`K` vectors :math:`{\mathbf{u}}_{r}` with :math:`r\in[0\ldots K-1].` From this series, we set

..  math::
        \forall k\in[0\ldots K-1]:u[k]=\max_{\rho=0}^{m}\,\left[{\mathbf{u}}_{{\left(\rho-k\right)\bmod K}}\right]_{\rho+1}.

Then, we define

..  math::
        \forall x\in{\mathbb{X}}_{r}:g(x)=\sum_{k=-r}^{m-r}\,u[{k\bmod K}]\,\beta^{m}(x-\delta y-k)

and observe that

..  math::
        \forall x\in{\mathbb{X}}_{r}:{\mathbf{u}}_{r}^{{\mathsf{T}}}\,{\mathbf{W}}^{m}\,{\mathbf{v}}^{m}(\chi)=\sum_{k=-r}^{m-r}\,\left[{\mathbf{u}}_{r}\right]_{k+r+1}\,\beta^{m}(x-\delta y-k).

The nonnegativity of B-splines allows us to finally establish that

..  math::
        \begin{array}{rcl}
        \forall x\in{\mathbb{X}}_{r}:g(x)-{\mathbf{u}}_{r}^{{\mathsf{T}}}\,{\mathbf{W}}^{m}\,{\mathbf{v}}^{m}(\chi)&=&\sum_{k=0}^{m}\,\left(\left(\max_{\rho=0}^{m}\,\left[{\mathbf{u}}_{{\left(\rho-k+r\right)\bmod K}}\right]_{\rho+1}\right)-\left[{\mathbf{u}}_{r}\right]_{k+1}\right)\,\beta^{m}(x-\delta y+r-k)\\
        &\geq&\sum_{k=0}^{m}\,\left(\left[{\mathbf{u}}_{r}\right]_{k+1}-\left[{\mathbf{u}}_{r}\right]_{k+1}\right)\,\beta^{m}(x-\delta y+r-k)=0,
        \end{array}

from which we conclude that :math:`g` is an upper bound of :math:`f.`

----

Generalizations
---------------

Up to now, we have considered positive degrees both for the spline to be bounded and for the bounding spline. With the same ideas, but without the formalism of linear algebra, bounds can also be established for nonnegative degrees. The class ``splinekit.PeriodSpline1D`` takes care of it all.

Given a method that builds an upper-bounding spline, a method to build a lower-bounding spline trivially arises if one considers the negated version of the upper-bounding spline of a negated spline.

In the following Jupyter Lab notebook, we show the upper-bounding and the lower-bounding splines of a random spline of some arbitrary period, degree, and delay.

..  admonition:: Jupyter Lab notebook

    `Bounding splines <https://splinekit.github.io/splinekit-jupyterlite/notebooks/index.html?path=spline_bounds.ipynb>`_
