# **Chebyshev Inequality**

In Markov's Inequality only deals with means, {==Chebyshev Inequality==} deals with mean and variance.

Let $\mathbf{X}$, define $\mathbf{Y}=(\mathbf{X}-E(\mathbf{X}))^2$, then $\mathbf{Y}$ is a non-negative random variable, then apply Markov's inequality to $\mathbf{Y}$. For any $a > 0$, then

$$
\begin{align*}
E(\mathbf{Y}) &= (\mathbf{X}-E(\mathbf{X}))^2 = \text{Var}(\mathbf{X}) \\
P(|\mathbf{X}-E(\mathbf{X})|\ge a) &= P((\mathbf{X}-E(\mathbf{X}))^2 \ge a^2)\;\text{let }a^2=b\\
&= P(\mathbf{Y} \ge b)\leq \frac{E(\mathbf{Y})}{b}\\
P(|\mathbf{X}-E(\mathbf{X})|\ge a) &\leq \frac{\text{Var}(\mathbf{X})}{a^2}
\end{align*}
$$

Chebyshev's Inequality bounds the probability that $\mathbf{X}$ deviates from the mean by more than $a$ standard deviations.
