:orphan:

..  role:: raw-html(raw)
    :format: html

Numeric Stability and Speed of B-Splines
========================================

Four approaches to the computation of the B-spline :math:`\beta,` and discussion of their relative merits in terms of speed and numerical accuracy.

----

Splines
-------

Splines are piecewise polynomials that are built as the weighted sum of integer-shifted B-splines, themselves being piecewise polynomials, too. The weights are called the spline coefficients. Formally, a spline of degree :math:`n\in{\mathbb{N}}` is the mapping

:math:`(1)`

..  math::
        f:{\mathbb{R}}\rightarrow{\mathbb{R}},x\mapsto f(x)=\sum_{k\in{\mathbb{Z}}}\,c[k]\,\beta^{n}(x-k),

where :math:`c` are the spline coefficients and :math:`\beta^{n}` the B-spline, with :math:`n` a superscript (not a power). To accurately evaluate the spline :math:`f` at the argument :math:`x,` it is crucial that the computation of :math:`\beta^{n}` be stable numerically. This is easy to achieve for small degrees; when the degree rises, however, one has to face the fact that the involved polynomials have a tendency to be made of delicately balancing terms.

Here, we investigate how the numerical stability depends on the way B-splines are computed. The ``splinekit`` library proposes a remarquably stable strategy that results in the very fast and *constant-speed* evaluation of B-splines.

----

B-Splines
---------

A B-spline :math:`\beta^{n}` can be computed in more than one way. Here are three possibilities.

Classic Formula
^^^^^^^^^^^^^^^

The classic, productive representation of the :math:`m`-th derivative of a B-spline of degree :math:`n\in{\mathbb{N}}` and argument :math:`x\in{\mathbb{R}}` is

:math:`(2)`

..  math::
        \frac{{\mathrm{d}}^{m}\beta^{n}(x)}{{\mathrm{d}}x^{m}}=\sum_{k=0}^{n+1}\,{\color{blue}\left(-1\right)^{k}}\,{n+1\choose k}\,\varsigma^{n-m}(x+\frac{n+1}{2}-k).

(To compute a non-differentiated B-spline, simply set :math:`m=0.`) Unfortunately, the term :math:`{\color{blue}\left(-1\right)^{k}}` results in additive contributions that tend to cancel each other. This spells numerical trouble, even if the polynomial simple element :math:`\varsigma^{n}:{\mathbb{R}}\rightarrow{\mathbb{R}},x\mapsto\varsigma^{n}(x)=\frac{1}{2\,n!}\,{\mathrm{sgn}}(x)\,x^{n}` has the flavor of a well-behaved canonic monomial. Another issue lies with the growth of the binomial coefficients with respect to the degree, which ultimately leads to a delicate balance between large numbers.

De Boor
^^^^^^^

Other computational recipes have been devised. For instance, the De Boor's recurrence relation expresses a B-spline of some degree as a weighted combination of B-splines of lesser degree. As defined in :math:`(2),` it turns out that :math:`\beta^{0}(x)=\left(\varsigma^{0}(x+\frac{1}{2})-\varsigma^{0}(x-\frac{1}{2})\right),` which is the base case. Then, the recursive step for :math:`n\in{\mathbb{N}}+1` is such that

:math:`(3)`

..  math::
        \beta^{n}(x)=\frac{1}{n}\,\left(\left(x+\frac{n+1}{2}\right)\,\beta^{n-1}(x+\frac{1}{2})-\left(x-\frac{n+1}{2}\right)\,\beta^{n-1}(x-\frac{1}{2})\right).

splinekit
^^^^^^^^^

In the ``splinekit`` library, B-splines of degree :math:`n=0` are computed as in the classic or the De Boor's approach. B-splines of positive degree, however, are computed as the scalar product

:math:`(4)`

..  math::
        \beta^{n}(x)={\mathbf{[\![}}\left|x\right|<\frac{n+1}{2}\,{\mathbf{]\!]}}\,\left(w^{n}[r][\cdot]\right)^{{\mathsf{T}}}\,{\mathbf{v}}^{n}(\chi),

