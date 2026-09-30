Addition
========

How to add two splines.

----

Brands of Splines
-----------------

This library handles splines that are at the same time specialized, and versatile. They are specialized for several reasons.

*   The splines of this library are one-dimensional functions that map a real number to another real number. There are other brands of splines that map a real number to a complex number. Yet other brands of splines map a multidimensional coordinate to a real number.
*   The splines of this library are piecewise polynomials. There are other brands of splines that are made of, for instance, ratios of polynomials, or exponential functions.
*   The splines of this library are made of polynomials of a nonnegative integer degree. There are other brands of splines where the degree is allowed to stray away from the integers. Yet other brands of splines allow degrees that are complex numbers.
*   The splines of this library are uniform, which means that the diameter of their pieces takes a constant value. Moreover, this diameter takes precisely a unit value. There are other brands of splines where each piece is allowed to have a different diameter.
*   The splines of this library are periodic. Moreover, their period is a positive integer number. There are other brands of splines that are unstructured, a category that encompasses quasi-periodic functions with irrational periods.

Despite being limited to one-dimensional integer-periodic uniform piecewise polynomials of nonnegative integer degree, the splines of this library have a lot of versatility.

*   Tensor products provide a convenient mechanism to handle multidimensional data. In particular, tensor-product splines are well-suited to the processing of images or volumetric Cartesian data.
*   The splines of this library are endowed with a mathematical property called the *order of approximation*. It guarantees that some objective measure of quality is met by the continuously defined function (the spline) that fills the gaps between discrete data (the finite number of bits used to specify the spline).
*   Beside the period, the degree, and the delay associated to the splines of this library, the spline coefficients make for a great many degrees of freedom in the specification of a spline. All these parameters can be chosen independently.

Unicity of the Representations
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Let a first spline :math:`f_{1}` be specified by its period :math:`K\in{\mathbb{N}}+1,` its degree :math:`n\in{\mathbb{N}},` its delay :math:`\delta x_{1}\in{\mathbb{R}},` and its spline coefficients :math:`\left(c_{1}[k]\right)_{k=0}^{K-1}.` It is the function

..  math::
    f_{1}:{\mathbb{R}}\rightarrow{\mathbb{R}},x\mapsto f_{1}(x)=\sum_{k\in{\mathbb{Z}}}\,c_{1}[{k\bmod K}]\,\beta^{n}(x-\delta x_{1}-k).

Let a second spline :math:`f_{2}` be

..  math::
    f_{2}:{\mathbb{R}}\rightarrow{\mathbb{R}},x\mapsto f_{2}(x)=\sum_{k\in{\mathbb{Z}}}\,c_{2}[{k\bmod K}]\,\beta^{n}(x-\delta x_{2}-k).

In general, it holds that :math:`\forall x\in{\mathbb{R}}:f_{1}(x)\neq f_{2}(x)` for vectors of coefficients that are not identical, or for delays that differ.

*   However, consider the situation when it jointly happens that :math:`\delta x_{2}=\delta x_{1}+k_{0}` and :math:`c_{2}[{\left(k-k_{0}\right)\bmod K}]=c_{1}[{k\bmod K}],` at indices :math:`k\in[0\ldots K-1]` and for any constant integer offset :math:`k_{0}\in{\mathbb{Z}}.` In this situation, :math:`f_{1}` is indistinguishable from  :math:`f_{2}.`
*   Likewise, :math:`f_{1}=f_{2}` for constant-valued coefficients :math:`c_{1}[k]=c_{0}=c_{2}[k]` for all :math:`k\in[0\ldots K-1],` irrespectively of any value that each of the delays :math:`\delta x_{1}` and :math:`\delta x_{2}` may take.

Addition of Compatible Splines
------------------------------

We want now to consider the function

..  math::
    f:{\mathbb{R}}\rightarrow{\mathbb{R}},x\mapsto f(x)=f_{1}(x)+f_{2}(x)

where, as above, the splines :math:`f_{1}` and :math:`f_{2}` have the same period and the same degree. Now, the spline :math:`f` can be handled by this library only if the support of the polynomial pieces of :math:`f_{1}` and :math:`f_{2}` do coincide, lest the property of the uniformity of :math:`f` be lost. To that effect, we ask that the condition :math:`\delta x_{1}=\delta x_{2}` be satisfied exactly. (As implied above by the investigation of the unicity of the representations, this condition is too strict; however, we retain it for simplicity.)

We propose here a few lines of code that create two random splines of specified identical period, degree, and delay, and then compute their sum.

..  admonition:: Jupyter Lab notebook

    `Addition of two compatible splines <https://splinekit.github.io/splinekit-jupyterlite/lab/?path=periodic-spline/addition/spline_add.ipynb&mode=single-document>`_

----

Addition of Splines of Identical Period
---------------------------------------

Let us relax some constraints and allow :math:`f_{1}` to be characterized by an independent degree :math:`n_{1},` an independent delay :math:`\delta x_{1},` and independent spline coefficients :math:`c_{1}.` Likewise, :math:`f_{2}` is characterized by an independent degree :math:`n_{2},` an independent delay :math:`\delta x_{2},` and independent spline coefficients :math:`c_{2}.` In the current context, the only property shared by :math:`f_{1}` and :math:`f_{2}` is their period :math:`K.`

The sum :math:`f_{1}+f_{2}` is a piecewise-polynomial spline of degree :math:`\min(n_{1},n_{2}),` albeit a nonuniform one. It therefore falls outside of the scope of this library. Nevertheless, we propose to determine the spline of arbitrary degree :math:`n` and arbitrary delay :math:`\delta x` that best represents :math:`f_{1}+f_{2}` in an integral least-squares sense, so that

..  math::
    f:{\mathbb{R}}\rightarrow{\mathbb{R}},x\mapsto f(x)=\sum_{k\in{\mathbb{Z}}}\,c[{k\bmod K}]\,\beta^{n}(x-\delta x-k)

has coefficients :math:`c` that minimize the criterion

..  math::
    J=\int_{0}^{K}\,\left(f(x)-\left(f_{1}(x)+f_{2}(x)\right)\right)^{2}\,{\mathrm{d}}x.

We propose now a few lines of code that create two random splines of specified independent degree and delay, and then compute the spline of arbitrary degree and delay that best represents their sum. All three splines have the same period.

..  admonition:: Jupyter Lab notebook

    `Addition of splines of identical period <https://splinekit.github.io/splinekit-jupyterlite/lab/?path=periodic-spline/addition/spline_add_prjct.ipynb&mode=single-document>`_
