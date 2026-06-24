:orphan:

..  role:: raw-html(raw)
    :format: html

Padding
=======

How to extend a finite-length vector of data to a virtually infinite-length sequence.

----

Purpose
-------

We wish to build the real function

..  math::
        f:{\mathbb{R}}\rightarrow{\mathbb{R}},x\mapsto f(x)=\sum_{k\in{\mathbb{Z}}}\,c[k]\,\varphi(x-k).

There, the sequence :math:`c` of coefficients is used to parameterize the function :math:`f` and gives us a good many degrees of freedom to shape it to our taste. The adaptability of :math:`c` makes it a tool of choice to represent sampled data as the continuously defined function :math:`f.` Given a sequence :math:`y=\left(y[k]\right)_{k\in{\mathbb{Z}}}` of regularly indexed samples :math:`y[k]` for :math:`k\in{\mathbb{Z}},` the purpose of the so-called interpolation procedure is to yield a sequence :math:`c` such that the interpolation condition :math:`f(k)=y[k]` is satisfied.

In case :math:`\varphi` is a B-spline, we shall soon see that there exist infinitely many sequences of coefficients that build the same one sequence of samples. Among all these sequences of coefficients , however, there is one that is of particular interest because it can be obtained by a very efficient algorithm to get :math:`c` out of :math:`y.`

Unfortunately, the theoretical derivations of the algorithm rely on :math:`y` being a *sequence*, which means that infinitely many samples are required. Now, one never has access to a sequence of samples in practice, only to a finite-dimensional *vector* :math:`{\mathbf{y}}=\left(y_{q}\right)_{q=1}^{K}\in{\mathbb{R}}^{K}` of samples. To take advantage of the theoretical derivations of the efficient algorithm, it is thus an unavoidable necessity that a method be engineered that converts the vector :math:`{\mathbf{y}}` into the sequence :math:`y.` We call padding the operation that consists in the engineering of the infinite-length subsequences :math:`\left(y[k]\right)_{k\in{\mathbb{Z}}_{<0}}` to the left and :math:`\left(y[k]\right)_{k\in{\mathbb{Z}}_{\geq K}}` to the right of the provided finite-length sequence (or finite-dimensional vector) :math:`\left(y[k]\right)_{k=0}^{K-1}=\left(y_{q}\right)_{q=1}^{K}={\mathbf{y}}.`

The engineering of :math:`{\mathbf{y}}\Rightarrow y` is application-dependent and every method is fair game. Here are a few cases.

*   The components of the vector :math:`{\mathbf{y}}` could represent angular data, in which case one would have to cope with angular wrapping.
*   Or, :math:`{\mathbf{y}}` could represent intensity data, in which case one would have to discourage negative intensities in the continuously defined function :math:`f` being constructed through the steps :math:`{\mathbf{y}}\Rightarrow y\Rightarrow c\Rightarrow f.`
*   Or, one could pretend that all unobserved samples do vanish and take the special value :math:`0.`
*   Or, one could assume that the first observed sample :math:`y[0]` has indeed the same value as all unobserved samples that came before, while the last observed sample :math:`y[K-1]` has the same value as all unobserved samples that folllow.

Many more padding recipes can be devised. Which one is the best in the context of your application is only a matter of convenience.

Uniqueness
----------

In the sequel, we let :math:`\varphi` be a polynomial B-spline of nonnegative integer degree :math:`n,` which is a real function :math:`\beta^{n}:{\mathbb{R}}\rightarrow{\mathbb{R}},x\mapsto\beta^{n}(x).` For any degree :math:`n\geq2` and for :math:`m\in[1\ldots\left\lfloor n/2\right\rfloor],` it is known that there exist :math:`\left\lfloor n/2\right\rfloor` mutually different, real, negative numbers :math:`z_{n,m}` in the open interval :math:`(-1,0)` that satisfy the relation

..  math::
        \frac{1}{\sum_{k\in{\mathbb{Z}}}\,\beta^{n}(k)\,z_{n,m}^{-k}}\not\in{\mathbb{C}}.

These numbers are called *poles* and are such that :math:`\sum_{k\in{\mathbb{Z}}}\,\beta^{n}(k)\,z_{n,m}^{-k}=0.` Because B-splines are even-symmetric, the pole reciprocals :math:`z_{n,m}^{-1}\in{\mathbb{R}}_{<-1}` satisfy the same relation.

Suppose now we have identified a sequence :math:`c` that verifies the interpolation condition :math:`\forall k\in{\mathbb{Z}}:y[k]=\sum_{k'\in{\mathbb{Z}}}\,c[k']\,\beta^{n}(k-k').` Then, it turns out that the sequence :math:`\left(c'[k']\right)_{k'\in{\mathbb{Z}}}=\left(c[k']+\sum_{m=1}^{\left\lfloor n/2\right\rfloor}\,\left(\lambda_{n,m}^{-}\,z_{n,m}^{-k'}+\lambda_{n,m}^{+}\,z_{n,m}^{k'}\right)\right)_{k'\in{\mathbb{Z}}}` is also such that :math:`\forall k\in{\mathbb{Z}}:y[k]=\sum_{k'\in{\mathbb{Z}}}\,c'[k']\,\beta^{n}(k-k'),` for any choice of the arbitrary constants :math:`\lambda_{n,m}^{-},\lambda_{n,m}^{+}\in{\mathbb{R}}.` One concludes that the interpolation condition :math:`\forall k\in{\mathbb{Z}}:y[k]=f(k)` alone is not sufficient to make the interpolation task well-defined and that additional requirements are needed.

