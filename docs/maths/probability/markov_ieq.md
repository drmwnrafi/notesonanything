# **Markov's Inequality**

Markov's inequality used to check $\mathbf{X}$ is non-negative random variable.
If $\mathbf{X} \ge 0$ then  

$$
P(\mathbf{X}\ge a) \leq \frac{E(P(\mathbf{X}\ge a))}{a}
$$

Let $\mathbf{X}$ be any positive discrete random variable, so we can write

$$
\begin{align*}
E(\mathbf{X}) =\sum_x xP(\mathbf{X}=x) &\ge \sum_{x\ge a} xP(\mathbf{X}=x)\\
&\ge \sum_{x\ge a} aP(\mathbf{X}=x)\\
&= aP(\mathbf{X}\ge a)
\end{align*}
$$