# **Exponential Distribution**

Exponential distribution is a dsitribution of interval time $t$ between events. The exponential distribution is commonly used to model lifetimes. Given random variable $\mathbf{T}$ being the time.

$$
\mathbf{T} \sim \text{Exp}(\lambda)
$$

where $\lambda$ is rate of events.

$$
f(x) = \lambda e^{-\lambda x}
$$

then, to compute probability by using CDF of exponential distribution,

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

This property means that the probability of an event occurring in future do not depend on any past information. But, in the real world scenario, the probability in the future depends on past information, e.g. the lifetimes of bulbs if lifetimes approach approx avg. liftimes it is not "forgets" the past time counting.

Thus, tackles that problems by using {==Hazard Rate==}. Hazard or failure rate is defined for non repairable populations with instaneous time.

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