The ``splinekit`` library comes equipped with paddings that guarantee the well-defined interpolation of finite-length data.

Available Paddings
------------------

The unobserved samples are, well, unobserved. Consequently, every strategy that assigns specific values to them is valid, but some are less practical than others. The ``splinekit`` library deals with paddings of low complexity; in particular, we focus on some for which the overall organization of :math:`c,` :math:`y,` and :math:`f` can be made consistent and solves the uniqueness issue. The seven paddings being considered are

*   Periodic
*   Narrow Mirror
*   Wide Mirror
*   Anti-Mirror
*   Nega-Periodic
*   Nega-Narrow Mirror
*   Nega-Wide Mirror

Except for the anti-mirror padding, all proposed forms honor some sort of periodicity over :math:`c,` albeit the length of a period may differ from the number :math:`K` of observed samples. The periodicity of :math:`c` then implies that :math:`y` and :math:`f` are likewise periodic; moreover, the trivial choice :math:`\lambda_{n,m}^{-}=0` and :math:`\lambda_{n,m}^{+}=0` is the only one that results in the alternative versions :math:`c'` of :math:`c` being periodic, too, which ensures uniqueness and makes the interpolation problem well-defined. In the case of the anti-mirror padding, a similar reasoning also leads to the uniqueness of the solution.

**Side Note** *The periodicity of the coefficients implies the periodicity of the samples, but the periodicity of the samples does not necessarily imply that the the coefficients are periodic. Indeed, the constants* :math:`\lambda_{n,m}^{-},\lambda_{n,m}^{+}` *can still be chosen arbitrarily to parameterize a family of coefficients that satisfy the interpolation constraint, even for periodic samples. In general, only one member of this family will have the same periodicity as that of the samples.*

We give now a piece of code that illustrates visually the effect of the various paddings on random splines of a specified degree.

..  admonition:: Jupyter Lab notebook

    `Padded splines <https://splinekit.github.io/splinekit-jupyterlite/lab/?path=padding/padding/padding_data.ipynb&mode=single-document>`_

----

Periodic Padding
----------------

An easy, general-purpose padding approach is to engineer the sequence :math:`c` of coefficients to be :math:`K`-periodic. This implies that the sequence :math:`y` of samples has to be the straighforward :math:`K`-periodized version of the vector :math:`{\mathbf{y}}\in{\mathbb{R}}^{K}.` Ultimately, this also implies that the function :math:`f` is itself :math:`K`-periodic. In summary, under a periodic padding, the relations being satisfied for any :math:`k\in{\mathbb{Z}}` and any :math:`x\in{\mathbb{R}}` are

..  math::
        c[k]=c[k+K]\;\Rightarrow\;\left\{\begin{array}{rcl}y[k]&=&y[k+K]\\f(x)&=&f(x+K).\end{array}\right.

Algorithmic Considerations
^^^^^^^^^^^^^^^^^^^^^^^^^^

In the context of a straightforward periodic padding, there are three major algorithmic approaches to the solution of the interpolation constraint :math:`y[k]=f(k)` for :math:`k\in[0\ldots K-1].`

1) The **linear-algebra approach** first establishes an explicit system of :math:`K` linear equations. The :math:`k`-th equation of the system would be :math:`y[k]=\sum_{k'=0}^{K-1}\,\left(\sum_{p\in{\mathbb{Z}}}\,\varphi(k-p\,K-k')\right)\,c[k'].` Tools of linear algebra would then be deployed to solve the system in terms of the unknown vector :math:`{\mathbf{c}}=\left(c[k]\right)_{k=0}^{K-1}` that represents one period of the periodic sequence :math:`c,` taken such that the first vector component :math:`c_{1}` is the sequence element at the origin :math:`c[0].` The overall computational cost is :math:`{\mathcal{O}}(K^{3})` when general solvers are used, and the cost would be :math:`{\mathcal{O}}(K^{2})` for Toeplitz systems. For periodic paddings, the system is circulant and the overall computational cost reduces to :math:`{\mathcal{O}}(K\,\log K)` with Fourier-based techniques to solve linear-algebra inversion problems.

2) The **discrete-Fourier approach** is best described concisely in matrix notations. Let :math:`{\mathbf{F}}\in{\mathbb{C}}^{K\times K}` be the discrete-Fourier transform, with the :math:`\nu`-th row and :math:`k`-th column entry given by :math:`{\mathrm{e}}^{-{\mathrm{j}}\,\left(\nu-1\right)\,\frac{2\,\pi}{K}\,\left(k-1\right)}.` Let :math:`{\mathbf{\phi}}=(\sum_{p\in{\mathbb{Z}}}\,\varphi(p\,K+k))_{k=0}^{K-1}` be the data-independent vector that contains the samples (at the integers) of the periodized version of the synthesis function :math:`\varphi.` Then, one has that :math:`{\mathbf{c}}={\mathbf{F}}^{-1}\,\left(\left({\mathbf{F}}\,{\mathbf{y}}\right)\oslash\left({\mathbf{F}}\,{\mathbf{\phi}}\right)\right),` where :math:`\oslash` is an element-wise division. In practice, the Fourier transformation and its inverse are implemented via the fast-Fourier algorithm, in which case the overall computational cost is :math:`{\mathcal{O}}(K\,\log K).`

