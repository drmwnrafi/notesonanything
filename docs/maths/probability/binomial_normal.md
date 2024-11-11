# **Binomial to Normal Distribution**

Given $n$ and $p$ are number bernouli trials and probability of success, respectively.

$$
P(\mathbf{X} = 1) = p \\
P(\mathbf{X} = 0) = q = 1-p 
$$

If $n$ large and $n\cdot p \cdot q$ not too small the distribution becomes {==Normal Distibution==},

$$
\mathbf{X} \sim \text{Binomial}(n, p) \rightarrow \mathbf{X} \sim \mathcal{N}(np, \sqrt{npq})
$$

where $np = \mu$ or mean, and $npq = \sigma^2$ or standart deviation or variance in terms of normal distribution, then we can compute the probability by using cummulative distribution function (CDF) of normal distribution.

Probability density fucntion of normal distribution is :

$$
f(x) = \frac{1}{\sqrt{2\pi} \sigma}e^{-\frac{(x-\mu)^2}{2\sigma^2}}
$$

Then, to calculate probability of specific random value $\mathbf{X}$ over interval $[a, b]$ by using CDF is

$$
P(a \leq \mathbf{X} \leq b) = \int_a^b f(x) \; dx= \int_a^b \frac{1}{\sqrt{2\pi} \sigma}e^{-\frac{(x-\mu)^2}{2\sigma^2}} \; dx
$$

It is easir to compute in standart normal distribution $\mathcal{N}(\mu=0, \sigma=1)$, also called {==z-distribution==}. To standarize $\mathbf{X}$, a random variable with any $\mu$ and $\sigma$ to 0 and 1. We need to compute z-score $Z$ corresponding to $\mathbf{X}$.

$$
Z = \frac{\mathbf{X}-\mu}{\sigma}
$$

Above equation shifts the mean of $\mathbf{X}$ to 0 and scale the distribution to have $\sigma=1$. Then, the standart normal PDF as follows :

$$
\phi(z) = \frac{1}{\sqrt{2\pi}}e^{-\frac{z^2}{2}}
$$

and the CDF of it is :

$$
\Phi(z) = \int_{-\infty}^z\frac{1}{\sqrt{2\pi}}e^{-\frac{z^2}{2}}\; dz
$$

Or compute $P(Z \leq \text{z-score})$ by using z-table.
