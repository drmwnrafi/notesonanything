# **Central Limit Theorem**

As large $n$ experiments with $\mathbf{X}_1, \mathbf{X}_2, \mathbf{X}_3, \cdots, \mathbf{X}_n$ as i.i.d. random variables are conducted, the sample mean will converge to the expected value. This leads to the Central Limit Theorem, regardless of the distribution of $\mathbf{X}_i$'s.

With the sample mean:

$$
\bar{\mathbf{X}}_n = \frac{1}{n}\sum_{i=1}^n \mathbf{X}_i \sim \mathcal{N}(\mu, \frac{\sigma^2}{n})
$$

{++Proof++}

The distribution of the normalized sum is:

$$
\begin{align*}
S_n &= \sum_{i=1}^n \mathbf{X}_i \\
NS_n &= \frac{S_n}{\sigma \sqrt{n}}
\end{align*}
$$

Proof Goal: 
The distribution of $NS_n$ approaches the standard normal distribution $\mathcal{N}(0,1)$ as $n \to \infty$.

$$
\lim_{n \to \infty} P(NS_n \leq z) = \Phi(z)
$$

where $\Phi(z)$ is the cumulative distribution function (CDF) of the standard normal distribution. The moment-generating function $m(t)$ for $\mathcal{N}(0,1)$ is:

$$
m(t) = e^{\frac{t^2}{2}}
$$

Taylor Expansion of $m(t)$:

$$
m(t) = \underbrace{m(0)}_{=1} + \underbrace{tm'(0)}_{=0} + \underbrace{\frac{t^2}{2}m''(0)}_{\frac{t^2}{2}} + \underbrace{\cdots}_{\mathcal{E}(t^3)}
$$

If $\mathbf{Y} = b\mathbf{X}$, then $m_{\mathbf{Y}} = m_{\mathbf{X}}(bt)$, so:

$$
\begin{align*}
m_{\mathbf{Z}}(t) &= m_{S_n}\left(\frac{t}{\sigma \sqrt{n}}\right) \\
&= \left[m\left(\frac{t}{\sigma \sqrt{n}}\right)\right]^n
\end{align*}
$$

where:

$$
\begin{align*}
m\left(\frac{t}{\sigma \sqrt{n}}\right) &= 1 + \frac{1}{2}\sigma^2\left(\frac{t^2}{\sigma^2 n}\right) + \mathcal{E}\left(\frac{t^3}{\sigma^3 n^{3/2}}\right) \\
&= 1 + \frac{1}{2}\left(\frac{t^2}{n}\right) + \mathcal{E}_n(\cdots)
\end{align*}
$$

Now, take the limit as $n \to \infty$ of $m_{\mathbf{Z}}(t)$:

$$
\begin{align*}
\lim_{n \to \infty} m_{\mathbf{Z}}(t) &= \lim_{n \to \infty} \left(1 + \frac{1}{2}\left(\frac{t^2}{n}\right) + \mathcal{E}_n\right) \\
&= \lim_{n \to \infty} \left(1 + \frac{t^2}{2n}\right)^n \\
&= e^{\frac{t^2}{2}}, \quad \text{since} \quad \lim_{n \to \infty} \left(1 + \frac{a}{n}\right)^n = e^a
\end{align*}
$$

This proves that the moment-generating function of $\mathcal{N}(0,1)$ is $e^{\frac{t^2}{2}}$.
