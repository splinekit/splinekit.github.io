Simple Operations on Periodic Splines
=====================================

How to add a delay, add a constant, multiply by a constant, negate, and mirror a one-dimensional spline.

----

Purpose
-------

Let :math:`f:{\mathbb{R}}\rightarrow{\mathbb{R}},x\mapsto f(x)` represent some polynomial periodic spline. It is a parametric real function that is characterized not only by spline coefficients that allow one to fit data, but also by overall parameters that encode its integer period, its integer degree, and its delay. What we want to do is to build another spline :math:`g` as a transformed version of :math:`f.` Here, we shall consider only trivial transformations.

Additional Delay
^^^^^^^^^^^^^^^^

The delay of :math:`f` can be manipulated so that :math:`g` is a translated version of :math:`f,` with :math:`g:{\mathbb{R}}\rightarrow{\mathbb{R}},x\mapsto g(x)=f(x+\lambda).` There, :math:`\lambda\in{\mathbb{R}}` is the additional delay. The instance function ``delayed_by`` can be used for that purpose.

Addition of a Constant
^^^^^^^^^^^^^^^^^^^^^^

The spline coefficients of :math:`f` can be manipulated so that :math:`g` is a value-offset version of :math:`f,` with :math:`g:{\mathbb{R}}\rightarrow{\mathbb{R}},x\mapsto g(x)=\lambda+f(x).` There, :math:`\lambda\in{\mathbb{R}}` is the constant value offset. The instance function ``plus`` can be used for that purpose.

Multiplication by a Constant
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The spline coefficients of :math:`f` can be manipulated so that :math:`g` is a value-scaled version of :math:`f,` with :math:`g:{\mathbb{R}}\rightarrow{\mathbb{R}},x\mapsto g(x)=\lambda\,f(x).` There, :math:`\lambda\in{\mathbb{R}}` is the constant value-scaling factor. The instance function ``times`` can be used for that purpose.

Negation
^^^^^^^^

The need may arise to have :math:`g` represent :math:`f` multiplied by the specific constant :math:`\lambda=\left(-1\right),` with :math:`g:{\mathbb{R}}\rightarrow{\mathbb{R}},x\mapsto g(x)=\left(-f(x)\right).` The instance function ``negated`` can be used for that purpose.

Mirroring
^^^^^^^^^

The spline coefficients of :math:`f,` along with its delay, can be manipulated so that :math:`g` is a mirrored version of :math:`f,` with :math:`g:{\mathbb{R}}\rightarrow{\mathbb{R}},x\mapsto g(x)=f(-x).` The instance function ``mirrored`` can be used for that purpose.


..  admonition:: Jupyter Lab notebook

    `Simple operations <https://splinekit.github.io/splinekit-jupyterlite/lab/?path=periodic-spline/ops/spline_simple_ops.ipynb&mode=single-document>`_
