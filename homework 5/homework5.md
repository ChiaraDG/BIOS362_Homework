### Suppose *x*<sub>*i*</sub>, i = 1, …, N are iid (*μ*, *σ*<sup>2</sup>). Let *x̄*<sub>1</sub><sup>\*</sup> and *x̄*<sub>2</sub><sup>\*</sup> be two bootstrap realizations of the sample mean. Show that the sampling correlation $cor(\\bar{x}\_1^{\*}, \\bar{x}\_2^{\*}) = \\frac{n}{2n-1} \\approx$ 50%. Along the way derive *v**a**r*(*x̄*<sub>1</sub><sup>\*</sup>) and the variance of the bagged mean *x̄*<sub>*b**a**g*</sub>. Here *x̄* is a linear statistic; bagging produces no reduction in variance for linear statistics.

First, we compute the variance of *x̄*<sub>1</sub><sup>\*</sup> (the
variance of *x̄*<sub>2</sub><sup>\*</sup> will be the same). Let,
$\\bar{x}\_j^{\*} = \\frac{1}{N}\\sum\_{i = 1}^{N} x^{\*}\_{ji}$ where
*x*<sub>*j**i*</sub><sup>\*</sup> are drawn with replacement from
{*x*<sub>1</sub>, ..., *x*<sub>*N*</sub>}. We know that within each
bootstrap sample; thus, conditioning on the *x*s in the sample:

$$
E\[\\bar{x}\_1^{\*} | x\_{1}, ..., x\_{N}\] = \\frac{1}{N}\\sum\_{i = 1}^NE\[x^{\*}\_{1i}\] = \\bar{x} \\implies E\[\\bar{x}\_1^{\*}\] = E\[E\[\\bar{x}\_1^{\*} | x\_{1}, ..., x\_{N}\]\] = \\mu
$$

and:

$$
Var(\\bar{x}\_1^{\*} | x\_{1}, ..., x\_{N}) = \\frac{Var(x\_{1i}^{\*}) }{N} = \\frac{1}{N}\\frac{\\sum\_{i}(x\_{1i} - \\bar{x}\_1)^2}{N} = \\frac{(N - 1)S^2}{N^2}
$$
 where $S^2 = \\frac{1}{N-1}\\sum\_{i = 1}(x\_{1i} - \\bar{x}\_1)^2$.

Using the law of iterative expectation:

$$
\\begin{aligned}
Var(\\bar{x}\_1^{\*}) &= E\[Var(\\bar{x}\_1^{\*} | x\_1, ...., x\_N)\] + Var(E\[\\bar{x}\_1^{\*} |x\_1, ..., x\_N\]) = E\\left\[\\frac{(N - 1)S^2}{N^2}\\right\] + Var\\left(\\bar{x}\\right) = \\frac{N - 1}{N^2}E\[S^2\] + \\frac{\\sigma^2}{N} \\\\
&= \\frac{(N - 1)\\sigma^2 +N\\sigma^2}{N^2} = \\frac{(2N - 1)\\sigma^2}{N^2}
\\end{aligned}
$$

The same result holds for *V**a**r*(*x̄*<sub>2</sub><sup>\*</sup>).
Second, we compute the
*C**o**v*(*x̄*<sub>1</sub><sup>\*</sup>, *x̄*<sub>2</sub><sup>\*</sup>).

$$
\\begin{aligned}
Cov(\\bar{x}\_1^{\*}, \\bar{x}\_2^{\*}) &= \\frac{1}{N^2}Cov\\left(\\sum\_{i = 1}^Nx^{\*}\_{1i}, \\sum\_{i = 1}^Nx^{\*}\_{2i} \\right) =  \\frac{1}{N^2}\\sum\_{i = 1}^{N}Cov(x^{\*}\_{1i},x^{\*}\_{2i}) = \\frac{\\sigma^2}{N}
\\end{aligned}
$$
 since
*C**o**v*(*x*<sub>1*i*</sub><sup>\*</sup>, *x*<sub>2*i*</sub><sup>\*</sup>)
is zero if
*x*<sub>1*i*</sub><sup>\*</sup> ≠ *x*<sub>2*i*</sub><sup>\*</sup> and
*σ*<sup>2</sup> if
*x*<sub>1*i*</sub><sup>\*</sup> = *x*<sub>2*i*</sub><sup>\*</sup>.
Putting the variance and covariance together we find:

$$
\\begin{aligned}
cor(\\bar{x}\_1^{\*}, \\bar{x}\_2^{\*}) &= \\frac{Cov(\\bar{x}\_1^{\*}, \\bar{x}\_2^{\*}) }{\\sqrt{Var(\\bar{x}\_1^{\*})Var(\\bar{x}\_2^{\*})}} = \\frac{\\sigma^2/N}{\\frac{(2N - 1)\\sigma^2}{N^2}} = \\frac{N}{2N - 1}
\\end{aligned}
$$

The variance of the bagged mean *x̄*<sub>*b**a**g*</sub> is given by:

$$
\\begin{aligned}
Var(\\bar{x}\_{bag}) &= Var\\left(\\frac{1}{2}(\\bar{x}\_1^{\*} + \\bar{x}\_2^{\*})\\right) = \\frac{1}{4}Var(\\bar{x}\_1^{\*} + \\bar{x}\_2^{\*}) = \\frac{1}{4}\\left\[Var(\\bar{x}\_1^{\*}) + Var(\\bar{x}\_2^{\*}) + 2Cov(\\bar{x}\_1^{\*}, \\bar{x}\_2^{\*})\\right\] \\\\
&= \\frac{1}{4}\\left\[2\\frac{(2N - 1)\\sigma^2}{N^2} + 2\\frac{\\sigma^2}{N}\\right\] = \\frac{1}{4}\\left\[\\frac{4N\\sigma^2 - 2\\sigma^2 + 2N\\sigma^2}{N^2}\\right\] = \\frac{(3N - 1)\\sigma^2}{2N^2}
\\end{aligned}
$$