where the notation :math:`{\mathbf{[\![}}\cdot\,{\mathbf{]\!]}}` is that of the Iverson bracket. Moreover, :math:`r=\left\lceil\xi\right\rceil\in{\mathbb{Z}}` for :math:`\xi=\left(\frac{n-1}{2}-x\right),` and :math:`\chi=\left(r-\xi\right)\in[0,1).` Finally, :math:`\left(w^{n}[r][\cdot]\right)^{{\mathsf{T}}}` is the :math:`(r+1)`-th row of the B-spline evaluation matrix :math:`{\mathbf{W}}^{n}\in{\mathbb{Q}}^{\left(n+1\right)\times\left(n+1\right)}` and :math:`{\mathbf{v}}^{n}(\chi)\in{\mathbb{R}}^{n+1}` is the Vandermonde vector of argument :math:`\chi` and degree :math:`n.` In this formulation of a B-spline, the fact that the Vandermonde vector has the domain :math:`[0,1)` greatly favors numerical stability since the image of each of its components is :math:`[0,1].`

The matrix :math:`{\mathbf{W}}^{n}` depends on the degree only and can be precomputed and cached. Its component at the :math:`(r+1)`-th row and :math:`(c+1)`-th column is rational and is defined as

:math:`(5)`

..  math::
        w_{r+1,c+1}^{n}=w^{n}[r][c]=\frac{1}{c!}\,\left(\left.\frac{{\mathrm{d}}^{c}\beta^{n}(x)}{{\mathrm{d}}x^{c}}\right|_{x=\frac{n-1}{2}-r}+\frac{1}{2}\,{\mathbf{[\![}}c=n\,{\mathbf{]\!]}}\,\left(-1\right)^{n-r}\,{n+1\choose r+1}\right).

The B-spline derivatives that appear in :math:`(5)` are computed as rational numbers, through :math:`(2).` This leads to a rational value for :math:`w^{n}[r][c],` which is then cached as its ``float`` approximation. With this strategy, the balancing act that was governed in :math:`(2)` by the term :math:`{\color{blue}\left(-1\right)^{k}}` is now applied to rational values and can be achieved exactly in the rational domain.

----

Experiments
-----------

We want to compare four computational approaches.

1.  In the first (ground-truth) approach, we rely on the observation that :math:`\beta^{n}:{\mathbb{R}}\rightarrow{\mathbb{R}}` is defined in :math:`(2)` in such a way that :math:`\beta^{n}` would map a rational number to a number that is rational, too, so that :math:`\beta^{n}:{\mathbb{Q}}\rightarrow{\mathbb{Q}}.` This allows us to compute exact, ground-truth values by relying on Python's built-in ``Fraction`` type.
2.  In the second (classic) approach, we compute :math:`\beta^{n}` as in :math:`(2),` this time relying on Python's built-in ``float`` type.
3.  In the third (De Boor) approach, we compute :math:`\beta^{n}` as in :math:`(3),` again relying on Python's built-in ``float`` type.
4.  In the fourth (splinekit) approach, we compute :math:`\beta^{n}` as in :math:`(4),` relying on Python's built-in ``float`` type.

What we measure is by how far the methods depart from the ground truth, in a mean-square sense expressed in dB units.

..  danger::

    *   The link below allows you to access and run the *installation-free* notebook. While the reported SNRs are representative, the timings are not because the Python kernel is WebAssembly-based and does not run natively.

    *   If you want to test for realistic timings on your own computer, then you will have to first install in full the ``splinekit`` library. Only after that will you be able to launch the notebook either as a regular, full-fledged Jupyter Lab or as a module executed by the native Python kernel.

    *   The timings reported in the Results Sections correspond to those of the native execution.

..  admonition:: Jupyter Lab notebook

    `Numeric stability and speed of B-splines <https://splinekit.github.io/splinekit-jupyterlite/notebooks/index.html?path=bspline_numeric_stability.ipynb>`_

..  hint::
    The notebook is available for download in compressed form from
    :download:`here <bspline_numeric_stability.ipynb.gz>`. Decompression is achieved from the terminal with ``gunzip bspline_numeric_stability.ipynb.gz``.

