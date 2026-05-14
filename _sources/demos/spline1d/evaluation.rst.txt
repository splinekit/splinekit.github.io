:orphan:

..  role:: raw-html(raw)
    :format: html

Evaluation of Splines
=====================

How to evaluate a periodic one-dimensional spline at just one argument or at a series of arguments.

----

Linear-Algebra Formulation
--------------------------

The obects that the class ``PeriodicSpline1D`` operates upon are mathematical functions and consist in descriptions of mappings :math:`f:{\mathbb{R}}\rightarrow{\mathbb{R}},x\mapsto f(x).` Oftentimes, the class allows one to obtain a new *mapping* from an existing one, for instance the gradient of a spline (not just a number but the whole function :math:`\dot{f}`) out of its original form :math:`f.` This is all good, but one is sometimes also interested in the more mundane goal of experimenting with a fixed mapping, typically to ask to what number :math:`f(x)\in{\mathbb{R}}` is the argument :math:`x\in{\mathbb{R}}` mapped to by :math:`f:{\mathbb{R}}\rightarrow{\mathbb{R}}.` The ``splinekit`` library  offers two possibilities to address this goal.

*   ``PeriodicSpline1D.at`` returns the value :math:`f(x)` of the spline :math:`f` evaluated at :math:`x.`
*   ``PeriodicSpline1D.get_samples`` returns an array of values at arguments separated by a constant step.

The two possibilities rely on the recipe followed by all one-dimensional uniform polynomial splines of nonnegative integer degree :math:`n\in{\mathbb{N}},` according to which an argument :math:`x` is mapped to the value

..  math::
        f(x)=\sum_{k\in{\mathbb{Z}}}\,c[{k\bmod K}]\,\beta^{n}(x-\delta x -k),

where :math:`K\in{\mathbb{N}}+1` is a positive integer period, :math:`c` is an arbitrary array of :math:`K` spline coefficients, :math:`\beta^{n}` is a B-spline whose degree :math:`n` is typeset in superscript (not a power), and :math:`\delta x\in{\mathbb{R}}` is an arbitrary delay. Put simply, a spline is a weighted sum of shifted B-splines.

Since the support of a B-spline is finite, the sum is finite at any given argument :math:`x.` Moreover, since B-splines of positive degrees are themselves made of unit-length pieces of polynomials, one can deploy the formalism of linear-algebra to make the expression of the spline recipe take the form

..  math::
        \begin{array}{rcl}
        \forall n\in{\mathbb{N}}+1,\forall x\in{\mathbb{R}}:f(x)&=&\left(\begin{array}{c}c[{\left(-r\right)\bmod K}]\\c[{\left(1-r\right)\bmod K}]\\c[{\left(2-r]\right)\bmod K}\\\vdots\\c[{\left(n-r\right)\bmod K}]\end{array}\right)^{{\mathsf{T}}}\,\left(\begin{array}{ccccc}w_{0,0}^{n}&w_{0,1}^{n}&w_{0,2}^{n}&\cdots&w_{0,n}^{n}\\w_{1,0}^{n}&w_{1,1}^{n}&w_{1,2}^{n}&\cdots&w_{1,n}^{n}\\w_{2,0}^{n}&w_{2,1}^{n}&w_{2,2}^{n}&\cdots&w_{2,n}^{n}\\\vdots&\vdots&\vdots&\ddots&\vdots\\w_{n,0}^{n}&w_{n,1}^{n}&w_{n,2}^{n}&\cdots&w_{n,n}^{n}\end{array}\right)\,\left(\begin{array}{c}1\\v\\v^{2}\\\vdots\\v^{n}\end{array}\right)\\
        &=&{\color{blue}{{\mathbf{c}}}}^{{\mathsf{T}}}\,{\color{blue}{{\mathbf{W}}^{n}}}\,{\color{blue}{{\mathbf{v}}^{n}}},
        \end{array}

where :math:`{\color{blue}{{\mathbf{c}}}}\in{\mathbb{R}}^{n+1}` is a vector whose :math:`n+1` components are extracted from the data-dependent array :math:`c` at some integer :math:`r\in{\mathbb{Z}}` that depends in discrete fashion on :math:`\left(x-\delta x\right),` where :math:`{\color{blue}{{\mathbf{W}}^{n}}}\in{\mathbb{R}}^{\left(n+1\right)\times\left(n+1\right)}` is a matrix that depends on :math:`n` only, and where :math:`{\color{blue}{{\mathbf{v}}^{n}}}\in{\mathbb{R}}^{n+1}` is a Vandermonde vector whose every component belongs to the interval :math:`[0,1]` and is made to depend continuously on :math:`\left(x-\delta x\right).`

----

Evaluation at a Single Argument
-------------------------------

To evaluate a one-dimensional periodic spline :math:`f` of positive degree :math:`n` at some argument :math:`x,` it is enough to compute :math:`{\mathbf{c}}^{{\mathsf{T}}}\,{\mathbf{W}}^{n}\,{\mathbf{v}}^{n}.` This is precisely what is done by the function call ``f.at(x)`` in the piece of code below, which otherwise generates and plots a random spline of some arbitrary degree and delay.

