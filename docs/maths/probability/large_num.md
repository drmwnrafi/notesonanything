# **Law of Large Numbers**

Let $\mathbf{X}_1, \mathbf{X}_2, \mathbf{X}_3, \cdots, \mathbf{X}_n$ be a sequence of independent random variables with $E(\mathbf{X}) = \mu$ and $\text{Var}(\mathbf{X}) = \sigma^2$. The Law of Large Numbers states that, as $n$ becomes large, repeated experiments will result in values that are close to the expected value.

The sample mean, defined as

$$
\mathbf{X}_n = \frac{1}{n} \sum_{i=1}^n \mathbf{X}_i,
$$

will converge to $\mu$ as $n \rightarrow \infty$.

### Proof:

We want to show that $P(|\mathbf{X}_n - \mu| > \epsilon) \rightarrow 0$ as $n \rightarrow \infty$. Since the $\mathbf{X}_i$'s are independent:

$$
\begin{aligned}
E(\mathbf{X}_n) &= \frac{1}{n} \sum_{i=1}^n E(\mathbf{X}_i) = \mu, \\
\text{Var}(\mathbf{X}_n) &= \frac{1}{n^2} \sum_{i=1}^n \text{Var}(\mathbf{X}_i) = \frac{\sigma^2}{n}, \\
\text{as } n &\rightarrow \infty, \text{Var}(\mathbf{X}_n) \rightarrow 0, \\
P(|\mathbf{X}_n - \mu| > \epsilon) &\leq \frac{\text{Var}(\mathbf{X}_n)}{\epsilon^2} \\
&= \frac{\sigma^2}{n\epsilon^2} \rightarrow 0, \quad \text{as } n \rightarrow \infty.
\end{aligned}
$$

Thus, we have shown that $P(|\mathbf{X}_n - \mu| > \epsilon) \rightarrow 0$ as $n \to \infty$, completing the proof.