Critical Regime
^^^^^^^^^^^^^^^

When expressed as a signal-to-noise ratio (SNR), the error is most noticeable near the end of the left and right tails of the B-spline. In this critical regime, the error is computed over :math:`400` samples taken at random, consistently between methods to allow for their fair comparison. We also report the computational time.

Results
"""""""

On a desktop computer of 2021, a typical resulting table is as follows.

:raw-html:`<TABLE frame="hsides" rules="groups">
<CAPTION>Accuracy and time in the critical regime</CAPTION>
<COLGROUP>
<COLGROUP span="2">
<COLGROUP span="2">
<COLGROUP span="2">
<COLGROUP span="2">
<TR><TH><TH colspan="2" align="center">&#160;Ground-Truth&#160;<TH colspan="2" align="center">&#160;Classic&#160;<TH colspan="2" align="center">&#160;De Boor&#160;<TH colspan="2" align="center">&#160;splinekit&#160;
<TR><TH>&#160;Degree&#160;<TH>&#160;SNR[dB]&#160;&#160;<TH>&#160;Time[s]&#160;<TH>&#160;SNR[dB]&#160;&#160;<TH>&#160;Time[s]&#160;<TH>&#160;SNR[dB]&#160;&#160;<TH>&#160;Time[s]&#160;<TH>&#160;SNR[dB]&#160;&#160;<TH>&#160;Time[s]&#160;
<TBODY>
<TR><TD align="right">0&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">3.1e-03&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">2.6e-04&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">1.6e-04&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">1.2e-04&#160;
<TR><TD align="right">1&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">7.2e-03&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">5.5e-04&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">3.2e-04&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">1.4e-03&#160;
<TR><TD align="right">2&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">1.0e-02&#160;<TD align="right">309.4&#160;&#160;<TD align="right">6.7e-04&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">6.3e-04&#160;<TD align="right">327.2&#160;&#160;<TD align="right">1.5e-03&#160;
<TR><TD align="right">3&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">1.4e-02&#160;<TD align="right">297.7&#160;&#160;<TD align="right">7.5e-04&#160;<TD align="right">320.9&#160;&#160;<TD align="right">1.3e-03&#160;<TD align="right">318.6&#160;&#160;<TD align="right">1.5e-03&#160;
<TR><TD align="right">4&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">1.8e-02&#160;<TD align="right">282.3&#160;&#160;<TD align="right">8.7e-04&#160;<TD align="right">319.3&#160;&#160;<TD align="right">2.5e-03&#160;<TD align="right">313.0&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">5&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">2.2e-02&#160;<TD align="right">262.1&#160;&#160;<TD align="right">9.9e-04&#160;<TD align="right">317.5&#160;&#160;<TD align="right">5.0e-03&#160;<TD align="right">310.6&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">6&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">2.7e-02&#160;<TD align="right">243.6&#160;&#160;<TD align="right">1.2e-03&#160;<TD align="right">317.2&#160;&#160;<TD align="right">9.9e-03&#160;<TD align="right">303.3&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">7&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">3.1e-02&#160;<TD align="right">224.9&#160;&#160;<TD align="right">1.4e-03&#160;<TD align="right">314.9&#160;&#160;<TD align="right">2.0e-02&#160;<TD align="right">302.2&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">8&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">3.7e-02&#160;<TD align="right">199.3&#160;&#160;<TD align="right">1.4e-03&#160;<TD align="right">315.2&#160;&#160;<TD align="right">4.0e-02&#160;<TD align="right">295.9&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">9&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">4.3e-02&#160;<TD align="right">171.9&#160;&#160;<TD align="right">1.6e-03&#160;<TD align="right">315.3&#160;&#160;<TD align="right">7.9e-02&#160;<TD align="right">294.5&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">10&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">5.0e-02&#160;<TD align="right">143.8&#160;&#160;<TD align="right">1.7e-03&#160;<TD align="right">313.5&#160;&#160;<TD align="right">1.6e-01&#160;<TD align="right">290.8&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">11&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">5.7e-02&#160;<TD align="right">120.2&#160;&#160;<TD align="right">1.8e-03&#160;<TD align="right">314.2&#160;&#160;<TD align="right">3.2e-01&#160;<TD align="right">287.6&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">12&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">6.5e-02&#160;<TD align="right">94.4&#160;&#160;<TD align="right">2.0e-03&#160;<TD align="right">312.2&#160;&#160;<TD align="right">6.4e-01&#160;<TD align="right">281.7&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">13&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">7.4e-02&#160;<TD align="right">72.6&#160;&#160;<TD align="right">2.3e-03&#160;<TD align="right">310.9&#160;&#160;<TD align="right">1.3e+00&#160;<TD align="right">284.1&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">14&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">8.3e-02&#160;<TD align="right">45.4&#160;&#160;<TD align="right">2.4e-03&#160;<TD align="right">310.3&#160;&#160;<TD align="right">2.5e+00&#160;<TD align="right">277.3&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">15&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">9.2e-02&#160;<TD align="right">15.9&#160;&#160;<TD align="right">2.6e-03&#160;<TD align="right">310.0&#160;&#160;<TD align="right">5.1e+00&#160;<TD align="right">273.7&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">16&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">1.0e-01&#160;<TD align="right">-13.4&#160;&#160;<TD align="right">2.7e-03&#160;<TD align="right">312.6&#160;&#160;<TD align="right">1.0e+01&#160;<TD align="right">270.1&#160;&#160;<TD align="right">1.3e-03&#160;
</TABLE>`