..  admonition:: Jupyter Lab notebook

    `Single argument <https://splinekit.github.io/splinekit-jupyterlite/notebooks/index.html?path=spline_at_scalar.ipynb>`_

----

Evaluation at Multiple Arguments
--------------------------------

We want now to evaluate :math:`f(x)` at the :math:`L\,M` arguments :math:`x\in\left\{x_{0}+k/M\right\}_{k=0}^{L\,M-1},` where :math:`M\in{\mathbb{N}}+1` is some positive integer oversampling factor, :math:`L\in{\mathbb{N}}+1` is the length of the support over which we want to sample :math:`f,` and :math:`x_{0}\in{\mathbb{R}}` is the argument of the first sample. To do so, we take advantage of ``PeriodicSpline1D.get_samples``. In the next piece of code, we illustrate how the combination of :math:`L,` :math:`M,` and :math:`x_{0}` can be used to specify the arguments where the spline is evaluated.

..  admonition:: Jupyter Lab notebook

    `Multiple arguments <https://splinekit.github.io/splinekit-jupyterlite/notebooks/index.html?path=spline_at_array.ipynb>`_

----

Experimental Performance
------------------------

When we evaluate a spline jointly at multiple arguments spaced regularly as above, it is judicious to pay attention on how the terms of :math:`{\mathbf{c}}^{{\mathsf{T}}}\,{\mathbf{W}}^{n}\,{\mathbf{v}}^{n}` depend on the variables of interest. In particular, when we compare the computation of :math:`f(x_{0}+q/M)` to that of :math:`f(x_{0}+\left(q+M\right)/M),` a detailed analysis reveals that the vector :math:`{\mathbf{c}}` changes, while the vector resulting from the product :math:`{\mathbf{W}}^{n}\,{\mathbf{v}}^{n}` is identical in the two cases. This invariance leads to computational savings.

To ascertain their benefit, we propose to time the determination of regularly spaced spline samples through either repeated calls to ``PeriodicSpline1D.at`` or through a combined call to ``PeriodicSpline1D.get_samples``. We report in degree-dependent tables by how many times the global call is faster than the repeated independent calls. For simplicity, we sample a whole period and set :math:`L=K,` letting the period :math:`K` explore some wide range. We let the oversampling factor :math:`M` take values in :math:`1` (unit-spaced samples) through :math:`6` (samples spaced by :math:`1/6`). Finally, to improve the robustness of the reported results, we repeat each timing experiment :math:`10` times.

..  danger::

    *   The link below allows you to inspect the notebook. Unfortunately, running it from the browser is meaningless: the timings of the *installation-free* version are not representative because the Python kernel is WebAssembly-based and does not run natively.

    *   If you want to test for realistic timings on your own computer, then you will have to first install in full the ``splinekit`` library. Only after that will you be able to launch the notebook either as a regular, full-fledged Jupyter Lab or as a module executed by the native Python kernel.

    *   The timings reported in the Results Section correspond to those of the native execution.

..  admonition:: Jupyter Lab notebook

    `Scalar vs array <https://splinekit.github.io/splinekit-jupyterlite/notebooks/index.html?path=spline_evaluation_speed.ipynb>`_

..  hint::
    The notebook is available for download in compressed form from
    :download:`here <spline_evaluation_speed.ipynb.gz>`. Decompression is achieved from the terminal with ``gunzip spline_evaluation_speed.ipynb.gz``.

Results
"""""""

On a desktop computer of 2021, typical resulting tables are as follows.

