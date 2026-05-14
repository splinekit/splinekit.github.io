Spline Bases
============

Illustration of the B-spline basis  :math:`\beta,` the cardinal B-spline basis :math:`\eta,` the dual B-spline basis :math:`\mathring{\beta},` and the orthonormal B-spline basis :math:`\phi.`

----

Roadmap
-------

We are first going to give the intuition of what a basis is, by making a detour with the representation of numbers. Then, we introduce uniform splines as weighted sums of integer-shifted bases. We go into greater depth in our general discussion of bases, and then focus on the specific case of the B-spline basis, of which we give just a few of their numerous properties. We end with the illustration of three other kinds of bases.

----

Representation of Numbers
-------------------------

Unlike in the antique world, the modern representation of numbers is positional, with digits. For instance, the number forty-two consists of the decimal digits :math:`4` and :math:`2,` with :math:`4` to the left and :math:`2` to the right.

Digits are the construction blocks of numbers. They are associated to a *base* (not a basis); in everyday life, the base is ten. In a computer, the binary base is two. Geeks would write octal numbers in base eight or hexadecimal numbers in base sixteen. Assuming the base is written as a base-ten subscript, one would notate

..  math::
        42_{10}=101010_{2}=52_{8}=2{\mathrm{A}}_{16}.

Formally, if :math:`b` is the base, the synthesis of a nonnegative and finite integer number :math:`m\in{\mathbb{N}}` from its digits :math:`\left(d[k]\right)_{k\in{\mathbb{N}}}` is understood to be

..  math::
        m=\sum_{k\in{\mathbb{N}}}\,d[k]\,b^{k},\;m\in{\mathbb{N}}.

In written form, the digits are ordered by their index, from high index to low index if one goes from left to right. While the synthesis operation involves infinitely many digits, the finite nature of :math:`m\in{\mathbb{N}}` imposes that :math:`d[k]=0` when :math:`k` is sufficiently large. For convenience, the writing of those leftmost digits of value zero is omitted, which results in a finite-length string even if the list of digits is implicitly infinite—at least, to the left.

The finite characteristic of the string may be of a different nature. Typically, your everyday-life number might possibly be an integer, but sometimes it is also an irrational number :math:`x\in{\mathbb{R}}\setminus{\mathbb{Q}}.` In this case, the representation is extended by considering digits :math:`\left(d[k]\right)_{k\in{\mathbb{Z}}}` with both negative and positive indices, like in

..  math::
        x=\sum_{k\in{\mathbb{Z}}}\,d[k]\,b^{k},\;x\in{\mathbb{R}},

but, by practical necessity, the written form requires a finite-length string. One achieves this by omitting the zeros to the left and truncating the digits to the right. Yet, conceptually, the number of digits is still infinite.

We have now made the point that a number can be synthesized by an *infinite* sum of *indexed weights* (the digits) that multiply some *index-dependent operation* on a base (the canonic number :math:`b` to the power of the index). We have omitted details such as the handling of the sign, or discussed the assumption that the base is an integer or, if it is indeed an integer :math:`b\in{\mathbb{N}}+2,` that the digits take integer values restricted to :math:`d[k]\in[0\ldots b-1].` The discussion of the unicity of the representation has been left aside, too.

Uniform Splines
^^^^^^^^^^^^^^^

Uniform splines are real functions. In other words, they are mappings that associate an input real number to some (often different) output number. To be called uniform splines, these functions are not entirely arbitrary but obey some rules and follow some structure. Specifically, they are number-like, written as the functional recipe

..  math::
        f:{\mathbb{R}}\rightarrow{\mathbb{R}},x\mapsto f(x)=\sum_{k\in{\mathbb{Z}}}\,c[k]\,\varphi(x-k).

There, the indexed weights :math:`\left(c[k]\right)_{k\in{\mathbb{Z}}}` play the same role as the digits :math:`\left(d[k]\right)_{k\in{\mathbb{Z}}}` of numbers, while the *basis* (not the base) :math:`\varphi` plays the same role as the base :math:`b` of numbers. However, whereas the base was raised to the integer power :math:`k,` the basis now is shifted by the integer amount :math:`k.` In other words, the coefficients are to the spline what the digits are to the string representation of a number, and the basis is to the spline what the (decimal or binary) base is to the string representation of a number.