Discussion for the Critical Regime
""""""""""""""""""""""""""""""""""

-   The numerical stability of the classic approach is consistently the worst at all degrees. Moreover, it even collapses when the degree rises: for degree :math:`n=16` already, there is more noise than signal, at least over the domain being examined, which is :math:`(-8.5,-6.5)\cup(6.5,8.5)` for :math:`n=16.` The computational cost exhibits a linear increase over the degrees explored here. Yet, the classic approach is still the fastest over the goldilocks range of degrees :math:`n\in\{3,4,5,6\}`, a range where its accuracy can also be considered sufficient for most purposes.
-   The numerical stability of the De Boor's approach is always the best (310dB corresponds to about 51 significant bits) but comes at a computational cost that increases combinatorially with the degree. Yet, the trivial code proposed here comes the fastest over the small range of degrees :math:`n\in\{0,1,2\}`. For such low degrees, however, one has to realize that the computational cost depends more on language idiosyncrasies (*e.g.*, namespace lookup, recursivity *vs* loops, checks on type and validity of the parameters) than on algorithmic considerations.
-   The numerical stability of the splinekit library is consistently much higher than that of the classic approach and degrades more slowly with the degree (270dB corresponds to about :math:`45` significant bits). Indeed, one has to reach the very high degree :math:`n=94` before the :math:`0` dB threshold is crossed, at least over the domain being examined, which is :math:`(-47.5,-45.5)\cup(45.5,47.5)` for :math:`n=94.`
-   For degree :math:`n=16`, splinekit is more than seven thousand times faster than De Boor while, for degree :math:`n=7` and higher, the splinekit strategy is always faster than the other three. The computation time of splinekit remains constant through all degrees, a property that holds up to :math:`n=94` and beyond.

Global Regime, Low Degrees
^^^^^^^^^^^^^^^^^^^^^^^^^^

In the global regime, the error is computed over :math:`400` samples taken at random according to a uniform distribution that is not restricted to the tails of B-splines but that covers their whole support, consistently between methods to allow for their fair comparison.

Here, we consider only low degrees. This allows us to include results for the (slow) De Boor's method.

Results
"""""""

On a desktop computer of 2021, a typical resulting table is as follows.

:raw-html:`<TABLE frame="hsides" rules="groups">
<CAPTION>Accuracy and time in the global regime, low degrees</CAPTION>
<COLGROUP>
<COLGROUP span="2">
<COLGROUP span="2">
<COLGROUP span="2">
<COLGROUP span="2">
<TR><TH><TH colspan="2" align="center">&#160;Ground-Truth&#160;<TH colspan="2" align="center">&#160;Classic&#160;<TH colspan="2" align="center">&#160;De Boor&#160;<TH colspan="2" align="center">&#160;splinekit&#160;
<TR><TH>&#160;Degree&#160;<TH>&#160;SNR[dB]&#160;&#160;<TH>&#160;Time[s]&#160;<TH>&#160;SNR[dB]&#160;&#160;<TH>&#160;Time[s]&#160;<TH>&#160;SNR[dB]&#160;&#160;<TH>&#160;Time[s]&#160;<TH>&#160;SNR[dB]&#160;&#160;<TH>&#160;Time[s]&#160;
<TBODY>
<TR><TD align="right">0&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">4.2e-03&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">3.8e-04&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">1.6e-04&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">1.3e-04&#160;
<TR><TD align="right">1&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">9.0e-03&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">5.4e-04&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">3.1e-04&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">1.4e-03&#160;
<TR><TD align="right">2&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">1.1e-02&#160;<TD align="right">308.6&#160;&#160;<TD align="right">6.6e-04&#160;<TD align="right">322.4&#160;&#160;<TD align="right">6.3e-04&#160;<TD align="right">319.8&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">3&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">1.4e-02&#160;<TD align="right">299.6&#160;&#160;<TD align="right">7.8e-04&#160;<TD align="right">319.6&#160;&#160;<TD align="right">1.3e-03&#160;<TD align="right">319.1&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">4&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">1.8e-02&#160;<TD align="right">287.2&#160;&#160;<TD align="right">9.1e-04&#160;<TD align="right">316.7&#160;&#160;<TD align="right">2.5e-03&#160;<TD align="right">316.5&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">5&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">2.2e-02&#160;<TD align="right">276.6&#160;&#160;<TD align="right">1.0e-03&#160;<TD align="right">316.6&#160;&#160;<TD align="right">5.0e-03&#160;<TD align="right">314.7&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">6&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">2.7e-02&#160;<TD align="right">264.3&#160;&#160;<TD align="right">1.2e-03&#160;<TD align="right">316.0&#160;&#160;<TD align="right">9.9e-03&#160;<TD align="right">317.3&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">7&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">3.2e-02&#160;<TD align="right">258.0&#160;&#160;<TD align="right">1.3e-03&#160;<TD align="right">317.1&#160;&#160;<TD align="right">2.0e-02&#160;<TD align="right">320.7&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">8&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">3.8e-02&#160;<TD align="right">243.2&#160;&#160;<TD align="right">1.5e-03&#160;<TD align="right">317.1&#160;&#160;<TD align="right">3.9e-02&#160;<TD align="right">313.7&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">9&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">4.4e-02&#160;<TD align="right">230.4&#160;&#160;<TD align="right">1.6e-03&#160;<TD align="right">316.4&#160;&#160;<TD align="right">7.9e-02&#160;<TD align="right">314.8&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">10&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">5.1e-02&#160;<TD align="right">216.5&#160;&#160;<TD align="right">1.8e-03&#160;<TD align="right">315.9&#160;&#160;<TD align="right">1.6e-01&#160;<TD align="right">314.3&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">11&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">5.8e-02&#160;<TD align="right">212.0&#160;&#160;<TD align="right">1.9e-03&#160;<TD align="right">314.8&#160;&#160;<TD align="right">3.2e-01&#160;<TD align="right">313.3&#160;&#160;<TD align="right">1.2e-03&#160;
<TR><TD align="right">12&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">6.6e-02&#160;<TD align="right">198.8&#160;&#160;<TD align="right">2.1e-03&#160;<TD align="right">315.5&#160;&#160;<TD align="right">6.3e-01&#160;<TD align="right">313.4&#160;&#160;<TD align="right">1.2e-03&#160;
<TR><TD align="right">13&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">7.5e-02&#160;<TD align="right">192.8&#160;&#160;<TD align="right">2.3e-03&#160;<TD align="right">313.5&#160;&#160;<TD align="right">1.3e+00&#160;<TD align="right">313.4&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">14&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">8.4e-02&#160;<TD align="right">178.4&#160;&#160;<TD align="right">2.5e-03&#160;<TD align="right">314.4&#160;&#160;<TD align="right">2.5e+00&#160;<TD align="right">311.4&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">15&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">9.4e-02&#160;<TD align="right">172.7&#160;&#160;<TD align="right">2.6e-03&#160;<TD align="right">314.0&#160;&#160;<TD align="right">5.1e+00&#160;<TD align="right">317.0&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">16&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">1.0e-01&#160;<TD align="right">161.0&#160;&#160;<TD align="right">2.8e-03&#160;<TD align="right">312.9&#160;&#160;<TD align="right">1.0e+01&#160;<TD align="right">310.1&#160;&#160;<TD align="right">1.3e-03&#160;
</TABLE>`

Discussion for the Global Regime, Low Degrees
"""""""""""""""""""""""""""""""""""""""""""""