3) The **recursive-filtering approach** is the one followed in the ``splinekit`` library. It requires that :math:`\varphi` has a finite support, is even-symmetric, and that the poles of the reciprocal of the :math:`z`-transform of its samples at the integers are real numbers. These properties are all satisfied by the polynomial B-splines of nonnegative integer degree :math:`n\in{\mathbb{N}}+2.` The overall computational cost is now :math:`{\mathcal{O}}(K\,\left\lfloor n/2\right\rfloor).`

Recursive Filtering
"""""""""""""""""""

Start the algorithm by letting :math:`{\mathbf{c}}\leftarrow{\mathbf{y}}.` Then, iteratively for every one of the poles :math:`z_{n,m}\in(-1,0)` indexed by :math:`m\in[1\ldots\left\lfloor n/2\right\rfloor]` and associated to the degree :math:`n,` apply the in-place recursive updates

..  math::
        \left\{\begin{array}{rcll}c[0]&\leftarrow&\frac{1}{1-z_{n,m}^{K}}\,\left(c[0]+\sum_{k=1}^{K-1}\,z_{n,m}^{k}\,c[K-k]\right)\\c[k]&\leftarrow&c[k]+z_{n,m}\,c[k-1],&k\in[1\ldots K-1]\\c[K-1]&\leftarrow&\frac{\left(1-z_{n,m}\right)^{2}}{1-z_{n,m}^{K}}\,\left(c[K-1]+\sum_{k=0}^{K-2}\,z_{n,m}^{k+1}\,c[k]\right)\\c[K-1-k]&\leftarrow&z_{n,m}\,c[K-k]+\left(1-z_{n,m}\right)^{2}\,c[K-1-k],&k\in[1\ldots K-1].\end{array}\right.

In practice, some computations can be spared if the sums that appear in the recursive-update equations are truncated at that index :math:`k` where the term :math:`z_{n,m}^{k}` becomes negligible.

Experimental Performance
^^^^^^^^^^^^^^^^^^^^^^^^

The following experiment establishes some simple statistics over the gain in speed achieved by the recursive approach over the fast-Fourier-based one. For each length :math:`K` and for each spline degree, we synthesize :math:`50` random vectors :math:`{\mathbf{y}}\in{\mathbb{Z}}^{K}` and let both the Fourier approach and the recursive approach determine the spline coefficients on the same data. We measure the duration of those computations and report by how many times the recursive approach is faster relatively to the Fourier approach. For instance, the number :math:`2.0` would mean that the recursive approach would be :math:`200\%` times faster or, equivalently, that it runs twice as fast.

..  danger::

    *   The link below allows you to inspect the notebook. Unfortunately, running it from the browser is meaningless: the timings of the *installation-free* version are not representative because the Python kernel is WebAssembly-based and does not run natively.

    *   If you want to test for realistic timings on your own computer, then you will have to first install in full the ``splinekit`` library. Only after that will you be able to launch the notebook either as a regular, full-fledged Jupyter Lab or as a module executed by the native Python kernel.

    *   The timings reported in the Results Section correspond to those of the native execution.

..  admonition:: Jupyter Lab notebook

    `Recursive vs Fourier <https://splinekit.github.io/splinekit-jupyterlite/lab/?path=padding/padding/padding_speed.ipynb&mode=single-document>`_

..  hint::
    The notebook is available for download in compressed form from
    :download:`here <padding_speed.ipynb.gz>`. Decompression is achieved from the terminal with ``gunzip padding_speed.ipynb.gz``.

Results
"""""""

In the first table, the number :math:`K` of observed data are powers of two, a situation that is very much to the advantage of the discrete-Fourier methods. On a desktop computer of 2021, typical resulting tables are as follows.

:raw-html:`<TABLE frame="hsides" rules="groups">
<CAPTION>Acceleration for Dyadic Periods</CAPTION>
<COLGROUP span="2">
<COLGROUP span="8">
<TR><TH colspan="2" align="right">Degree&#160;<TH align="right">&#160;2&#160;<TH align="right">&#160;3&#160;<TH align="right">&#160;4&#160;<TH align="right">&#160;5&#160;<TH align="right">&#160;6&#160;<TH align="right">&#160;7&#160;<TH align="right">&#160;8&#160;<TH align="right">&#160;9&#160;
<TR><TH colspan="2" align="left">Period&#160;
<TBODY>
<TR><TD align="left"><I>K</I> =&#160;<TD align="right">1&#160;<TD align="right">&#160;0.5&#160;<TD align="right">&#160;0.5&#160;<TD align="right">&#160;0.5&#160;<TD align="right">&#160;0.5&#160;<TD align="right">&#160;0.4&#160;<TD align="right">&#160;0.5&#160;<TD align="right">&#160;0.5&#160;<TD align="right">&#160;0.5&#160;
<TR><TD align="left"><I>K</I> =&#160;<TD align="right">2&#160;<TD align="right">&#160;10.2&#160;<TD align="right">&#160;8.0&#160;<TD align="right">&#160;9.9&#160;<TD align="right">&#160;7.0&#160;<TD align="right">&#160;11.0&#160;<TD align="right">&#160;8.8&#160;<TD align="right">&#160;12.7&#160;<TD align="right">&#160;7.7&#160;
<TR><TD align="left"><I>K</I> =&#160;<TD align="right">4&#160;<TD align="right">&#160;8.3&#160;<TD align="right">&#160;8.2&#160;<TD align="right">&#160;10.1&#160;<TD align="right">&#160;7.6&#160;<TD align="right">&#160;11.6&#160;<TD align="right">&#160;11.6&#160;<TD align="right">&#160;12.6&#160;<TD align="right">&#160;10.9&#160;
<TR><TD align="left"><I>K</I> =&#160;<TD align="right">8&#160;<TD align="right">&#160;8.9&#160;<TD align="right">&#160;8.8&#160;<TD align="right">&#160;10.2&#160;<TD align="right">&#160;8.1&#160;<TD align="right">&#160;12.5&#160;<TD align="right">&#160;10.3&#160;<TD align="right">&#160;12.7&#160;<TD align="right">&#160;10.5&#160;
<TR><TD align="left"><I>K</I> =&#160;<TD align="right">16&#160;<TD align="right">&#160;9.8&#160;<TD align="right">&#160;9.7&#160;<TD align="right">&#160;7.9&#160;<TD align="right">&#160;10.0&#160;<TD align="right">&#160;12.0&#160;<TD align="right">&#160;12.2&#160;<TD align="right">&#160;10.7&#160;<TD align="right">&#160;11.7&#160;
<TR><TD align="left"><I>K</I> =&#160;<TD align="right">32&#160;<TD align="right">&#160;11.5&#160;<TD align="right">&#160;11.4&#160;<TD align="right">&#160;9.8&#160;<TD align="right">&#160;12.4&#160;<TD align="right">&#160;13.0&#160;<TD align="right">&#160;9.9&#160;<TD align="right">&#160;11.4&#160;<TD align="right">&#160;13.7&#160;
<TR><TD align="left"><I>K</I> =&#160;<TD align="right">64&#160;<TD align="right">&#160;15.2&#160;<TD align="right">&#160;14.8&#160;<TD align="right">&#160;12.6&#160;<TD align="right">&#160;14.7&#160;<TD align="right">&#160;14.5&#160;<TD align="right">&#160;14.5&#160;<TD align="right">&#160;12.4&#160;<TD align="right">&#160;14.5&#160;
<TR><TD align="left"><I>K</I> =&#160;<TD align="right">128&#160;<TD align="right">&#160;20.4&#160;<TD align="right">&#160;20.4&#160;<TD align="right">&#160;15.6&#160;<TD align="right">&#160;18.2&#160;<TD align="right">&#160;16.6&#160;<TD align="right">&#160;16.5&#160;<TD align="right">&#160;13.7&#160;<TD align="right">&#160;15.4&#160;
<TR><TD align="left"><I>K</I> =&#160;<TD align="right">256&#160;<TD align="right">&#160;27.8&#160;<TD align="right">&#160;23.8&#160;<TD align="right">&#160;23.1&#160;<TD align="right">&#160;22.6&#160;<TD align="right">&#160;19.4&#160;<TD align="right">&#160;17.4&#160;<TD align="right">&#160;17.0&#160;<TD align="right">&#160;16.7&#160;
<TR><TD align="left"><I>K</I> =&#160;<TD align="right">512&#160;<TD align="right">&#160;40.3&#160;<TD align="right">&#160;34.9&#160;<TD align="right">&#160;29.4&#160;<TD align="right">&#160;28.9&#160;<TD align="right">&#160;23.3&#160;<TD align="right">&#160;21.4&#160;<TD align="right">&#160;19.3&#160;<TD align="right">&#160;19.0&#160;
<TR><TD align="left"><I>K</I> =&#160;<TD align="right">1024&#160;<TD align="right">&#160;52.6&#160;<TD align="right">&#160;49.6&#160;<TD align="right">&#160;37.0&#160;<TD align="right">&#160;35.6&#160;<TD align="right">&#160;26.9&#160;<TD align="right">&#160;24.9&#160;<TD align="right">&#160;21.7&#160;<TD align="right">&#160;21.1&#160;
<TR><TD align="left"><I>K</I> =&#160;<TD align="right">2048&#160;<TD align="right">&#160;75.6&#160;<TD align="right">&#160;69.7&#160;<TD align="right">&#160;44.8&#160;<TD align="right">&#160;42.9&#160;<TD align="right">&#160;31.3&#160;<TD align="right">&#160;29.2&#160;<TD align="right">&#160;23.3&#160;<TD align="right">&#160;23.1&#160;
<TR><TD align="left"><I>K</I> =&#160;<TD align="right">4096&#160;<TD align="right">&#160;96.5&#160;<TD align="right">&#160;96.6&#160;<TD align="right">&#160;54.1&#160;<TD align="right">&#160;53.1&#160;<TD align="right">&#160;36.8&#160;<TD align="right">&#160;33.4&#160;<TD align="right">&#160;26.5&#160;<TD align="right">&#160;26.5&#160;
<TR><TD align="left"><I>K</I> =&#160;<TD align="right">8192&#160;<TD align="right">&#160;101.6&#160;<TD align="right">&#160;99.5&#160;<TD align="right">&#160;53.6&#160;<TD align="right">&#160;53.0&#160;<TD align="right">&#160;36.1&#160;<TD align="right">&#160;31.8&#160;<TD align="right">&#160;25.1&#160;<TD align="right">&#160;25.0&#160;
<TR><TD align="left"><I>K</I> =&#160;<TD align="right">16384&#160;<TD align="right">&#160;107.4&#160;<TD align="right">&#160;105.9&#160;<TD align="right">&#160;55.7&#160;<TD align="right">&#160;55.2&#160;<TD align="right">&#160;38.1&#160;<TD align="right">&#160;32.4&#160;<TD align="right">&#160;25.8&#160;<TD align="right">&#160;25.4&#160;
</TABLE>`

In the second table, we repeat the experiment, with the difference that the :math:`50` lengths are now chosen uniformly at random in some range. On a desktop computer of 2021, typical resulting tables are as follows.

:raw-html:`<TABLE frame="hsides" rules="groups">
<CAPTION>Acceleration for Random Periods</CAPTION>
<COLGROUP span="3">
<COLGROUP span="8">
<TR><TH colspan="3" align="right">Degree&#160;<TH align="right">&#160;2&#160;<TH align="right">&#160;3&#160;<TH align="right">&#160;4&#160;<TH align="right">&#160;5&#160;<TH align="right">&#160;6&#160;<TH align="right">&#160;7&#160;<TH align="right">&#160;8&#160;<TH align="right">&#160;9&#160;
<TR><TH colspan="3" align="left">Period Range&#160;
<TBODY>
<TR><TD align="right">1&#160;<TD align="left">&#8804; <I>K</I> &#60;&#160;<TD align="right">2&#160;<TD align="right">&#160;0.6&#160;<TD align="right">&#160;0.5&#160;<TD align="right">&#160;0.5&#160;<TD align="right">&#160;0.5&#160;<TD align="right">&#160;0.5&#160;<TD align="right">&#160;0.5&#160;<TD align="right">&#160;0.5&#160;<TD align="right">&#160;0.5&#160;
<TR><TD align="right">2&#160;<TD align="left">&#8804; <I>K</I> &#60;&#160;<TD align="right">4&#160;<TD align="right">&#160;6.1&#160;<TD align="right">&#160;8.3&#160;<TD align="right">&#160;9.5&#160;<TD align="right">&#160;9.1&#160;<TD align="right">&#160;9.4&#160;<TD align="right">&#160;11.0&#160;<TD align="right">&#160;12.5&#160;<TD align="right">&#160;11.5&#160;
<TR><TD align="right">4&#160;<TD align="left">&#8804; <I>K</I> &#60;&#160;<TD align="right">8&#160;<TD align="right">&#160;8.6&#160;<TD align="right">&#160;8.2&#160;<TD align="right">&#160;9.8&#160;<TD align="right">&#160;7.6&#160;<TD align="right">&#160;11.4&#160;<TD align="right">&#160;11.3&#160;<TD align="right">&#160;11.5&#160;<TD align="right">&#160;9.8&#160;
<TR><TD align="right">8&#160;<TD align="left">&#8804; <I>K</I> &#60;&#160;<TD align="right">16&#160;<TD align="right">&#160;9.5&#160;<TD align="right">&#160;9.1&#160;<TD align="right">&#160;10.4&#160;<TD align="right">&#160;8.3&#160;<TD align="right">&#160;9.2&#160;<TD align="right">&#160;11.7&#160;<TD align="right">&#160;12.6&#160;<TD align="right">&#160;10.2&#160;
<TR><TD align="right">16&#160;<TD align="left">&#8804; <I>K</I> &#60;&#160;<TD align="right">32&#160;<TD align="right">&#160;10.7&#160;<TD align="right">&#160;10.5&#160;<TD align="right">&#160;11.5&#160;<TD align="right">&#160;9.4&#160;<TD align="right">&#160;12.5&#160;<TD align="right">&#160;12.6&#160;<TD align="right">&#160;13.5&#160;<TD align="right">&#160;11.2&#160;
<TR><TD align="right">32&#160;<TD align="left">&#8804; <I>K</I> &#60;&#160;<TD align="right">64&#160;<TD align="right">&#160;13.4&#160;<TD align="right">&#160;13.4&#160;<TD align="right">&#160;13.7&#160;<TD align="right">&#160;11.3&#160;<TD align="right">&#160;13.9&#160;<TD align="right">&#160;14.3&#160;<TD align="right">&#160;14.2&#160;<TD align="right">&#160;14.4&#160;
<TR><TD align="right">64&#160;<TD align="left">&#8804; <I>K</I> &#60;&#160;<TD align="right">128&#160;<TD align="right">&#160;19.0&#160;<TD align="right">&#160;18.3&#160;<TD align="right">&#160;14.3&#160;<TD align="right">&#160;17.3&#160;<TD align="right">&#160;16.2&#160;<TD align="right">&#160;16.2&#160;<TD align="right">&#160;13.6&#160;<TD align="right">&#160;15.5&#160;
<TR><TD align="right">128&#160;<TD align="left">&#8804; <I>K</I> &#60;&#160;<TD align="right">256&#160;<TD align="right">&#160;25.0&#160;<TD align="right">&#160;24.8&#160;<TD align="right">&#160;18.2&#160;<TD align="right">&#160;21.1&#160;<TD align="right">&#160;18.7&#160;<TD align="right">&#160;18.5&#160;<TD align="right">&#160;15.3&#160;<TD align="right">&#160;16.7&#160;
<TR><TD align="right">256&#160;<TD align="left">&#8804; <I>K</I> &#60;&#160;<TD align="right">512&#160;<TD align="right">&#160;36.3&#160;<TD align="right">&#160;35.8&#160;<TD align="right">&#160;25.1&#160;<TD align="right">&#160;27.6&#160;<TD align="right">&#160;22.7&#160;<TD align="right">&#160;22.3&#160;<TD align="right">&#160;18.2&#160;<TD align="right">&#160;19.1&#160;
<TR><TD align="right">512&#160;<TD align="left">&#8804; <I>K</I> &#60;&#160;<TD align="right">1024&#160;<TD align="right">&#160;57.3&#160;<TD align="right">&#160;55.9&#160;<TD align="right">&#160;36.1&#160;<TD align="right">&#160;37.9&#160;<TD align="right">&#160;29.5&#160;<TD align="right">&#160;29.2&#160;<TD align="right">&#160;23.1&#160;<TD align="right">&#160;23.8&#160;
<TR><TD align="right">1024&#160;<TD align="left">&#8804; <I>K</I> &#60;&#160;<TD align="right">2048&#160;<TD align="right">&#160;80.2&#160;<TD align="right">&#160;69.8&#160;<TD align="right">&#160;47.3&#160;<TD align="right">&#160;45.9&#160;<TD align="right">&#160;33.7&#160;<TD align="right">&#160;31.1&#160;<TD align="right">&#160;26.0&#160;<TD align="right">&#160;25.7&#160;
<TR><TD align="right">2048&#160;<TD align="left">&#8804; <I>K</I> &#60;&#160;<TD align="right">4096&#160;<TD align="right">&#160;106.5&#160;<TD align="right">&#160;102.1&#160;<TD align="right">&#160;62.6&#160;<TD align="right">&#160;61.3&#160;<TD align="right">&#160;43.3&#160;<TD align="right">&#160;38.5&#160;<TD align="right">&#160;31.3&#160;<TD align="right">&#160;30.9&#160;
<TR><TD align="right">4096&#160;<TD align="left">&#8804; <I>K</I> &#60;&#160;<TD align="right">8192&#160;<TD align="right">&#160;121.8&#160;<TD align="right">&#160;106.9&#160;<TD align="right">&#160;59.6&#160;<TD align="right">&#160;59.3&#160;<TD align="right">&#160;41.0&#160;<TD align="right">&#160;35.8&#160;<TD align="right">&#160;28.6&#160;<TD align="right">&#160;28.3&#160;
<TR><TD align="right">8192&#160;<TD align="left">&#8804; <I>K</I> &#60;&#160;<TD align="right">16384&#160;<TD align="right">&#160;123.4&#160;<TD align="right">&#160;122.7&#160;<TD align="right">&#160;60.9&#160;<TD align="right">&#160;63.5&#160;<TD align="right">&#160;43.5&#160;<TD align="right">&#160;37.4&#160;<TD align="right">&#160;29.4&#160;<TD align="right">&#160;29.5&#160;
<TR><TD align="right">16384&#160;<TD align="left">&#8804; <I>K</I> &#60;&#160;<TD align="right">32768&#160;<TD align="right">&#160;133.3&#160;<TD align="right">&#160;130.2&#160;<TD align="right">&#160;68.0&#160;<TD align="right">&#160;67.0&#160;<TD align="right">&#160;45.5&#160;<TD align="right">&#160;39.5&#160;<TD align="right">&#160;31.1&#160;<TD align="right">&#160;32.1&#160;
</TABLE>`

Discussion
""""""""""

The conclusion of the experiments is unequivocal: The recursive approach is substantially faster than the fast-Fourier approach, at all lengths (except :math:`K=1`), ranges of lengths, and all degrees being investigated.

----

Narrow-Mirror Padding
---------------------

Under a narrow-mirror padding, one assumes :math:`\forall k\in{\mathbb{Z}}` that the spline coefficients satisfy

..  math::
        \left\{\begin{array}{rcl}c[k]&=&c[-k]\\c[k+K-1]&=&c[K-1-k].\end{array}\right.

Then, it holds for any :math:`k\in{\mathbb{Z}}` and any :math:`x\in{\mathbb{R}}` that

..  math::
        c[k+2\,K-2]=c[k]

..  math::
        \left\{\begin{array}{rcl}y[k]&=&y[-k]\\y[k+K-1]&=&y[K-1-k]\\y[k+2\,K-2]&=&y[k]\end{array}\right.

..  math::
        \left\{\begin{array}{rcl}f(x)&=&f(-x)\\f(x+K-1)&=&f(K-1-x)\\f(x+2\,K-2)&=&f(x)\end{array}\right.

Recursive Filtering
^^^^^^^^^^^^^^^^^^^

Start the algorithm by letting :math:`{\mathbf{c}}\leftarrow{\mathbf{y}}.` Then, iteratively for every one of the poles :math:`z_{n,m}\in(-1,0)` indexed by :math:`m\in[1\ldots\left\lfloor n/2\right\rfloor]` and associated to the degree :math:`n,` apply the in-place recursive updates

..  math::
        \left\{\begin{array}{rcll}c[0]&\leftarrow&\frac{1}{1-z_{n,m}^{2\,K-2}}\,\sum_{k=0}^{K-2}\,z_{n,m}^{k}\,\left(c[k]+z_{n,m}^{K-1}\,c[K-1-k]\right)\\c[k]&\leftarrow&c[k]+z_{n,m}\,c[k-1],&k\in[1\ldots K-1]\\c[K-1]&\leftarrow&\frac{\left(1-z_{n,m}\right)^{2}}{1-z_{n,m}^{2}}\,\left(z_{n,m}\,c[K-2]+c[K-1]\right)\\c[K-1-k]&\leftarrow&z_{n,m}\,c[K-k]+\left(1-z_{n,m}\right)^{2}\,c[K-1-k],&k\in[1\ldots K-1]\end{array}\right.

----

Wide-Mirror Padding
-------------------

Under a wide-mirror padding, one assumes :math:`\forall k\in{\mathbb{Z}}` that the spline coefficients satisfy

..  math::
        \left\{\begin{array}{rcl}c[k]&=&c[-1-k]\\c[k+K]&=&c[K-1-k].\end{array}\right.

Then, it holds for any :math:`k\in{\mathbb{Z}}` and any :math:`x\in{\mathbb{R}}` that

..  math::
        c[k+2\,K]=c[k]

..  math::
        \left\{\begin{array}{rcl}y[k]&=&y[-1-k]\\y[k+K]&=&y[K-1-k]\\y[k+2\,K]&=&y[k]\end{array}\right.

..  math::
        \left\{\begin{array}{rcl}f(x)&=&f(-1-x)\\f(x+K)&=&f(K-1-x)\\f(x+2\,K)&=&f(x)\end{array}\right.

Recursive Filtering
^^^^^^^^^^^^^^^^^^^

Start the algorithm by letting :math:`{\mathbf{c}}\leftarrow{\mathbf{y}}.` Then, iteratively for every one of the poles :math:`z_{n,m}\in(-1,0)` indexed by :math:`m\in[1\ldots\left\lfloor n/2\right\rfloor]` and associated to the degree :math:`n,` apply the in-place recursive updates

..  math::
        \left\{\begin{array}{rcll}c[0]&\leftarrow&c[0]+\frac{z_{n,m}}{1-z_{n,m}^{2\,K}}\,\sum_{k=0}^{K-1}\,z_{n,m}^{k}\,\left(c[k]+z_{n,m}^{K}\,c[K-1-k]\right)\\c[k]&\leftarrow&c[k]+z_{n,m}\,c[k-1],&k\in[1\ldots K-1]\\c[K-1]&\leftarrow&\left(1-z_{n,m}\right)\,c[K-1]\\c[K-1-k]&\leftarrow&z_{n,m}\,c[K-k]+\left(1-z_{n,m}\right)^{2}\,c[K-1-k],&k\in[1\ldots K-1]\end{array}\right.

----

Anti-Mirror Padding
-------------------

Under an anti-mirror padding, one assumes :math:`\forall k\in{\mathbb{Z}}` that the spline coefficients satisfy

..  math::
        \left\{\begin{array}{rcl}c[k]-c[0]&=&c[0]-c[-k]\\c[k+K-1]-c[K-1]&=&c[K-1]-c[K-1-k]\end{array}\right.

Then, it holds for any :math:`k\in{\mathbb{Z}}` and any :math:`x\in{\mathbb{R}}` that

..  math::
        c[k+2\,K-2]=c[k]+2\,\left(c[K-1]-c[0]\right)

..  math::
        \left\{\begin{array}{rcl}y[k]-y[0]&=&y[0]-y[-k]\\y[k+K-1]-y[K-1]&=&y[K-1]-y[K-1-k]\\y[k+2\,K-2]&=&y[k]+2\,\left(y[K-1]-y[0]\right)\end{array}\right.

..  math::
        \left\{\begin{array}{rcl}f(x)-f(0)&=&f(0)-f(-x)\\f(x+K-1)-f(K-1)&=&f(K-1)-f(K-1-x)\\f(x+2\,K-2)&=&f(x)+2\,\left(f(K-1)-f(0)\right)\end{array}\right.

Recursive Filtering
^^^^^^^^^^^^^^^^^^^

Start the algorithm by letting :math:`{\mathbf{c}}\leftarrow{\mathbf{y}}.` Then, iteratively for every one of the poles :math:`z_{n,m}\in(-1,0)` indexed by :math:`m\in[1\ldots\left\lfloor n/2\right\rfloor]` and associated to the degree :math:`n,` apply the in-place recursive updates

..  math::
        \left\{\begin{array}{rcll}c[0]&\leftarrow&\frac{1}{1-z_{n,m}^{2\,K-2}}\,\left(\frac{1+z_{n,m}}{1-z_{n,m}}\,\left(c[0]-z_{n,m}^{K-1}\,c[K-1]\right)\right.\\&&\left.\mbox{}-\sum_{k=1}^{K-2}\,z_{n,m}^{k}\,\left(c[k]-z_{n,m}^{K-1}\,c[K-1-k]\right)\right)\\c[k]&\leftarrow&c[k]+z_{n,m}\,c[k-1],&k\in[1\ldots K-1]\\c[K-1]&\leftarrow&c[K-1]-z_{n,m}\,c[K-2]\\c[K-1-k]&\leftarrow&z_{n,m}\,c[K-k]+\left(1-z_{n,m}\right)^{2}\,c[K-1-k],&k\in[1\ldots K-1]\end{array}\right.

----

Nega-Periodic Padding
---------------------

Under a nega-periodic padding, one assumes :math:`\forall k\in{\mathbb{Z}}` that the spline coefficients satisfy

..  math::
        c[k+K]=-c[k]

Then, it holds for any :math:`k\in{\mathbb{Z}}` and any :math:`x\in{\mathbb{R}}` that

..  math::
        c[k+2\,K]=c[k]

..  math::
        \left\{\begin{array}{rcl}y[k+K]&=&-y[k]\\y[k+2\,K]&=&y[k]\end{array}\right.

..  math::
        \left\{\begin{array}{rcl}f(x+K)&=&-f(x)\\f(x+2\,K)&=&f(x)\end{array}\right.

Recursive Filtering
^^^^^^^^^^^^^^^^^^^

Start the algorithm by letting :math:`{\mathbf{c}}\leftarrow{\mathbf{y}}.` Then, iteratively for every one of the poles :math:`z_{n,m}\in(-1,0)` indexed by :math:`m\in[1\ldots\left\lfloor n/2\right\rfloor]` and associated to the degree :math:`n,` apply the in-place recursive updates

..  math::
        \left\{\begin{array}{rcll}c[0]&\leftarrow&c[0]-\frac{z_{n,m}}{1+z_{n,m}^{K}}\,\sum_{k=0}^{K-1}\,z_{n,m}^{K-1-k}\,c[k]\\c[k]&\leftarrow&c[k]+z_{n,m}\,c[k-1],&k\in[1\ldots K-1]\\c[K-1]&\leftarrow&\frac{1-z_{n,m}}{1+z_{n,m}}\,\left(\left(1+z_{n,m}^{2\,K}\right)\,c[K-1]\right.\\&&\left.\mbox{}-\frac{1}{1+z_{n,m}^{K}}\,\sum_{k=0}^{K-1}\,\left(z_{n,m}^{3\,K-1-k}+z_{n,m}^{k+1}\right)\,c[k]\right)\\c[K-1-k]&\leftarrow&z_{n,m}\,c[K-k]+\left(1-z_{n,m}\right)^{2}\,c[K-1-k],&k\in[1\ldots K-1]\end{array}\right.

----

Nega-Narrow-Mirror Padding
--------------------------

Under a nega-narrow-mirror padding, one assumes :math:`\forall k\in{\mathbb{Z}}` that the spline coefficients satisfy

..  math::
        \left\{\begin{array}{rcl}c[k-1]&=&-c[-1-k]\\c[k+K+1]&=&-c[K-1-k].\end{array}\right.

Then, it holds for any :math:`k\in{\mathbb{Z}}` and any :math:`x\in{\mathbb{R}}` that

..  math::
        \left\{\begin{array}{rcl}c[k+2\,K+2]&=&c[k]\\c[\left(K+1\right)\,k-1]&=&0\end{array}\right.

..  math::
        \left\{\begin{array}{rcl}y[k-1]&=&-y[-1-k]\\y[k+K+1]&=&-y[K-1-k]\\y[k+2\,K+2]&=&y[k]\\y[\left(K+1\right)\,k-1]&=&0\end{array}\right.

..  math::
        \left\{\begin{array}{rcl}f(x-1)&=&-f(-1-x)\\f(x+K+1)&=&-f(K-1-x)\\f(x+2\,K+2)&=&f(x)\\f(\left(K+1\right)\,x-1)&=&0\end{array}\right.

Recursive Filtering
^^^^^^^^^^^^^^^^^^^

Start the algorithm by letting :math:`{\mathbf{c}}\leftarrow{\mathbf{y}}.` Then, iteratively for every one of the poles :math:`z_{n,m}\in(-1,0)` indexed by :math:`m\in[1\ldots\left\lfloor n/2\right\rfloor]` and associated to the degree :math:`n,` apply the in-place recursive updates

..  math::
        \left\{\begin{array}{rcll}c[0]&\leftarrow&c[0]-\frac{z_{n,m}^{2}}{1-z_{n,m}^{2\,K+2}}\,\sum_{k=0}^{K-1}\,z_{n,m}^{k}\,\left(c[k]-z_{n,m}^{K+1}\,c[K-1-k]\right)\\c[k]&\leftarrow&c[k]+z_{n,m}\,c[k-1],&k\in[1\ldots K-1]\\c[K-1]&\leftarrow&\left(1-z_{n,m}\right)^{2}\,c[K-1]\\c[K-1-k]&\leftarrow&z_{n,m}\,c[K-k]+\left(1-z_{n,m}\right)^{2}\,c[K-1-k],&k\in[1\ldots K-1]\end{array}\right.

----

Nega-Wide-Mirror Padding
------------------------

Under a nega-wide-mirror padding, one assumes :math:`\forall k\in{\mathbb{Z}}` that the spline coefficients satisfy

..  math::
        \left\{\begin{array}{rcl}c[k]&=&-c[-1-k]\\c[k+K]&=&-c[K-1-k].\end{array}\right.

Then, it holds for any :math:`k\in{\mathbb{Z}}` and any :math:`x\in{\mathbb{R}}` that

..  math::
        c[k+2\,K]=c[k]

..  math::
        \left\{\begin{array}{rcl}y[k]&=&-y[-1-k]\\y[k+K]&=&-y[K-1-k]\\y[k+2\,K]&=&y[k]\end{array}\right.

..  math::
        \left\{\begin{array}{rcl}f(x)&=&-f(-1-x)\\f(x+K)&=&-f(K-1-x)\\f(x+2\,K)&=&f(x)\end{array}\right.

Recursive Filtering
^^^^^^^^^^^^^^^^^^^

Start the algorithm by letting :math:`{\mathbf{c}}\leftarrow{\mathbf{y}}.` Then, iteratively for every one of the poles :math:`z_{n,m}\in(-1,0)` indexed by :math:`m\in[1\ldots\left\lfloor n/2\right\rfloor]` and associated to the degree :math:`n,` apply the in-place recursive updates

..  math::
        \left\{\begin{array}{rcll}c[0]&\leftarrow&c[0]-\frac{z_{n,m}}{1-z_{n,m}^{2\,K}}\,\sum_{k=0}^{K-1}\,z_{n,m}^{k}\,\left(c[k]-z_{n,m}^{K}\,c[K-1-k]\right)\\c[k]&\leftarrow&c[k]+z_{n,m}\,c[k-1],&k\in[1\ldots K-1]\\c[K-1]&\leftarrow&\frac{\left(1-z_{n,m}\right)^{2}}{1+z_{n,m}}\,c[K-1]\\c[K-1-k]&\leftarrow&z_{n,m}\,c[K-k]+\left(1-z_{n,m}\right)^{2}\,c[K-1-k],&k\in[1\ldots K-1]\end{array}\right.