In some elaborate versions of splines, the shift is allowed to take non-integer values and even to vary with the index; here, we limit ourselves to integer shifts, which is the reason why such splines are said to be *uniform*.

Bases
^^^^^

Given the number :math:`x`, many a base :math:`b` can be adopted to represent its value, with adjusted digits :math:`d`. Likewise, given a spline :math:`f`, many a basis :math:`\varphi` can be adopted to represent the function, with adjusted weights :math:`c`. For :math:`f` to be truly called a spline, however, the basis itself must adhere to some specific constraints. As we consider only *polynomial* uniform splines of nonnegative integer degree :math:`n\in{\mathbb{N}}`, the basis :math:`\varphi` must

*   be a piecewise polynomial of degree :math:`n` with pieces of unit length;
*   be :math:`\left(n-1\right)`-times continuously differentiable and :math:`n`-times differentiable;
*   have its :math:`n`-th derivative be necessarily discontinous in at least one place.

These properties go a long way but are not sufficient. The true technical requirements demand for an understanding that goes beyond calculus and we do not reproduce them here.

In its canonical form, the basis is possibly the odd- or even-symmetric *polynomial simple element* of nonnegative integer degree :math:`n\in{\mathbb{N}},` a function notated :math:`\varsigma^{n}` with :math:`n` a superscript—not a power. It maps :math:`x\in{\mathbb{R}}` to :math:`\varsigma^{n}(x)=\frac{1}{2\,n!}\,{\mathrm{sgn}}(x)\,x^{n}.` Alternatively, it can also be the one-sided function that maps :math:`x\in{\mathbb{R}}_{<0}` to :math:`0` and :math:`x\in{\mathbb{R}}_{\geq0}` to :math:`x^{n}.` However, unlike :math:`\varsigma^{n},` the one-sided function is restricted to those integer degrees that are positive since it is not well-defined at :math:`x=0` for :math:`n=0`.

These canonic versions have their uses since their simplicity facilitates theoretical developments. However, they are terribly bad in practice. In any case, the important thing to remember is that, just like one given number admits a representation in any arbitrary base, one given spline admits a representation in any arbitrary basis.

----

B-Spline
--------

A basis that ticks many marks, both practical and theoretical, is the so-called polynomial B-spline :math:`\beta^{n}` of nonnegative integer degree :math:`n,` with :math:`n` a superscript. Indeed, a B-spline is the function that is used most often as construction block to build splines. The letter B in B-spline stands for *basis*. In practice, its most appealing property is its finite support, which is conductive to fast algorithms. At the same time, it has remarkable properties beyond those linked to differentiablity, a few of them being as follows.

*   **Explicit Formula** A finite number of arithmetic operations is all it takes to compute a B-spline.
*   **Rational Mapping** B-splines map rational numbers :math:`x\in{\mathbb{Q}}` to rational numbers, which allows computers to make exact rational computations, if desired.
*   **Finite Support** B-splines are bump-like functions that take the value zero except on a central interval of diameter :math:`n+1.`
*   **Symmetry** B-splines are even-symmetric.
*   **Nonnegativity** B-splines never take a negative value.
*   **Boundedness** B-splines are nonnegative and never exceed the value :math:`1.`
*   **Multiscale Relations** B-splines can be expressed exactly as a weighted sum of shifted B-splines downscaled by an arbitrary positive integer factor. The shifts are trivial and the weights are easy to compute exactly.
*   **Order of Approximation** In the theory of approximation, B-splines achieve the desirable property of a maximal order of approximation for their support. In particular, they satisfy the *partition of unity*.

While a B-spline has many favorable properties, it comes at a price: it is not interpolating, which means that it is not trivial to determine the coefficients that ensure that the spline takes imposed values. More precisely, given the sequence :math:`\left(y[q]\right)_{q\in{\mathbb{Z}}}`, some work is required to find :math:`c_{1}` such that

..  math::
        \left(y[q]\right)_{q\in{\mathbb{Z}}}=\left(\sum_{k\in{\mathbb{R}}}\,c_{1}[k]\,\beta^{n}(q-k)\right)_{q\in{\mathbb{Z}}}.

In the sequel, the B-spline basis provides the reference to which all other bases will be compared. We set

..  math::
        \begin{array}{rcl}
        f:{\mathbb{R}}\rightarrow{\mathbb{R}},\;x\mapsto f(x)&=&\sum_{k\in{\mathbb{R}}}\,c[k]\,\varphi(x-k)\\
        &=&\sum_{k\in{\mathbb{R}}}\,c_{1}[k]\,\beta^{n}(x-k).
        \end{array}