:raw-html:`<TABLE frame="hsides" rules="groups">
<CAPTION>Acceleration for Spline Degree <I>n</I> = 1</CAPTION>
<COLGROUP span="1">
<COLGROUP span="9">
<TR><TH colspan="1" align="right">Period&#160;<TH align="right">&#160;2&#160;<TH align="right">&#160;5&#160;<TH align="right">&#160;10&#160;<TH align="right">&#160;20&#160;<TH align="right">&#160;50&#160;<TH align="right">&#160;100&#160;<TH align="right">&#160;200&#160;<TH align="right">&#160;500&#160;<TH align="right">&#160;1000&#160;
<TR><TH colspan="1" align="left">Oversampling&#160;
<TBODY>
<TR><TD align="left"><I>M</I> = 1&#160;<TD align="right">&#160;1.00&#160;<TD align="right">&#160;2.13&#160;<TD align="right">&#160;3.15&#160;<TD align="right">&#160;4.41&#160;<TD align="right">&#160;3.96&#160;<TD align="right">&#160;5.20&#160;<TD align="right">&#160;5.36&#160;<TD align="right">&#160;5.64&#160;<TD align="right">&#160;4.72&#160;
<TR><TD align="left"><I>M</I> = 2&#160;<TD align="right">&#160;0.98&#160;<TD align="right">&#160;2.32&#160;<TD align="right">&#160;3.64&#160;<TD align="right">&#160;5.13&#160;<TD align="right">&#160;7.03&#160;<TD align="right">&#160;7.95&#160;<TD align="right">&#160;8.39&#160;<TD align="right">&#160;8.90&#160;<TD align="right">&#160;8.92&#160;
<TR><TD align="left"><I>M</I> = 3&#160;<TD align="right">&#160;1.43&#160;<TD align="right">&#160;2.93&#160;<TD align="right">&#160;4.68&#160;<TD align="right">&#160;6.73&#160;<TD align="right">&#160;9.83&#160;<TD align="right">&#160;11.17&#160;<TD align="right">&#160;12.68&#160;<TD align="right">&#160;13.36&#160;<TD align="right">&#160;13.62&#160;
<TR><TD align="left"><I>M</I> = 4&#160;<TD align="right">&#160;1.50&#160;<TD align="right">&#160;3.25&#160;<TD align="right">&#160;5.47&#160;<TD align="right">&#160;8.24&#160;<TD align="right">&#160;12.31&#160;<TD align="right">&#160;14.85&#160;<TD align="right">&#160;16.44&#160;<TD align="right">&#160;17.78&#160;<TD align="right">&#160;17.74&#160;
<TR><TD align="left"><I>M</I> = 5&#160;<TD align="right">&#160;1.74&#160;<TD align="right">&#160;3.54&#160;<TD align="right">&#160;6.01&#160;<TD align="right">&#160;9.39&#160;<TD align="right">&#160;14.61&#160;<TD align="right">&#160;17.77&#160;<TD align="right">&#160;20.19&#160;<TD align="right">&#160;21.71&#160;<TD align="right">&#160;22.38&#160;
<TR><TD align="left"><I>M</I> = 6&#160;<TD align="right">&#160;1.72&#160;<TD align="right">&#160;3.79&#160;<TD align="right">&#160;6.48&#160;<TD align="right">&#160;10.35&#160;<TD align="right">&#160;16.39&#160;<TD align="right">&#160;20.54&#160;<TD align="right">&#160;23.96&#160;<TD align="right">&#160;25.91&#160;<TD align="right">&#160;26.70&#160;
</TABLE>`

:raw-html:`<TABLE frame="hsides" rules="groups">
<CAPTION>Acceleration for Spline Degree <I>n</I> = 2</CAPTION>
<COLGROUP span="1">
<COLGROUP span="9">
<TR><TH colspan="1" align="right">Period&#160;<TH align="right">&#160;2&#160;<TH align="right">&#160;5&#160;<TH align="right">&#160;10&#160;<TH align="right">&#160;20&#160;<TH align="right">&#160;50&#160;<TH align="right">&#160;100&#160;<TH align="right">&#160;200&#160;<TH align="right">&#160;500&#160;<TH align="right">&#160;1000&#160;
<TR><TH colspan="1" align="left">Oversampling&#160;
<TBODY>
<TR><TD align="left"><I>M</I> = 1&#160;<TD align="right">&#160;1.16&#160;<TD align="right">&#160;2.09&#160;<TD align="right">&#160;3.01&#160;<TD align="right">&#160;3.91&#160;<TD align="right">&#160;4.78&#160;<TD align="right">&#160;5.28&#160;<TD align="right">&#160;5.56&#160;<TD align="right">&#160;5.66&#160;<TD align="right">&#160;5.68&#160;
<TR><TD align="left"><I>M</I> = 2&#160;<TD align="right">&#160;1.26&#160;<TD align="right">&#160;2.35&#160;<TD align="right">&#160;3.59&#160;<TD align="right">&#160;5.13&#160;<TD align="right">&#160;6.96&#160;<TD align="right">&#160;7.97&#160;<TD align="right">&#160;8.56&#160;<TD align="right">&#160;8.97&#160;<TD align="right">&#160;8.93&#160;
<TR><TD align="left"><I>M</I> = 3&#160;<TD align="right">&#160;1.50&#160;<TD align="right">&#160;2.94&#160;<TD align="right">&#160;4.64&#160;<TD align="right">&#160;6.81&#160;<TD align="right">&#160;9.83&#160;<TD align="right">&#160;11.46&#160;<TD align="right">&#160;12.86&#160;<TD align="right">&#160;13.34&#160;<TD align="right">&#160;13.57&#160;
<TR><TD align="left"><I>M</I> = 4&#160;<TD align="right">&#160;1.67&#160;<TD align="right">&#160;3.24&#160;<TD align="right">&#160;5.31&#160;<TD align="right">&#160;8.09&#160;<TD align="right">&#160;12.45&#160;<TD align="right">&#160;14.50&#160;<TD align="right">&#160;16.10&#160;<TD align="right">&#160;17.47&#160;<TD align="right">&#160;18.04&#160;
<TR><TD align="left"><I>M</I> = 5&#160;<TD align="right">&#160;1.74&#160;<TD align="right">&#160;3.57&#160;<TD align="right">&#160;5.87&#160;<TD align="right">&#160;9.19&#160;<TD align="right">&#160;14.50&#160;<TD align="right">&#160;17.83&#160;<TD align="right">&#160;20.38&#160;<TD align="right">&#160;21.80&#160;<TD align="right">&#160;22.51&#160;
<TR><TD align="left"><I>M</I> = 6&#160;<TD align="right">&#160;1.92&#160;<TD align="right">&#160;3.92&#160;<TD align="right">&#160;6.36&#160;<TD align="right">&#160;10.26&#160;<TD align="right">&#160;16.41&#160;<TD align="right">&#160;20.38&#160;<TD align="right">&#160;23.92&#160;<TD align="right">&#160;25.73&#160;<TD align="right">&#160;26.66&#160;
</TABLE>`

