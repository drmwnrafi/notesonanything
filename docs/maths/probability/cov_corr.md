# **Covariance and Correlation**

Covariance and correlation describe the relationship between two random variables. Covariance is a measure of how two random variables change together. Correlation is a standardized measure of the strength and direction of the linear relationship between two variables.

$$
\begin{align*}
\text{Cov}(\mathbf{X},\mathbf{Y}) &= E\left[(\mathbf{X}-E(\mathbf{X}))(\mathbf{Y}-E(\mathbf{Y}))\right] \\
&= E\left[(\mathbf{X}-\mu_\mathbf{X})(\mathbf{Y}-\mu_\mathbf{X})\right]
\end{align*}
$$

## **Properties**

$$
\require{cancel}
\begin{align*}
\text{Cov}(\mathbf{X},\mathbf{Y}) &= E\left[(\mathbf{X}-\mu_\mathbf{X})(\mathbf{Y}-\mu_\mathbf{X})\right] \\
&= E(\mathbf{X}\mathbf{Y}) - \mu_\mathbf{X}E(\mathbf{Y}) - \cancel{\mu_\mathbf{Y}E(\mathbf{X})} + \cancel{\mu_\mathbf{X}\mu_\mathbf{Y}} \\
&= E(\mathbf{X}\mathbf{Y}) - \mu_\mathbf{X}E(\mathbf{Y}) \\
&= E(\mathbf{X}\mathbf{Y}) - E(\mathbf{X})E(\mathbf{Y})
\end{align*}
$$

When $\mathbf{X}$ and $\mathbf{Y}$ are independent, then $\text{Cov}(\mathbf{X},\mathbf{Y}) = 0$.

$$
\begin{align*}
\text{Var}(\mathbf{X}) &= \text{Cov}(\mathbf{X},\mathbf{X}) \\
\text{Var}(\mathbf{X}+\mathbf{Y}) &= \text{Var}(\mathbf{X}) + \text{Var}(\mathbf{Y}) + 2\text{Cov}(\mathbf{X},\mathbf{Y})
\end{align*}
$$

## **Correlation of $\mathbf{X}$ and $\mathbf{Y}$**

- A correlation of +1 indicates a perfect positive linear relationship.
- A correlation of -1 indicates a perfect negative linear relationship.
- A correlation of 0 implies no linear relationship.

$$
\begin{align*}
\text{Cor}(\mathbf{X},\mathbf{Y}) &= \frac{\text{Cov}(\mathbf{X},\mathbf{Y})}{\sigma(\mathbf{X})\sigma(\mathbf{Y})} \\
&= \frac{\sigma_{\mathbf{X}\mathbf{Y}}}{\sigma_{\mathbf{X}}\sigma_{\mathbf{Y}}} \\
\text{Cor}(a\mathbf{X}+b, c\mathbf{Y}+d) &= \text{Cor}(\mathbf{X}, \mathbf{Y})
\end{align*}
$$

!!! info "The Caveat"
    While independent variables always have zero covariance and zero correlation, the reverse is not true: zero covariance or zero correlation does not imply independence.
