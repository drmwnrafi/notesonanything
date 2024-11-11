# **Laws of Large Number**

Let sequances of independent random variables $\mathbf{X}_1, \mathbf{X}_2, \mathbf{X}_3, \cdots \mathbf{X}_n$ with $E(\mathbf{X}) = \mu$ and $\text{Var}(\mathbf{X})=\sigma^2$.
Laws of large number states that large $n$ repeat experiment will results close to the expected value. 

Then, the sample mean can describe by $\mathbf{X}=\frac{1}{n}\sum_{i=1}^n\mathbf{X}_i$ will converge to $\mu$ when $n\rightarrow\infty$.

{++Proof++}

$P(|\mathbf{X}_n-\mu| > \epsilon)\rightarrow 0$ as $n \rightarrow \infty$. Since $\mathbf{X}$ independent,

$$
\begin{align*}
E(\mathbf{X}_n) &= \frac{1}{n}\sum_{i=1}^n E(\mathbf{X}_i) = \mu, \\
\text{Var}(\mathbf{X}_n) &= \frac{1}{n^2}\sum_{i=1}^n \text{Var}(\mathbf{X}_i) = \frac{\sigma^2}{n}, \\ 
\text{as } n &\rightarrow \infty, \text{Var}(\mathbf{X}_n) \rightarrow 0, \\
P(|\mathbf{X}_n - \mu| > \epsilon) &\leq \frac{\text{Var}(\mathbf{X}_n)}{\epsilon^2} \\
&= \frac{\sigma^2}{n\epsilon^2} \rightarrow 0, \quad \text{as } n \rightarrow \infty.
\end{align*}
$$