:raw-html:`<TABLE frame="hsides" rules="groups">
<CAPTION>Acceleration for Spline Degree <I>n</I> = 3</CAPTION>
<COLGROUP span="1">
<COLGROUP span="9">
<TR><TH colspan="1" align="right">Period&#160;<TH align="right">&#160;2&#160;<TH align="right">&#160;5&#160;<TH align="right">&#160;10&#160;<TH align="right">&#160;20&#160;<TH align="right">&#160;50&#160;<TH align="right">&#160;100&#160;<TH align="right">&#160;200&#160;<TH align="right">&#160;500&#160;<TH align="right">&#160;1000&#160;
<TR><TH colspan="1" align="left">Oversampling&#160;
<TBODY>
<TR><TD align="left"><I>M</I> = 1&#160;<TD align="right">&#160;1.18&#160;<TD align="right">&#160;1.96&#160;<TD align="right">&#160;2.86&#160;<TD align="right">&#160;3.81&#160;<TD align="right">&#160;4.79&#160;<TD align="right">&#160;5.25&#160;<TD align="right">&#160;5.54&#160;<TD align="right">&#160;5.61&#160;<TD align="right">&#160;5.67&#160;
<TR><TD align="left"><I>M</I> = 2&#160;<TD align="right">&#160;1.24&#160;<TD align="right">&#160;2.30&#160;<TD align="right">&#160;3.49&#160;<TD align="right">&#160;4.93&#160;<TD align="right">&#160;6.84&#160;<TD align="right">&#160;7.89&#160;<TD align="right">&#160;8.50&#160;<TD align="right">&#160;8.87&#160;<TD align="right">&#160;9.11&#160;
<TR><TD align="left"><I>M</I> = 3&#160;<TD align="right">&#160;1.55&#160;<TD align="right">&#160;2.99&#160;<TD align="right">&#160;4.58&#160;<TD align="right">&#160;6.67&#160;<TD align="right">&#160;9.76&#160;<TD align="right">&#160;11.47&#160;<TD align="right">&#160;13.11&#160;<TD align="right">&#160;13.23&#160;<TD align="right">&#160;13.66&#160;
<TR><TD align="left"><I>M</I> = 4&#160;<TD align="right">&#160;1.67&#160;<TD align="right">&#160;3.32&#160;<TD align="right">&#160;5.30&#160;<TD align="right">&#160;8.00&#160;<TD align="right">&#160;12.18&#160;<TD align="right">&#160;14.72&#160;<TD align="right">&#160;16.23&#160;<TD align="right">&#160;17.75&#160;<TD align="right">&#160;17.86&#160;
<TR><TD align="left"><I>M</I> = 5&#160;<TD align="right">&#160;1.82&#160;<TD align="right">&#160;3.61&#160;<TD align="right">&#160;5.92&#160;<TD align="right">&#160;9.12&#160;<TD align="right">&#160;14.26&#160;<TD align="right">&#160;17.32&#160;<TD align="right">&#160;20.27&#160;<TD align="right">&#160;21.66&#160;<TD align="right">&#160;22.68&#160;
<TR><TD align="left"><I>M</I> = 6&#160;<TD align="right">&#160;1.91&#160;<TD align="right">&#160;3.89&#160;<TD align="right">&#160;6.39&#160;<TD align="right">&#160;10.03&#160;<TD align="right">&#160;16.05&#160;<TD align="right">&#160;20.47&#160;<TD align="right">&#160;23.82&#160;<TD align="right">&#160;25.92&#160;<TD align="right">&#160;26.19&#160;
</TABLE>`

