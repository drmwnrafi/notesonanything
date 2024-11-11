# **Moment Generating Function**

The moment generating function (MGF), denoted as $m(t)$, provides insight into the characteristics of a distribution and has a similar form to a Taylor series. It captures the distribution's properties up to the $n$-order. The MGF uniquely determines the cumulative distribution function (CDF) of a random variable $\mathbf{X}$. For a random variable $\mathbf{X}$, the MGF is given by:

$$
m(t) = 
\begin{cases}
\sum_x e^{tx} P(\mathbf{X}=x), &\mathbf{X} \text{ discrete}\\
\\
\underbrace{\int_{-\infty}^\infty e^{tx} f(x) \; dx}_{\text{Laplace T-form of PDF}}, &\mathbf{X} \text{ continous}
\end{cases}
$$

## **Moments of a Distribution**
The $n$-th moment of a distribution can be derived from the derivatives of the MGF evaluated at $t=0$:

$$
\begin{matrix}
n-\text{order} & \text{Moment} & \text{Expression}\\
1-\text{st} & \text{Mean} & E(\mathbf{X})=\mu\\
2-\text{nd} & \text{Variance} & E[(\mathbf{X}-\mu)^2]=\sigma^2\\
3-\text{th} & \text{Skweness} & E\left[\left(\frac{\mathbf{X}-\mu}{\sigma}\right)^3\right]\\
4-\text{th} & \text{Kurtosis} & E\left[\left(\frac{\mathbf{X}-\mu}{\sigma}\right)^4\right] = \frac{E(\mathbf{X}^4)}{\sigma}\\
\vdots & \vdots & \vdots \\
n-th & \text{"n-th moments"} & E(\mathbf{X}^n)
\end{matrix}
$$

## **Properties of the Moment Generating Function**

Let $\mathbf{X}$ and $\mathbf{X}$ be independent random variables, and corresponding moment generating functions $m_\mathbf{X}$ and $m_\mathbf{Y}$.

Define $\mathbf{Z} = \mathbf{X} +\mathbf{Y}$, then

$$
m_\mathbf{z}(t) = m_\mathbf{X}(t) m_\mathbf{Y}(t)
$$

Proof with aim $m_\mathbf{Z}(t) = m_\mathbf{X}(t) + \mathbf{Y}(t)$,

$$
\begin{align*}
m_\mathbf{z}(t) &= E(e^{t\mathbf{Z}})=E(E^{t(\mathbf{X}+\mathbf{y})})\\
&= E(e^{t\mathbf{X}})e^{t\mathbf{Y}}\\
&= E(e^{t\mathbf{X}})E(e^{t\mathbf{Y}})\\
&= m_\mathbf{X}(t) m_\mathbf{Y}(t)
\end{align*}
$$


Example :

The probability mass function (PMF) of a random variable $\mathbf{X}\sim \text{Poisson}(\lambda)$ with discrete random variable $\mathbf{X}$,

$$
P(\mathbf{X}=x) = \frac{\lambda^xe^{-\lambda}}{x!}
$$

then, the $m(t)$ is :

$$
\begin{align*}
m(t)&=\sum_x e^{tx}\frac{\lambda^xe^{-\lambda}}{x!}\\
&=e^{-\lambda} \sum_x \frac{(e^t\lambda)^x}{x!}\\
&= e^{-\lambda}e^{\lambda e^t}\\
&= e^{\lambda\left(e^t-1\right)}\\
\end{align*}
$$

Compute $E(\mathbf{X}), E(\mathbf{X}^2),\cdots ,E(\mathbf{X}^n)$ by take differentiating $m(t)$,

$$
\begin{align*}
m(t) &= e^{\lambda\left(e^t-1\right)} \\
m'(t)&= e^{\lambda\left(e^t-1\right)} \cdot \lambda e^t\\
m''(t)&= e^{\lambda\left(e^t-1\right)} (\lambda e^t)^2 + e^{\lambda\left(e^t-1\right)} (\lambda e^t) 
\end{align*}
$$

To calculate $E(\mathbf{X}^n)$, set $t=0$ in the $n$-th derivative of $m(t)$,

$$
\begin{align*}
E(\mathbf{X}) &= m'(0) = \lambda \\
E(\mathbf{X}^2) &= m''(0) = \lambda^2 + \lambda \\
\end{align*}
$$

The variance is given by:

$$
\begin{align*}
\text{Var}(\mathbf{X}) &= E(\mathbf{X}^2) - (E(\mathbf{X}))^2\\
&= \lambda^2 + \lambda - \lambda\\
&= \lambda
\end{align*}
$$

{++Proof++}

Proof Goals : aim to show $E(\mathbf{X}^n) = \frac{d^n}{d^nt}m(t)$

For continuous random variables, the moment generating function is:

$$
\begin{align*}
m(t) &= \int_{-\infty}^\infty e^{tx} f(x) \; dx \\
\end{align*}
$$

The derivatives of of $m(t)$ are :

$$
\begin{align*}
m'(t) &= \int_{-\infty}^\infty x e^{tx} f(x) \; dx \\
m''(t) &= \int_{-\infty}^\infty x^2 e^{tx} f(x) \; dx \\
\vdots & \quad \quad \quad \quad \vdots \\
\frac{d^n}{d^nt}m(t) &= \int_{-\infty}^\infty x^n e^{tx} f(x) \; dx
\end{align*}
$$

Substituting $t=0$ into these derivatives, we get the moments:

$$
\begin{align*}
m'(0) &= \int_{-\infty}^\infty x f(x) \; dx = E(\mathbf{X})\\
m''(0) &= \int_{-\infty}^\infty x^2 f(x) \; dx = E(\mathbf{X}^2)\\
\vdots & \quad \quad \quad \quad \quad \vdots \\
\frac{d^n}{d^nt}m(0) &= \int_{-\infty}^\infty x^n f(x) \; dx = E(\mathbf{X}^n)
\end{align*}
$$

This concludes the proof for the moments of a continuous random variable.