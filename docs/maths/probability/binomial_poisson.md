# **Binomial to Poisson Distribution**

Given $n$ and $p$ are large number of bernouli trials and probability of success, respectively.

$$
P(\mathbf{X} = 1) = p \\
P(\mathbf{X} = 0) = q = 1-p 
$$

If $n\rightarrow \infty$, $P\rightarrow 0$ , and $n\cdot p$ or $n \cdot q$ stays constant, then the distribution becomes {==Poisson Distribution==}.

$$
\mathbf{X} \sim \text{Binomial}(n, p) \rightarrow \mathbf{X} \sim \text{Poisson}(\lambda) 
$$

where $\lambda = n\cdot p$ is parameters for Poisson Distribution

Let $\lambda = np$, $p = \frac{\lambda}{n}$, and $\mathbf{X} \sim \text{Binomial}(n, p)$

$$
\require{cancel}
\begin{align*}
P(\mathbf{X}=k) &= \binom{n}{k} p^k (1-p)^{n-k} \\
&= \frac{n!}{k!(n-k)!} \left(\frac{\lambda}{n}\right)^k \left(1-\frac{\lambda}{n}\right)^n-k\\
&= \frac{\lambda^k}{k!} \frac{n!}{(n-k)!} \left(\frac{\lambda}{n}\right)^k \left(1-\frac{\lambda}{n}\right)^n-k\\
& = \frac{\lambda^k}{k!}\frac{n(n-1)(n-2)\cdots (n-k+1)\cancel{(n-k)!}}{\cancel{(n-k)!}n^k} \left(1-\frac{\lambda}{n}\right)^n-k\\
&=  \frac{\lambda^k}{k!}\left[1\left(1-\frac{1}{n}\right) \left(1-\frac{2}{n}\right)\cdots\left(1-\frac{k+1}{n}\right)\right] \left(1-\frac{\lambda}{n}\right)^n \left(1-\frac{\lambda}{n}\right)^{-k}
\end{align*}
$$

as $n$ large number of trials, $n\rightarrow \infty$.

$$
\lim_{n\rightarrow \infty} \left(1-\frac{\lambda}{n}\right)^n = e^{-\lambda}\\
\lim_{n\rightarrow \infty} \left(1-\frac{1}{n}\right) \left(1-\frac{2}{n}\right)\cdots\left(1-\frac{k+1}{n}\right) = 1\\
\lim_{n\rightarrow \infty} \left(1-\frac{\lambda}{n}\right)^{-k}= 1\\
$$

then,

$$
P(\mathbf{X=k})=\frac{\lambda^ke^{-\lambda}}{k!}
$$