Here, we plot a B-spline over its support, indexed by its degree.

..  admonition:: Jupyter Lab notebook

    `Polynomial B-spline <https://splinekit.github.io/splinekit-jupyterlite/notebooks/index.html?path=bspline_member.ipynb>`_

----

Cardinal B-Spline
-----------------

There exists a basis :math:`\eta^{n}` that happens to be interpolating. It is called a cardinal B-spline. Some of its mathematical properties can be considered less favorable than those of B-splines; most notably, contrarily to B-splines, the support of cardinal B-splines is infinite, which makes it an impractical basis. Yet, its interpolating property, according to which :math:`\eta^{n}(0)=1` and :math:`\eta^{n}(k)=0` for :math:`k\in{\mathbb{Z}}\setminus\{0\},` greatly facilitates the determination of the coefficients of the representation. Indeed, it is enough to let :math:`c_{2}=y` to automatically get that

..  math::
        \left(y[q]\right)_{q\in{\mathbb{Z}}}=\left(\sum_{k\in{\mathbb{R}}}\,c_{2}[k]\,\eta^{n}(q-k)\right)_{q\in{\mathbb{Z}}}.

It then holds that

..  math::
        \begin{array}{rcl}
        f:{\mathbb{R}}\rightarrow{\mathbb{R}},\;x\mapsto f(x)&=&\sum_{k\in{\mathbb{R}}}\,c[k]\,\varphi(x-k)\\
        &=&\sum_{k\in{\mathbb{R}}}\,c_{1}[k]\,\beta^{n}(x-k)\\
        &=&\sum_{k\in{\mathbb{R}}}\,c_{2}[k]\,\eta^{n}(x-k).
        \end{array}


Here, we plot a cardinal B-spline, indexed by its degree.

..  admonition:: Jupyter Lab notebook

    `Cardinal B-spline <https://splinekit.github.io/splinekit-jupyterlite/notebooks/index.html?path=bspline_cardinal.ipynb>`_

Relation to the Cardinal Sine Function
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The function :math:`f:{\mathbb{R}}\rightarrow{\mathbb{R}},\;x\mapsto\sum_{k\in{\mathbb{R}}}\,c[k]\,\varphi(x-k)` is a spline only for certain bases :math:`\varphi.` A famous case where :math:`f` fails to be a spline arises if one chooses :math:`\sin(\pi\,x)/\left(\pi\,x\right)={\mathrm{sinc}}\,x` to play the role of the basis, which is associated to the celebrated Whittaker–Shannon interpolation formula and has strong theoretical properties.

As it turns out, the cardinal B-spline :math:`\eta^{n}` tends (in a functional sense) to :math:`{\mathrm{sinc}}` when :math:`n\rightarrow\infty.` The convergence, however, is poor. In particular, the tails of the :math:`{\mathrm{sinc}}` function decay in reciprocal fashion, a decay that is much gentler than the exponential decay followed by the tails of the cardinal B-spline.

We verify now visually over the range :math:`x\in[-15,15]` that
:math:`{\mathrm{sinc}}\,x=\lim_{n\rightarrow\infty}\eta^{n}(x),` albeit slowly.

..  admonition:: Jupyter Lab notebook

    `Convergence of the cardinal B-spline to the sinc function <https://splinekit.github.io/splinekit-jupyterlite/notebooks/index.html?path=bspline_sinc.ipynb>`_

----

Dual B-Spline
-------------

Let the basis :math:`\mathring{\beta}^{n_{2},n_{1}}` of dual degree :math:`n_{2}\in{\mathbb{N}}` and primal degree :math:`n_{1}\in{\mathbb{N}}` be called a dual B-spline. It is yet another basis that is sometimes used to provide an alternate (equivalent) representation of the spline as

..  math::
        \begin{array}{rcl}
        f:{\mathbb{R}}\rightarrow{\mathbb{R}},\;x\mapsto f(x)&=&\sum_{k\in{\mathbb{R}}}\,c[k]\,\varphi(x-k)\\
        &=&\sum_{k\in{\mathbb{R}}}\,c_{1}[k]\,\beta^{n}(x-k)\\
        &=&\sum_{k\in{\mathbb{R}}}\,c_{2}[k]\,\eta^{n}(x-k)\\
        &=&\sum_{k\in{\mathbb{R}}}\,c_{3}[k]\,\mathring{\beta}^{n,n_{1}}(x-k).
        \end{array}

