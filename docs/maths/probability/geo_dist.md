# **Geometric Distribution**

Geometric distribution tells how likely first event success given $n$ trials sequance of independent Bernoulli event with probability $p$. Then, the formula for Probability Mass FUnction (PMF) of random variabel $\mathbf{X}$.

$$
\mathbf{X} \sim \text{Geometric}(p)
$$

Because its calculate the probability of first success at $n^th$ trial, so the previous trails before $n^th$ suppose to be failure.Then, by independence, the probability of this event is,

$$
\begin{align*}
P(\mathbf{X} = n) &= P(X_0 = 0 \cap X_1 = 0 \cap \cdots \cap X_{n-1} = 0 \cap X_n = 1)\\
&= P(X_0 = 0) \cdot P(X_1 = 0) \cdots P(X_{n-1} = 0) \cdot P(X_n = 1)\\
P(\mathbf{X} = n)&=(1-p)^{n-1}p
\end{align*}
$$