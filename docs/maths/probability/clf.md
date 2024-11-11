# **Central Limit Theorem**

As large $n$ experiment with $\mathbf{X}_1, \mathbf{X}_2, \mathbf{X}_3, \cdots \mathbf{X}_n$ as I.I.D random variables will leads to close to expected value, it will reaches Central Limit Theorem, doesn't what the distribution of $\mathbf{X}_i$'s is.
With the sample means, 

$$
\bar{\mathbf{X}}_n = \frac{1}{n}\sum_{i=0}^n \mathbf{X}_i \sim \mathcal{N}(\mu, \frac{\sigma^2}{n})
$$

{++Proof++}

The distribution of normlized sum :

$$
\begin{align*}
S_n &= \sum_{i=1}^n \mathbf{X}_i \\
NS_n &= \frac{S_n}{\sigma\sqrt{n}}
\end{align*}
$$

Proof Goals : the distribution of $NS_n$ approaches the standard normal distribution $\mathcal{N}(0,1)$ as $n\rightarrow \infty$.

$$
\lim_{n\rightarrow \infty} P(NS_n \leq z) = \Phi(z)
$$

where $\Phi(z)$ is the cumulative distribution function (CDF) of standart normal distribution. Then, moment generating function $m(t)$ for $\mathcal{N}(0,1)$ is:

$$
m(t) =e^{\frac{t^2}{2}}
$$

Taylor expand of $m(t)$,

$$
m(t) = \underbrace{m(0)}_{=1} + \underbrace{tm'(0)}_{=0} + \underbrace{\frac{t^2}{2}m''(0)}_{\frac{t^2\sigma^2}{2}} + \underbrace{\cdots}_{\mathcal{E}(t^3)}
$$

If $\mathbf{Y} = b\mathbf{X}$ then $m_\mathbf{Y} = m_\mathbf{X}(bt)$,

$$
\begin{align*}
m_\mathbf{Z}(t) &= m_{S_n}\left(\frac{t}{\sigma\sqrt{n}}\right)\\
&= \left[m\left(\frac{t}{\sigma\sqrt{n}}\right)\right]^n
\end{align*}
$$

where,

$$
\begin{align*}
m\left(\frac{t}{\sigma\sqrt{n}}\right) &= 1+\frac{1}{2}\sigma^2\left(\frac{t^2}{\sigma^2n}\right) + \mathcal{E}\left(\frac{t^3}{\sigma^3n^{\frac{3}{2}}}\right)\\
&= 1+\frac{1}{2}\left(\frac{t^2}{n}\right) + \mathcal{E}_n(\cdots)
\end{align*}
$$

Then, take limit $n\rightarrow \infty$ of $m_\mathbf{Z}(t)$,

$$
\begin{align*}
\lim_{n\rightarrow \infty} m_\mathbf{Z}(t) &= \lim_{n\rightarrow \infty} \left[+\frac{1}{2}\left(\frac{t^2}{n}\right) + \mathcal{E}_n\right]\\
&=  \lim_{n\rightarrow \infty} \left(1+\frac{t^2}{2n}\right)^2\\
&= e^{\frac{t^2}{2}}, \text{with }\lim_{n\rightarrow \infty} \left(1+\frac{a}{n}\right)^2=e^2
\end{align*}
$$

It is proven where $m(t)$ of $\mathcal{N}(0,1)$ is $e^{\frac{t^2}{2}}$.