Here, the equality of the representations of :math:`f` is achieved only if we set :math:`n_{2}=n,` while the primal degree :math:`n_{1}\in{\mathbb{N}}` is free. The defining property of a dual B-spline is that it is the unique spline of degree :math:`n_{2}` which, once convolved with a B-spline of degree :math:`n_{1},` results in a cardinal, interpolating spline. Formally, this is written as

..  math::
        \forall x\in{\mathbb{R}}:\eta^{n_{2}+n_{1}+1}(x)=\int_{{\mathbb{R}}}\,\mathring{\beta}^{n_{2},n_{1}}(y)\,\beta^{n_{1}}(x-y)\,{\mathrm{d}}y.

Several algorithms have been designed to take advantage of the interpolating property of :math:`\mathring{\beta}^{n_{2},n_{1}}*\beta^{n_{1}}` and of the equivalence of the representation of :math:`f` through :math:`c_{1}` or :math:`c_{3}.`

Here, we show a dual B-spline indexed by its dual degree :math:`n_{2}` and its primal degree :math:`n_{1}.`

..  admonition:: Jupyter Lab notebook

    `Dual B-spline <https://splinekit.github.io/splinekit-jupyterlite/notebooks/index.html?path=bspline_dual.ipynb>`_

Defining Property of Dual B-Splines
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

We verify now visually over the range :math:`x\in[-15,15]` that
:math:`\eta^{n_{2}+n_{1}+1}(x)=\left(\mathring{\beta}^{n_{2},n_{1}}*\beta^{n_{1}}\right)(x).`

..  admonition:: Jupyter Lab notebook

    `Dual B-spline property <https://splinekit.github.io/splinekit-jupyterlite/notebooks/index.html?path=bspline_dual_property.ipynb>`_

----

Orthonormal B-Spline
--------------------

Let the orthonormal B-spline :math:`\phi^{n}` of degree :math:`n` be a final basis of interest that is sometimes used to provide an alternate (equivalent) representation of the spline as

..  math::
        \begin{array}{rcl}
        f:{\mathbb{R}}\rightarrow{\mathbb{R}},\;x\mapsto f(x)&=&\sum_{k\in{\mathbb{R}}}\,c[k]\,\varphi(x-k)\\
        &=&\sum_{k\in{\mathbb{R}}}\,c_{1}[k]\,\beta^{n}(x-k)\\
        &=&\sum_{k\in{\mathbb{R}}}\,c_{2}[k]\,\eta^{n}(x-k)\\
        &=&\sum_{k\in{\mathbb{R}}}\,c_{3}[k]\,\mathring{\beta}^{n,n_{1}}(x-k)\\
        &=&\sum_{k\in{\mathbb{R}}}\,c_{4}[k]\,\phi^{n}(x-k).
        \end{array}

The defining property of the orthonormal B-spline is that it is the unique (up to its sign) spline of degree :math:`n` which, once convolved with itself, results in a cardinal, interpolating spline. Formally, this is written as

..  math::
        \forall x\in{\mathbb{R}}:\eta^{2\,n+1}(x)=\int_{{\mathbb{R}}}\,\phi^{n}(y)\,\phi^{n}(x-y)\,{\mathrm{d}}y.

Several algorithms have been designed to take advantage of the interpolating property of :math:`\phi^{n}*\phi^{n}` and of the equivalence of the representation of :math:`f` through :math:`c_{1}` or :math:`c_{4}.`

Here, we show an orthonormal B-spline indexed by its degree :math:`n.`

..  admonition:: Jupyter Lab notebook

    `Orthonormal B-spline <https://splinekit.github.io/splinekit-jupyterlite/notebooks/index.html?path=bspline_ortho.ipynb>`_

Defining Property of Orthonormal B-Splines
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

We verify now visually over the range :math:`x\in[-15,15]` that
:math:`\eta^{2\,n+1}(x)=\left(\phi^{n}*\phi^{n}\right)(x).`

..  admonition:: Jupyter Lab notebook

    `Orthonormal B-spline property <https://splinekit.github.io/splinekit-jupyterlite/notebooks/index.html?path=bspline_ortho_property.ipynb>`_

