# **Theory of Expectation**

Expectation is the "center of mass" or weighted average of the probability distribution. If $\mathbf{X}$ is a discrete random variable with probability $P(x)$, then the expectation is denoted by $E[\mathbf{X}]$. To compute the expectation of $E[\mathbf{X}]$, it is defined as:

- Discrete: $E[\mathbf{X}] = \sum_{\text{all }k} x_k P(\mathbf{X} = x_k) = \mu$
- Continuous: $E[\mathbf{X}] = \int_{-\infty}^{\infty} x f(x) \, dx$

## **Properties of Expectation**

- $E(\mathbf{X} + \mathbf{Y}) = E(\mathbf{X}) + E(\mathbf{Y})$
- If $\mathbf{X}$ and $\mathbf{Y}$ are independent, then $E(\mathbf{X}\mathbf{Y}) = E(\mathbf{X}) E(\mathbf{Y})$
- The expectation of a function of a random variable $\mathbf{Y} = g(\mathbf{X})$ is:

    $$
    \begin{aligned}
    E(\mathbf{Y}) = E(g(\mathbf{X})) &= \sum_x g(x) P(x), \quad \text{if discrete random variable} \\
    E(\mathbf{Y}) = E(g(\mathbf{X})) &= \int_{-\infty}^{\infty} g(x) f(x) \, dx, \quad \text{if continuous random variable} \\
    E(g(\mathbf{X}) h(\mathbf{Y})) &= E(g(\mathbf{X})) E(h(\mathbf{Y}))
    \end{aligned}
    $$