:raw-html:`<TABLE frame="hsides" rules="groups">
<CAPTION>Acceleration for Spline Degree <I>n</I> = 4</CAPTION>
<COLGROUP span="1">
<COLGROUP span="9">
<TR><TH colspan="1" align="right">Period&#160;<TH align="right">&#160;2&#160;<TH align="right">&#160;5&#160;<TH align="right">&#160;10&#160;<TH align="right">&#160;20&#160;<TH align="right">&#160;50&#160;<TH align="right">&#160;100&#160;<TH align="right">&#160;200&#160;<TH align="right">&#160;500&#160;<TH align="right">&#160;1000&#160;
<TR><TH colspan="1" align="left">Oversampling&#160;
<TBODY>
<TR><TD align="left"><I>M</I> = 1&#160;<TD align="right">&#160;1.19&#160;<TD align="right">&#160;1.93&#160;<TD align="right">&#160;2.77&#160;<TD align="right">&#160;3.58&#160;<TD align="right">&#160;4.63&#160;<TD align="right">&#160;5.14&#160;<TD align="right">&#160;5.49&#160;<TD align="right">&#160;5.61&#160;<TD align="right">&#160;5.68&#160;
<TR><TD align="left"><I>M</I> = 2&#160;<TD align="right">&#160;1.26&#160;<TD align="right">&#160;2.27&#160;<TD align="right">&#160;3.50&#160;<TD align="right">&#160;4.90&#160;<TD align="right">&#160;6.78&#160;<TD align="right">&#160;7.80&#160;<TD align="right">&#160;8.60&#160;<TD align="right">&#160;8.87&#160;<TD align="right">&#160;9.20&#160;
<TR><TD align="left"><I>M</I> = 3&#160;<TD align="right">&#160;1.52&#160;<TD align="right">&#160;2.96&#160;<TD align="right">&#160;4.54&#160;<TD align="right">&#160;6.53&#160;<TD align="right">&#160;9.67&#160;<TD align="right">&#160;11.36&#160;<TD align="right">&#160;12.44&#160;<TD align="right">&#160;13.28&#160;<TD align="right">&#160;13.43&#160;
<TR><TD align="left"><I>M</I> = 4&#160;<TD align="right">&#160;1.74&#160;<TD align="right">&#160;3.33&#160;<TD align="right">&#160;5.27&#160;<TD align="right">&#160;7.86&#160;<TD align="right">&#160;11.82&#160;<TD align="right">&#160;14.51&#160;<TD align="right">&#160;16.59&#160;<TD align="right">&#160;17.88&#160;<TD align="right">&#160;17.94&#160;
<TR><TD align="left"><I>M</I> = 5&#160;<TD align="right">&#160;1.77&#160;<TD align="right">&#160;3.78&#160;<TD align="right">&#160;5.92&#160;<TD align="right">&#160;9.04&#160;<TD align="right">&#160;14.23&#160;<TD align="right">&#160;17.09&#160;<TD align="right">&#160;19.86&#160;<TD align="right">&#160;21.93&#160;<TD align="right">&#160;22.42&#160;
<TR><TD align="left"><I>M</I> = 6&#160;<TD align="right">&#160;1.85&#160;<TD align="right">&#160;4.00&#160;<TD align="right">&#160;6.36&#160;<TD align="right">&#160;9.87&#160;<TD align="right">&#160;15.98&#160;<TD align="right">&#160;20.05&#160;<TD align="right">&#160;23.92&#160;<TD align="right">&#160;25.98&#160;<TD align="right">&#160;26.25&#160;
</TABLE>`

:raw-html:`<TABLE frame="hsides" rules="groups">
<CAPTION>Acceleration for Spline Degree <I>n</I> = 5</CAPTION>
<COLGROUP span="1">
<COLGROUP span="9">
<TR><TH colspan="1" align="right">Period&#160;<TH align="right">&#160;2&#160;<TH align="right">&#160;5&#160;<TH align="right">&#160;10&#160;<TH align="right">&#160;20&#160;<TH align="right">&#160;50&#160;<TH align="right">&#160;100&#160;<TH align="right">&#160;200&#160;<TH align="right">&#160;500&#160;<TH align="right">&#160;1000&#160;
<TR><TH colspan="1" align="left">Oversampling&#160;
<TBODY>
<TR><TD align="left"><I>M</I> = 1&#160;<TD align="right">&#160;1.21&#160;<TD align="right">&#160;1.86&#160;<TD align="right">&#160;2.61&#160;<TD align="right">&#160;3.50&#160;<TD align="right">&#160;4.55&#160;<TD align="right">&#160;5.12&#160;<TD align="right">&#160;5.53&#160;<TD align="right">&#160;5.56&#160;<TD align="right">&#160;5.69&#160;
<TR><TD align="left"><I>M</I> = 2&#160;<TD align="right">&#160;1.23&#160;<TD align="right">&#160;2.40&#160;<TD align="right">&#160;3.33&#160;<TD align="right">&#160;4.65&#160;<TD align="right">&#160;6.58&#160;<TD align="right">&#160;7.74&#160;<TD align="right">&#160;8.51&#160;<TD align="right">&#160;8.85&#160;<TD align="right">&#160;9.05&#160;
<TR><TD align="left"><I>M</I> = 3&#160;<TD align="right">&#160;1.49&#160;<TD align="right">&#160;3.02&#160;<TD align="right">&#160;4.32&#160;<TD align="right">&#160;6.41&#160;<TD align="right">&#160;9.57&#160;<TD align="right">&#160;10.90&#160;<TD align="right">&#160;12.31&#160;<TD align="right">&#160;13.21&#160;<TD align="right">&#160;13.43&#160;
<TR><TD align="left"><I>M</I> = 4&#160;<TD align="right">&#160;1.71&#160;<TD align="right">&#160;3.51&#160;<TD align="right">&#160;5.14&#160;<TD align="right">&#160;7.66&#160;<TD align="right">&#160;11.86&#160;<TD align="right">&#160;14.16&#160;<TD align="right">&#160;16.05&#160;<TD align="right">&#160;17.43&#160;<TD align="right">&#160;17.88&#160;
<TR><TD align="left"><I>M</I> = 5&#160;<TD align="right">&#160;1.82&#160;<TD align="right">&#160;3.79&#160;<TD align="right">&#160;5.79&#160;<TD align="right">&#160;8.83&#160;<TD align="right">&#160;13.87&#160;<TD align="right">&#160;17.24&#160;<TD align="right">&#160;19.97&#160;<TD align="right">&#160;21.59&#160;<TD align="right">&#160;22.20&#160;
<TR><TD align="left"><I>M</I> = 6&#160;<TD align="right">&#160;1.93&#160;<TD align="right">&#160;4.19&#160;<TD align="right">&#160;6.39&#160;<TD align="right">&#160;9.85&#160;<TD align="right">&#160;15.83&#160;<TD align="right">&#160;19.82&#160;<TD align="right">&#160;23.03&#160;<TD align="right">&#160;25.60&#160;<TD align="right">&#160;26.53&#160;
</TABLE>`