-   The timings of all methods are consistent with the timings discussed in the critical regime. One concludes that the computational costs do not depend on the value of the arguments of B-splines.
-   The numerical stability of the classic approach is again consistently the worst at all degrees. Yet, contrarily to the critical regime, it remains globally serviceable for the degrees considered here.
-   The numerical stability of the De Boor's approach is approximately the same in the critical regime or in the global regime.
-   The numerical stability of the splinekit library is on par with that of the De Boor's approach and consistently much higher than that of the classic approach. It does not degrade with the degree.

Global Regime
^^^^^^^^^^^^^

In the global regime, the error is computed over :math:`400` samples taken at random according to a uniform distribution that is not restricted to the tails of B-splines but that covers their whole support, consistently between methods to allow for their fair comparison.

Here, we consider degrees too high to allow the (slow) De Boor's method to terminate computations in a reasonable time.

Results
"""""""

On a desktop computer of 2021, a typical resulting table is as follows.

:raw-html:`<TABLE frame="hsides" rules="groups">
<CAPTION>Accuracy and time in the global regime</CAPTION>
<COLGROUP>
<COLGROUP span="2">
<COLGROUP span="2">
<COLGROUP span="2">
<TR><TH><TH colspan="2" align="center">&#160;Ground-Truth&#160;<TH colspan="2" align="center">&#160;Classic&#160;<TH colspan="2" align="center">&#160;splinekit&#160;
<TR><TH>&#160;Degree&#160;<TH>&#160;SNR[dB]&#160;&#160;<TH>&#160;Time[s]&#160;<TH>&#160;SNR[dB]&#160;&#160;<TH>&#160;Time[s]&#160;<TH>&#160;SNR[dB]&#160;&#160;<TH>&#160;Time[s]&#160;
<TBODY>
<TR><TD align="right">0&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">3.3e-03&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">3.8e-04&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">1.1e-04&#160;
<TR><TD align="right">1&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">7.9e-03&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">5.6e-04&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">2&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">1.2e-02&#160;<TD align="right">307.2&#160;&#160;<TD align="right">6.8e-04&#160;<TD align="right">319.3&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">3&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">1.4e-02&#160;<TD align="right">300.6&#160;&#160;<TD align="right">8.0e-04&#160;<TD align="right">318.6&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">4&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">1.8e-02&#160;<TD align="right">285.4&#160;&#160;<TD align="right">9.1e-04&#160;<TD align="right">316.9&#160;&#160;<TD align="right">1.2e-03&#160;
<TR><TD align="right">5&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">2.2e-02&#160;<TD align="right">275.8&#160;&#160;<TD align="right">1.1e-03&#160;<TD align="right">315.4&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">6&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">2.8e-02&#160;<TD align="right">267.0&#160;&#160;<TD align="right">1.2e-03&#160;<TD align="right">316.3&#160;&#160;<TD align="right">1.4e-03&#160;
<TR><TD align="right">7&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">3.2e-02&#160;<TD align="right">257.1&#160;&#160;<TD align="right">1.4e-03&#160;<TD align="right">319.5&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">8&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">3.7e-02&#160;<TD align="right">243.9&#160;&#160;<TD align="right">1.5e-03&#160;<TD align="right">314.2&#160;&#160;<TD align="right">1.2e-03&#160;
<TR><TD align="right">9&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">4.8e-02&#160;<TD align="right">231.7&#160;&#160;<TD align="right">1.6e-03&#160;<TD align="right">314.2&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">10&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">5.3e-02&#160;<TD align="right">219.7&#160;&#160;<TD align="right">1.8e-03&#160;<TD align="right">313.8&#160;&#160;<TD align="right">1.2e-03&#160;
<TR><TD align="right">11&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">5.9e-02&#160;<TD align="right">209.2&#160;&#160;<TD align="right">1.9e-03&#160;<TD align="right">313.4&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">12&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">6.6e-02&#160;<TD align="right">200.4&#160;&#160;<TD align="right">2.0e-03&#160;<TD align="right">313.2&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">13&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">7.4e-02&#160;<TD align="right">189.9&#160;&#160;<TD align="right">2.3e-03&#160;<TD align="right">311.7&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">14&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">8.4e-02&#160;<TD align="right">182.4&#160;&#160;<TD align="right">2.5e-03&#160;<TD align="right">313.1&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">15&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">9.3e-02&#160;<TD align="right">169.6&#160;&#160;<TD align="right">2.6e-03&#160;<TD align="right">317.8&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">16&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">1.0e-01&#160;<TD align="right">160.3&#160;&#160;<TD align="right">2.7e-03&#160;<TD align="right">309.6&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">17&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">1.1e-01&#160;<TD align="right">147.7&#160;&#160;<TD align="right">2.9e-03&#160;<TD align="right">310.3&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">18&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">1.2e-01&#160;<TD align="right">135.4&#160;&#160;<TD align="right">3.0e-03&#160;<TD align="right">311.6&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">19&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">1.4e-01&#160;<TD align="right">115.7&#160;&#160;<TD align="right">3.1e-03&#160;<TD align="right">311.7&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">20&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">1.5e-01&#160;<TD align="right">105.2&#160;&#160;<TD align="right">3.3e-03&#160;<TD align="right">310.6&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">21&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">1.6e-01&#160;<TD align="right">96.4&#160;&#160;<TD align="right">4.4e-03&#160;<TD align="right">310.1&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">22&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">1.8e-01&#160;<TD align="right">84.6&#160;&#160;<TD align="right">4.7e-03&#160;<TD align="right">310.1&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">23&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">2.0e-01&#160;<TD align="right">81.5&#160;&#160;<TD align="right">4.8e-03&#160;<TD align="right">310.1&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">24&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">2.1e-01&#160;<TD align="right">65.2&#160;&#160;<TD align="right">5.2e-03&#160;<TD align="right">309.5&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">25&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">2.2e-01&#160;<TD align="right">59.4&#160;&#160;<TD align="right">5.3e-03&#160;<TD align="right">310.9&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">26&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">2.4e-01&#160;<TD align="right">49.3&#160;&#160;<TD align="right">5.6e-03&#160;<TD align="right">310.7&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">27&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">2.6e-01&#160;<TD align="right">41.6&#160;&#160;<TD align="right">5.9e-03&#160;<TD align="right">312.4&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">28&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">2.9e-01&#160;<TD align="right">29.3&#160;&#160;<TD align="right">6.3e-03&#160;<TD align="right">309.7&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">29&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">3.0e-01&#160;<TD align="right">20.9&#160;&#160;<TD align="right">6.5e-03&#160;<TD align="right">309.9&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">30&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">3.2e-01&#160;<TD align="right">8.8&#160;&#160;<TD align="right">6.8e-03&#160;<TD align="right">309.7&#160;&#160;<TD align="right">1.3e-03&#160;
<TR><TD align="right">31&#160;<TD align="right">&#8734;&#160;&#160;<TD align="right">3.4e-01&#160;<TD align="right">0.6&#160;&#160;<TD align="right">6.9e-03&#160;<TD align="right">321.3&#160;&#160;<TD align="right">1.3e-03&#160;
</TABLE>`

Discussion for the Global Regime
""""""""""""""""""""""""""""""""

-   The numerical stability of the classic approach degrades sharply near the end of the range of degrees that we considered here, to the point of uselessness.
-   The numerical stability of the splinekit library is as good as the floating-point representation of numbers will allow, at all degrees shown here.

