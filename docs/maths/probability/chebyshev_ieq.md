# **Chebyshev's Inequality**

While Markov's Inequality only deals with means, Chebyshev's Inequality deals with both the mean and variance.

Let $\mathbf{X}$, and define $\mathbf{Y} = (\mathbf{X} - E(\mathbf{X}))^2$. Since $\mathbf{Y}$ is a non-negative random variable, we apply Markov's inequality to $\mathbf{Y}$. For any $a > 0$, we have:

$$
\begin{aligned}
E(\mathbf{Y}) &= (\mathbf{X} - E(\mathbf{X}))^2 = \text{Var}(\mathbf{X}) \\
P(|\mathbf{X} - E(\mathbf{X})| \geq a) &= P((\mathbf{X} - E(\mathbf{X}))^2 \geq a^2) \quad \text{let} \ a^2 = b \\
&= P(\mathbf{Y} \geq b) \leq \frac{E(\mathbf{Y})}{b} \\
P(|\mathbf{X} - E(\mathbf{X})| \geq a) &\leq \frac{\text{Var}(\mathbf{X})}{a^2}
\end{aligned}
$$

Chebyshev's Inequality bounds the probability that $\mathbf{X}$ deviates from the mean by more than $a$ standard deviations.
