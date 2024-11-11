# **Exponential Distribution**

The exponential distribution describes the interval time $t$ between events. It is commonly used to model lifetimes. Given the random variable $\mathbf{T}$ as the time, we have:

$$
\mathbf{T} \sim \text{Exp}(\lambda)
$$

where $\lambda$ is rate of events.

$$
f(x) = \lambda e^{-\lambda x}
$$

To compute the probability using the CDF of the exponential distribution, we get:

$$
\begin{align*}
F(a) = P(\mathbf{T}\leq a)&=\int_0^a f(x) \; dx \\
&= -e^{-\lambda x} \Big|_0^a\\
&= e^{-0\lambda} - e^{-a\lambda} \\
&= 1-e^{-a\lambda} \\
F(a, b) = P(a \leq \mathbf{T}\leq b)&=\int_a^b f(x) \; dx \\
&= -e^{-\lambda x} \Big|_a^b\\
&= e^{-a\lambda} - e^{-b\lambda} 
\end{align*}
$$

One of the key properties of the exponential distribution is its memoryless property, which states that for $\mathbf{T} \sim \text{Exp}(\lambda)$ :

$$
\begin{align*}
P(\mathbf{T}>t+s | \mathbf{T}>s) &= \frac{P(\mathbf{T}>s+t \cap \mathbf{T}>t)}{P(\mathbf{T}>s)}\\
&=\frac{P(\mathbf{T}>s+t)}{P(\mathbf{T}>s)}\\
&=\frac{e^{-\lambda s}e^{-\lambda t}}{e^{-\lambda s}}\\
&=e^{-\lambda t}\\
&=P(\mathbf{T}>t)\\
\end{align*}
$$

This property means that the probability of an event occurring in the future does not depend on any past information. However, in real-world scenarios, the probability in the future may depend on past information. For example, the lifetimes of bulbs may not "forget" past times, and as the lifetimes approach the average, the past information still has an influence.

To address this, we use the {==Hazard Rate==}. The hazard or failure rate is defined for non-repairable populations with instantaneous time.

$$
\begin{align*}
P(\mathbf{T} \leq t+dt | \mathbf{T}>t) &= 1-P(\mathbf{T} > t+dt | \mathbf{T}>t) \\
&= 1-P(\mathbf{T}>dt) \\
&= 1-e^{-\lambda dt} \\
&= 1-\left[1-\lambda dt + \frac{1}{2}\lambda^2dt^2-\cdots\right]\\
&\approx \lambda \; dt \\
P(\mathbf{T} \leq t+dt) &= \lambda P(\mathbf{T}>t)\; dt
\end{align*}
$$

