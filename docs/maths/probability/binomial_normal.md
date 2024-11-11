# **Binomial to Normal Distribution**

Given $n$ as the number of Bernoulli trials and $p$ as the probability of success, we have:

$$
P(\mathbf{X} = 1) = p \\
P(\mathbf{X} = 0) = q = 1 - p 
$$

If $n$ is large and $n \cdot p \cdot q$ is not too small, the distribution approaches a **Normal Distribution**:

$$
\mathbf{X} \sim \text{Binomial}(n, p) \rightarrow \mathbf{X} \sim \mathcal{N}(np, \sqrt{npq})
$$

where $np = \mu$ (mean), and $npq = \sigma^2$ (variance). The standard deviation is $\sigma$, and we can compute the probability using the cumulative distribution function (CDF) of the normal distribution.

The probability density function (PDF) of the normal distribution is:

$$
f(x) = \frac{1}{\sqrt{2\pi} \sigma} e^{-\frac{(x - \mu)^2}{2\sigma^2}}
$$

To calculate the probability of a specific random value $\mathbf{X}$ over the interval $[a, b]$ using the CDF, we have:

$$
P(a \leq \mathbf{X} \leq b) = \int_a^b f(x) \, dx = \int_a^b \frac{1}{\sqrt{2\pi} \sigma} e^{-\frac{(x - \mu)^2}{2\sigma^2}} \, dx
$$

It is easier to compute this for the standard normal distribution $\mathcal{N}(\mu = 0, \sigma = 1)$, also called the **z-distribution**. To standardize $\mathbf{X}$, a random variable with any $\mu$ and $\sigma$, to mean 0 and standard deviation 1, we compute the z-score $Z$ corresponding to $\mathbf{X}$:

$$
Z = \frac{\mathbf{X} - \mu}{\sigma}
$$

This equation shifts the mean of $\mathbf{X}$ to 0 and scales the distribution to have $\sigma = 1$. The standard normal PDF is:

$$
\phi(z) = \frac{1}{\sqrt{2\pi}} e^{-\frac{z^2}{2}}
$$

and the CDF of the standard normal distribution is:

$$
\Phi(z) = \int_{-\infty}^z \frac{1}{\sqrt{2\pi}} e^{-\frac{z^2}{2}} \, dz
$$

You can compute $P(Z \leq \text{z-score})$ using the z-table.