:raw-html:`<TABLE frame="hsides" rules="groups">
<CAPTION>Acceleration for Spline Degree <I>n</I> = 6</CAPTION>
<COLGROUP span="1">
<COLGROUP span="9">
<TR><TH colspan="1" align="right">Period&#160;<TH align="right">&#160;2&#160;<TH align="right">&#160;5&#160;<TH align="right">&#160;10&#160;<TH align="right">&#160;20&#160;<TH align="right">&#160;50&#160;<TH align="right">&#160;100&#160;<TH align="right">&#160;200&#160;<TH align="right">&#160;500&#160;<TH align="right">&#160;1000&#160;
<TR><TH colspan="1" align="left">Oversampling&#160;
<TBODY>
<TR><TD align="left"><I>M</I> = 1&#160;<TD align="right">&#160;1.19&#160;<TD align="right">&#160;1.84&#160;<TD align="right">&#160;2.47&#160;<TD align="right">&#160;3.35&#160;<TD align="right">&#160;4.41&#160;<TD align="right">&#160;4.98&#160;<TD align="right">&#160;5.46&#160;<TD align="right">&#160;5.58&#160;<TD align="right">&#160;5.62&#160;
<TR><TD align="left"><I>M</I> = 2&#160;<TD align="right">&#160;1.26&#160;<TD align="right">&#160;2.36&#160;<TD align="right">&#160;3.27&#160;<TD align="right">&#160;4.59&#160;<TD align="right">&#160;6.49&#160;<TD align="right">&#160;7.61&#160;<TD align="right">&#160;8.38&#160;<TD align="right">&#160;8.91&#160;<TD align="right">&#160;8.96&#160;
<TR><TD align="left"><I>M</I> = 3&#160;<TD align="right">&#160;1.54&#160;<TD align="right">&#160;3.05&#160;<TD align="right">&#160;4.27&#160;<TD align="right">&#160;6.21&#160;<TD align="right">&#160;9.22&#160;<TD align="right">&#160;10.12&#160;<TD align="right">&#160;12.25&#160;<TD align="right">&#160;13.07&#160;<TD align="right">&#160;13.37&#160;
<TR><TD align="left"><I>M</I> = 4&#160;<TD align="right">&#160;1.74&#160;<TD align="right">&#160;3.56&#160;<TD align="right">&#160;5.21&#160;<TD align="right">&#160;7.55&#160;<TD align="right">&#160;11.82&#160;<TD align="right">&#160;13.53&#160;<TD align="right">&#160;16.15&#160;<TD align="right">&#160;17.50&#160;<TD align="right">&#160;17.77&#160;
<TR><TD align="left"><I>M</I> = 5&#160;<TD align="right">&#160;1.84&#160;<TD align="right">&#160;3.93&#160;<TD align="right">&#160;5.81&#160;<TD align="right">&#160;8.70&#160;<TD align="right">&#160;13.51&#160;<TD align="right">&#160;16.72&#160;<TD align="right">&#160;19.62&#160;<TD align="right">&#160;21.53&#160;<TD align="right">&#160;22.30&#160;
<TR><TD align="left"><I>M</I> = 6&#160;<TD align="right">&#160;1.90&#160;<TD align="right">&#160;4.22&#160;<TD align="right">&#160;6.35&#160;<TD align="right">&#160;9.71&#160;<TD align="right">&#160;15.67&#160;<TD align="right">&#160;19.47&#160;<TD align="right">&#160;23.18&#160;<TD align="right">&#160;25.44&#160;<TD align="right">&#160;26.61&#160;
</TABLE>`

