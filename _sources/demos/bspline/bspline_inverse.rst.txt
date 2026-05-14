Inverse of B-Spline Sequences
=============================

Illustration of the B-spline inverse sequence :math:`\left(b^{-1}\right)` and
its periodized version.

----

Usefulness
----------

B-spline inverse sequences are a crucial ingredient of spline processing. They play a particularly important role when one desires to build a uniform spline that interpolates given discrete data. The ``splinekit`` library provides access to such sequences, both in non-periodic and periodic cases.

----

Non-Periodic Case
-----------------

B-splines are continuously defined functions :math:`\beta^{n}:{\mathbb{R}}\rightarrow{\mathbb{R}},` where the superscript :math:`n\in{\mathbb{N}}` is the nonnegative integer degree of a B-spline. Since B-splines map any real argument :math:`x\in{\mathbb{R}}` to :math:`\beta^{n}(x)\in{\mathbb{R}},` they can also map any *integer* argument :math:`k\in{\mathbb{Z}}` to :math:`\beta^{n}(k).` This corresponds to the integer samples of the B-spline and forms the sequence :math:`\left(b^{n}[k]\right)_{k\in{\mathbb{Z}}}=\left(\beta^{n}(k)\right)_{k\in{\mathbb{R}}\cap{\mathbb{Z}}}.` This sequence has a finite support; more precisely, it turns out that

..  math::
        \forall k\in{\mathbb{Z}}\setminus[-\left\lfloor\frac{n}{2}\right\rfloor\ldots\left\lfloor\frac{n}{2}\right\rfloor]:0=b^{n}[k].

One question that arises now is whether the sequence :math:`b^{n}` possesses a discrete-convolution inverse sequence. Let us notate such inverse sequence as :math:`\left(b^{n}\right)^{-1}.` What we are primarily asking is whether there exists :math:`\left(b^{n}\right)^{-1}` such that

..  math::
        \begin{array}{rcl}
        \forall k\in{\mathbb{Z}}:{\mathbf{[\![}}k=0\,{\mathbf{]\!]}}&=&\left(\left(b^{n}\right)^{-1}*b^{n}\right)[k]\\
        &=&\sum_{q\in{\mathbb{Z}}}\,\left(b^{n}\right)^{-1}[q]\,b^{n}[k-q],
        \end{array}

where the notation :math:`{\mathbf{[\![}}\cdot\,{\mathbf{]\!]}}` is that of the Iverson bracket. (From a mathematical point of view, let us observe that the convolution is always well-defined because of the finite-support property of :math:`b^{n}.`)

The answer to our primary question is yes. Indeed, for :math:`n>1` there are infinitely many such sequences, parameterized by :math:`2\,\left\lfloor\frac{n}{2}\right\rfloor` free numbers. Among all of them, however, only one has the finite energy :math:`\sum_{k\in{\mathbb{Z}}}\,\left(\left(b^{n}\right)^{-1}[k]\right)^{2}\in{\mathbb{R}}.` We show now this sequence for a few degrees.

..  admonition:: Jupyter Lab notebook

    `B-spline  non-periodic inverse sequence <https://splinekit.github.io/splinekit-jupyterlite/notebooks/index.html?path=bspline_inv.ipynb>`_

----

Periodic Case
-------------

Consider now that the convolution is a periodic one, with positive integer period :math:`K.` We're asking about the existence of a vector :math:`\left(\left(b_{K}^{n}\right)^{-1}[k]\right)_{k=0}^{K-1}` such that

..  math::
        \forall k\in[0\ldots K-1]:{\mathbf{[\![}}k=0\,{\mathbf{]\!]}}=\sum_{q=0}^{K-1}\,\left(b_{K}^{n}\right)^{-1}[q]\,b_{K}^{n}[{\left(k-q\right)\bmod K}],

where the periodization of :math:`b^{n}` defines the vector :math:`\left(b_{K}^{n}[k]\right)_{k=0}^{K-1}=\left(\sum_{p\in{\mathbb{Z}}}\,b^{n}[p\,K+k]\right)_{k=0}^{K-1}.` It can be verified that the periodized version of :math:`\left(b^{n}\right)^{-1}` described by the vector

..  math::
        \left(\left(b_{K}^{n}\right)^{-1}[k]\right)_{k=0}^{K-1}=\left(\sum_{p\in{\mathbb{Z}}}\,\left(b^{n}\right)^{-1}[p\,K+k]\right)_{k=0}^{K-1}

satisfies our periodic-convolution requirement. We show now this sequence for a few periods and degrees.

..  admonition:: Jupyter Lab notebook

    `B-spline  periodic inverse sequence <https://splinekit.github.io/splinekit-jupyterlite/notebooks/index.html?path=bspline_periodic_inv.ipynb>`_
