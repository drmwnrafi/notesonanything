# **Binomial to Poisson Distribution**

Given that $n$ is the number of Bernoulli trials and $p$ is the probability of success, we have:

$$
P(\mathbf{X} = 1) = p \\
P(\mathbf{X} = 0) = q = 1 - p
$$

If $n \to \infty$, $p \to 0$, and $n \cdot p$ or $n \cdot q$ remains constant, the distribution approaches the **Poisson Distribution**.

$$
\mathbf{X} \sim \text{Binomial}(n, p) \rightarrow \mathbf{X} \sim \text{Poisson}(\lambda)
$$

where $\lambda = n \cdot p$ is the parameter for the Poisson Distribution.

Let $\lambda = np$, $p = \frac{\lambda}{n}$, and $\mathbf{X} \sim \text{Binomial}(n, p)$.

$$
\begin{align*}
P(\mathbf{X} = k) &= \binom{n}{k} p^k (1 - p)^{n - k} \\
&= \frac{n!}{k!(n - k)!} \left( \frac{\lambda}{n} \right)^k \left( 1 - \frac{\lambda}{n} \right)^{n - k} \\
&= \frac{\lambda^k}{k!} \frac{n!}{(n - k)!} \left( \frac{\lambda}{n} \right)^k \left( 1 - \frac{\lambda}{n} \right)^{n - k} \\
&= \frac{\lambda^k}{k!} \frac{n(n - 1)(n - 2) \cdots (n - k + 1)}{n^k} \left( 1 - \frac{\lambda}{n} \right)^{n - k}
\end{align*}
$$

As $n$ approaches a large number of trials, $n \to \infty$, we have:

$$
\lim_{n \to \infty} \left( 1 - \frac{\lambda}{n} \right)^n = e^{-\lambda}
$$

$$
\lim_{n \to \infty} \left( 1 - \frac{1}{n} \right) \left( 1 - \frac{2}{n} \right) \cdots \left( 1 - \frac{k + 1}{n} \right) = 1
$$

$$
\lim_{n \to \infty} \left( 1 - \frac{\lambda}{n} \right)^{-k} = 1
$$

Thus, we obtain the Poisson distribution:

$$
P(\mathbf{X} = k) = \frac{\lambda^k e^{-\lambda}}{k!}
$$