:raw-html:`<TABLE frame="hsides" rules="groups">
<CAPTION>Acceleration for Spline Degree <I>n</I> = 7</CAPTION>
<COLGROUP span="1">
<COLGROUP span="9">
<TR><TH colspan="1" align="right">Period&#160;<TH align="right">&#160;2&#160;<TH align="right">&#160;5&#160;<TH align="right">&#160;10&#160;<TH align="right">&#160;20&#160;<TH align="right">&#160;50&#160;<TH align="right">&#160;100&#160;<TH align="right">&#160;200&#160;<TH align="right">&#160;500&#160;<TH align="right">&#160;1000&#160;
<TR><TH colspan="1" align="left">Oversampling&#160;
<TBODY>
<TR><TD align="left"><I>M</I> = 1&#160;<TD align="right">&#160;1.15&#160;<TD align="right">&#160;1.82&#160;<TD align="right">&#160;2.32&#160;<TD align="right">&#160;3.18&#160;<TD align="right">&#160;4.29&#160;<TD align="right">&#160;4.92&#160;<TD align="right">&#160;5.33&#160;<TD align="right">&#160;5.60&#160;<TD align="right">&#160;5.58&#160;
<TR><TD align="left"><I>M</I> = 2&#160;<TD align="right">&#160;1.23&#160;<TD align="right">&#160;2.37&#160;<TD align="right">&#160;3.17&#160;<TD align="right">&#160;4.42&#160;<TD align="right">&#160;6.32&#160;<TD align="right">&#160;7.39&#160;<TD align="right">&#160;8.20&#160;<TD align="right">&#160;8.71&#160;<TD align="right">&#160;8.97&#160;
<TR><TD align="left"><I>M</I> = 3&#160;<TD align="right">&#160;1.59&#160;<TD align="right">&#160;3.05&#160;<TD align="right">&#160;4.27&#160;<TD align="right">&#160;6.03&#160;<TD align="right">&#160;8.79&#160;<TD align="right">&#160;11.06&#160;<TD align="right">&#160;12.15&#160;<TD align="right">&#160;13.11&#160;<TD align="right">&#160;13.36&#160;
<TR><TD align="left"><I>M</I> = 4&#160;<TD align="right">&#160;1.75&#160;<TD align="right">&#160;3.52&#160;<TD align="right">&#160;5.04&#160;<TD align="right">&#160;7.37&#160;<TD align="right">&#160;9.16&#160;<TD align="right">&#160;13.77&#160;<TD align="right">&#160;15.80&#160;<TD align="right">&#160;17.45&#160;<TD align="right">&#160;18.00&#160;
<TR><TD align="left"><I>M</I> = 5&#160;<TD align="right">&#160;1.83&#160;<TD align="right">&#160;3.89&#160;<TD align="right">&#160;5.71&#160;<TD align="right">&#160;8.53&#160;<TD align="right">&#160;13.43&#160;<TD align="right">&#160;16.77&#160;<TD align="right">&#160;19.49&#160;<TD align="right">&#160;21.47&#160;<TD align="right">&#160;22.55&#160;
<TR><TD align="left"><I>M</I> = 6&#160;<TD align="right">&#160;1.98&#160;<TD align="right">&#160;4.22&#160;<TD align="right">&#160;6.30&#160;<TD align="right">&#160;9.55&#160;<TD align="right">&#160;15.05&#160;<TD align="right">&#160;19.28&#160;<TD align="right">&#160;23.05&#160;<TD align="right">&#160;25.61&#160;<TD align="right">&#160;26.28&#160;
</TABLE>`

:raw-html:`<TABLE frame="hsides" rules="groups">
<CAPTION>Acceleration for Spline Degree <I>n</I> = 8</CAPTION>
<COLGROUP span="1">
<COLGROUP span="9">
<TR><TH colspan="1" align="right">Period&#160;<TH align="right">&#160;2&#160;<TH align="right">&#160;5&#160;<TH align="right">&#160;10&#160;<TH align="right">&#160;20&#160;<TH align="right">&#160;50&#160;<TH align="right">&#160;100&#160;<TH align="right">&#160;200&#160;<TH align="right">&#160;500&#160;<TH align="right">&#160;1000&#160;
<TR><TH colspan="1" align="left">Oversampling&#160;
<TBODY>
<TR><TD align="left"><I>M</I> = 1&#160;<TD align="right">&#160;1.16&#160;<TD align="right">&#160;1.79&#160;<TD align="right">&#160;2.23&#160;<TD align="right">&#160;3.03&#160;<TD align="right">&#160;4.12&#160;<TD align="right">&#160;4.82&#160;<TD align="right">&#160;5.30&#160;<TD align="right">&#160;5.53&#160;<TD align="right">&#160;5.58&#160;
<TR><TD align="left"><I>M</I> = 2&#160;<TD align="right">&#160;1.29&#160;<TD align="right">&#160;2.39&#160;<TD align="right">&#160;3.13&#160;<TD align="right">&#160;4.34&#160;<TD align="right">&#160;6.22&#160;<TD align="right">&#160;7.26&#160;<TD align="right">&#160;8.22&#160;<TD align="right">&#160;8.74&#160;<TD align="right">&#160;8.91&#160;
<TR><TD align="left"><I>M</I> = 3&#160;<TD align="right">&#160;1.59&#160;<TD align="right">&#160;3.05&#160;<TD align="right">&#160;4.13&#160;<TD align="right">&#160;5.88&#160;<TD align="right">&#160;9.05&#160;<TD align="right">&#160;10.61&#160;<TD align="right">&#160;12.07&#160;<TD align="right">&#160;12.87&#160;<TD align="right">&#160;13.47&#160;
<TR><TD align="left"><I>M</I> = 4&#160;<TD align="right">&#160;1.69&#160;<TD align="right">&#160;3.56&#160;<TD align="right">&#160;4.98&#160;<TD align="right">&#160;7.24&#160;<TD align="right">&#160;9.13&#160;<TD align="right">&#160;13.60&#160;<TD align="right">&#160;15.73&#160;<TD align="right">&#160;17.38&#160;<TD align="right">&#160;17.58&#160;
<TR><TD align="left"><I>M</I> = 5&#160;<TD align="right">&#160;1.92&#160;<TD align="right">&#160;3.97&#160;<TD align="right">&#160;5.68&#160;<TD align="right">&#160;8.35&#160;<TD align="right">&#160;13.16&#160;<TD align="right">&#160;16.59&#160;<TD align="right">&#160;19.47&#160;<TD align="right">&#160;21.11&#160;<TD align="right">&#160;22.08&#160;
<TR><TD align="left"><I>M</I> = 6&#160;<TD align="right">&#160;2.01&#160;<TD align="right">&#160;4.27&#160;<TD align="right">&#160;6.32&#160;<TD align="right">&#160;9.42&#160;<TD align="right">&#160;15.10&#160;<TD align="right">&#160;19.25&#160;<TD align="right">&#160;22.72&#160;<TD align="right">&#160;25.02&#160;<TD align="right">&#160;25.68&#160;
</TABLE>`

