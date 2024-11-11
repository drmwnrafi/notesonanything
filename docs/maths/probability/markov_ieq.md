# **Markov's Inequality**

Markov's inequality is used to check if $\mathbf{X}$ is a non-negative random variable. If $\mathbf{X} \geq 0 \), then:

$$
P(\mathbf{X} \geq a) \leq \frac{E(\mathbf{X})}{a}
$$

Let $\mathbf{X}$ be any positive discrete random variable, so we can write:

$$
\begin{aligned}
E(\mathbf{X}) &= \sum_x x P(\mathbf{X} = x) \\
&\geq \sum_{x \geq a} x P(\mathbf{X} = x) \\
&\geq \sum_{x \geq a} a P(\mathbf{X} = x) \\
&= a P(\mathbf{X} \geq a)
\end{aligned}
$$

Thus, Markov's inequality gives us the result:

$$
P(\mathbf{X} \geq a) \leq \frac{E(\mathbf{X})}{a}
$$
