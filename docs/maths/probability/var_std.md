# **Variance and Standard Deviation**

The mean $\mu$ is defined as the center of mass of the distribution, the variance $\sigma^2$ is the average of squared differences from the mean $\mu$, and the standard deviation $\sigma$ tells us how spread out the distribution is.

If we apply a function $g(\cdot)$ to $\mathbf{X}$, with $g(\mathbf{X}) = \mathbf{X}^2$, then:

$$
\begin{aligned}
\mathbf{Y} &= \mathbf{X}^2 \\
E(\mathbf{Y}) &= \int_{-\infty}^{\infty} x^2 f(x) \, dx
\end{aligned}
$$

If $\mathbf{X}$ and $\mathbf{Y}$ are independent, then:

$$
\text{Var}(\mathbf{X} + \mathbf{Y}) = \text{Var}(\mathbf{X}) + \text{Var}(\mathbf{Y})
$$

Let the random variable $\mathbf{X}$ follow a normal distribution:

$$
\mathbf{X} \sim \mathcal{N}(\mu, \sigma^2)
$$

Then, the PDF of the normal distribution is:

$$
f(x) = \frac{1}{\sqrt{2\pi} \sigma} e^{-\frac{(x - \mu)^2}{2 \sigma^2}}
$$

where $\mu$ is $E(\mathbf{X})$ and $\sigma^2$ is the variance of $\mathbf{X}$.

To compute $\text{Var}(\mathbf{X})$, we have:

$$
\begin{aligned}
\text{Var}(\mathbf{X}) &= E((\mathbf{X} - \mu)^2) \\
&= E(\mathbf{X}^2) - [E(\mathbf{X})]^2
\end{aligned}
$$

**Proof:**

$$
\begin{aligned}
\text{Var}(\mathbf{X}) &= E((\mathbf{X} - \mu)^2) \\
&= E(\mathbf{X}^2 - 2\mu \mathbf{X} + \mu^2) \\
&= E(\mathbf{X}^2) - E(2\mu \mathbf{X}) + E(\mu^2) \\
&= E(\mathbf{X}^2) - 2\mu E(\mathbf{X}) + \mu^2 \\
&= E(\mathbf{X}^2) - \mu^2
\end{aligned}
$$

### Example:

Given $T \sim \text{Exp}(\lambda)$, where the PDF is $f(t) = \lambda e^{-\lambda t}$, the mean is:

$$
\begin{aligned}
E(T) &= \int_0^\infty t f(t) \, dt \\
&= \int_0^\infty t \lambda e^{-\lambda t} \, dt \\
&= \left[ -t e^{-\lambda t} \right]_0^\infty + \int_0^\infty e^{-\lambda t} \, dt \\
&= 0 - \frac{1}{\lambda} \left[ e^{-\lambda t} \right]_0^\infty \\
&= \frac{1}{\lambda}
\end{aligned}
$$

Variance:

$$
\begin{aligned}
\text{Var}(T) &= E(T^2) - [E(T)]^2
\end{aligned}
$$

To compute $E(T^2)$, we have:

$$
\begin{aligned}
E(T^2) &= \int_0^\infty t^2 \lambda e^{-\lambda t} \, dt \\
&= \frac{2}{\lambda^2}
\end{aligned}
$$

Then:

$$
\begin{aligned}
\text{Var}(T) &= \frac{2}{\lambda^2} - \frac{1}{\lambda^2} \\
&= \frac{1}{\lambda^2}
\end{aligned}
$$

Standard Deviation:

$$
\sigma(T) = \sqrt{\frac{1}{\lambda^2}} = \frac{1}{\lambda}
$$

Median:

$$
\begin{aligned}
F(t) &= \int_0^t f(t) \, dt \\
&= 1 - e^{-\lambda t} = \frac{1}{2}
\end{aligned}
$$

Then, we solve $e^{-\lambda t} = \frac{1}{2}$, which gives:

$$
t = \frac{\ln(2)}{\lambda}
$$

Thus, the median is:

$$
\text{Median}(T) = \frac{\ln(2)}{\lambda}
$$
