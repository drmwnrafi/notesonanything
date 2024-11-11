# **Moment Generating Function**

Moment generating function $m(t)$ has same characteristic with Tylor Series, it tells the characteristic of the distribution with $n$-order. Moment generating functions uniquely determine the CDF by using :

$$
m(t) = 
\begin{cases}
\sum_x e^{tx} P(\mathbf{X}=x), &\mathbf{X} \text{ discrete}\\
\\
\underbrace{\int_{-\infty}^\infty e^{tx} f(x) \; dx}_{\text{Laplace T-form of PDF}}, &\mathbf{X} \text{ continous}
\end{cases}
$$

The moment generating functions of distribution :

$$
\begin{matrix}
n-\text{order} & \text{Moment} & \text{Expression}\\
1-\text{st} & \text{Mean} & E(\mathbf{X})=\mu\\
2-\text{nd} & \text{Variance} & E[(\mathbf{X}-\mu)^2]=\sigma^2\\
3-\text{th} & \text{Skweness} & E\left[\left(\frac{\mathbf{X}-\mu}{\sigma}\right)^3\right]\\
4-\text{th} & \text{Kurtosis} & E\left[\left(\frac{\mathbf{X}-\mu}{\sigma}\right)^4\right] = \frac{E(\mathbf{X}^4)}{\sigma}\\
\vdots & \vdots & \vdots \\
n-th & \text{"moments"} & E(\mathbf{X}^n)
\end{matrix}
$$

Properties of $m(t)$ :

Let $\mathbf{X}$ and $\mathbf{X}$ be independent random variables, and corresponding moment generating functions are $m_\mathbf{X}$ and $m_\mathbf{Y}$.

Define $\mathbf{Z} = \mathbf{X} +\mathbf{Y}$, then

$$
m_\mathbf{z}(t) = m_\mathbf{X}(t) m_\mathbf{Y}(t)
$$

Proof with aim $m_\mathbf{Z}(t) = m_\mathbf{X}(t) + \mathbf{Y}(t)$,

$$
\begin{align*}
m_\mathbf{z}(t) &= E(e^{t\mathbf{Z}})=E(E^{t(\mathbf{X}+\mathbf{y})})\\
&= E(e^{t\mathbf{X}})e^{t\mathbf{Y}})\\
&= E(e^{t\mathbf{X}})E(e^{t\mathbf{Y}})\\
&= m_\mathbf{X}(t) m_\mathbf{Y}(t)
\end{align*}
$$


Example :

$m(t)$ of Poisson distribution $\mathbf{X}\sim \text{Poisson}(\lambda)$ with discrete random variable $\mathbf{X}$,

$$
P(\mathbf{X}=x) = \frac{\lambda^xe^{-\lambda}}{x!}
$$

then, 

$$
\begin{align*}
m(t)&=\sum_x e^{tx}\frac{\lambda^xe^{-\lambda}}{x!}\\
&=e^{-\lambda} \sum_x \frac{(e^t\lambda)^x}{x!}\\
&= e^{-\lambda}e^{\lambda e^t}\\
&= e^{\lambda\left(e^t-1\right)}\\
\end{align*}
$$

Compute $E(\mathbf{X}), E(\mathbf{X}^2),\cdots ,E(\mathbf{X}^n)$ by take derivatis of $m(t)$,

$$
\begin{align*}
m(t) &= e^{\lambda\left(e^t-1\right)} \\
m'(t)&= e^{\lambda\left(e^t-1\right)} \cdot \lambda e^t\\
m''(t)&= e^{\lambda\left(e^t-1\right)} (\lambda e^t)^2 + e^{\lambda\left(e^t-1\right)} (\lambda e^t) 
\end{align*}
$$

then, to take $E(\mathbf{X}^n)$ set $t=0$ to $\frac{d^n}{d^nt}m(t)$,

$$
\begin{align*}
E(\mathbf{X}) &= m'(0) = \lambda \\
E(\mathbf{X}^2) &= m''(0) = \lambda^2 + \lambda \\
\end{align*}
$$

then we can calculate variance by using $E(\mathbf{X})$ and $E(\mathbf{X}^2)$

$$
\begin{align*}
\text{Var}(\mathbf{X}) &= E(\mathbf{X}^2) - (E(\mathbf{X}))^2\\
&= \lambda^2 + \lambda - \lambda\\
&= \lambda
\end{align*}
$$

{++Proof++}

Proof Goals : aim to show $E(\mathbf{X}^n) = \frac{d^n}{d^nt}m(t)$

Take $m(t)$ of $\mathbf{X}$ continous,

$$
\begin{align*}
m(t) &= \int_{-\infty}^\infty e^{tx} f(x) \; dx \\
\end{align*}
$$

then take the derivatives of $m(t)$

$$
\begin{align*}
m'(t) &= \int_{-\infty}^\infty x e^{tx} f(x) \; dx \\
m''(t) &= \int_{-\infty}^\infty x^2 e^{tx} f(x) \; dx \\
\vdots & \quad \quad \quad \quad \vdots \\
\frac{d^n}{d^nt}m(t) &= \int_{-\infty}^\infty x^n e^{tx} f(x) \; dx
\end{align*}
$$

Subtite $t=0$ to derivatives of $m(t)$,

$$
\begin{align*}
m'(0) &= \int_{-\infty}^\infty x f(x) \; dx = E(\mathbf{X})\\
m''(0) &= \int_{-\infty}^\infty x^2 f(x) \; dx = E(\mathbf{X}^2)\\
\vdots & \quad \quad \quad \quad \quad \vdots \\
\frac{d^n}{d^nt}m(0) &= \int_{-\infty}^\infty x^n f(x) \; dx = E(\mathbf{X}^n)
\end{align*}
$$