:raw-html:`<TABLE frame="hsides" rules="groups">
<CAPTION>Acceleration for Spline Degree <I>n</I> = 9</CAPTION>
<COLGROUP span="1">
<COLGROUP span="9">
<TR><TH colspan="1" align="right">Period&#160;<TH align="right">&#160;2&#160;<TH align="right">&#160;5&#160;<TH align="right">&#160;10&#160;<TH align="right">&#160;20&#160;<TH align="right">&#160;50&#160;<TH align="right">&#160;100&#160;<TH align="right">&#160;200&#160;<TH align="right">&#160;500&#160;<TH align="right">&#160;1000&#160;
<TR><TH colspan="1" align="left">Oversampling&#160;
<TBODY>
<TR><TD align="left"><I>M</I> = 1&#160;<TD align="right">&#160;1.15&#160;<TD align="right">&#160;1.75&#160;<TD align="right">&#160;2.16&#160;<TD align="right">&#160;2.92&#160;<TD align="right">&#160;4.02&#160;<TD align="right">&#160;4.70&#160;<TD align="right">&#160;5.23&#160;<TD align="right">&#160;5.50&#160;<TD align="right">&#160;5.60&#160;
<TR><TD align="left"><I>M</I> = 2&#160;<TD align="right">&#160;1.30&#160;<TD align="right">&#160;2.37&#160;<TD align="right">&#160;3.06&#160;<TD align="right">&#160;4.25&#160;<TD align="right">&#160;6.03&#160;<TD align="right">&#160;7.17&#160;<TD align="right">&#160;8.12&#160;<TD align="right">&#160;8.78&#160;<TD align="right">&#160;8.92&#160;
<TR><TD align="left"><I>M</I> = 3&#160;<TD align="right">&#160;1.59&#160;<TD align="right">&#160;3.06&#160;<TD align="right">&#160;4.13&#160;<TD align="right">&#160;5.76&#160;<TD align="right">&#160;8.96&#160;<TD align="right">&#160;10.33&#160;<TD align="right">&#160;12.16&#160;<TD align="right">&#160;12.98&#160;<TD align="right">&#160;13.42&#160;
<TR><TD align="left"><I>M</I> = 4&#160;<TD align="right">&#160;1.79&#160;<TD align="right">&#160;3.56&#160;<TD align="right">&#160;4.95&#160;<TD align="right">&#160;7.10&#160;<TD align="right">&#160;10.77&#160;<TD align="right">&#160;13.69&#160;<TD align="right">&#160;14.68&#160;<TD align="right">&#160;17.40&#160;<TD align="right">&#160;17.55&#160;
<TR><TD align="left"><I>M</I> = 5&#160;<TD align="right">&#160;1.85&#160;<TD align="right">&#160;3.91&#160;<TD align="right">&#160;5.66&#160;<TD align="right">&#160;8.20&#160;<TD align="right">&#160;12.90&#160;<TD align="right">&#160;16.29&#160;<TD align="right">&#160;19.33&#160;<TD align="right">&#160;21.35&#160;<TD align="right">&#160;22.24&#160;
<TR><TD align="left"><I>M</I> = 6&#160;<TD align="right">&#160;1.96&#160;<TD align="right">&#160;4.28&#160;<TD align="right">&#160;6.33&#160;<TD align="right">&#160;9.45&#160;<TD align="right">&#160;14.22&#160;<TD align="right">&#160;19.29&#160;<TD align="right">&#160;22.71&#160;<TD align="right">&#160;25.16&#160;<TD align="right">&#160;25.79&#160;
</TABLE>`

Discussion
""""""""""

The gain in computational efficiency is substantial. It increases with the oversampling factor and with the period of the spline. The degree of the spline does not play much of a role.
