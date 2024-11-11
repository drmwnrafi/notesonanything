# **Theory of Expectation**

Expectation is a "center of mass" or weighted average of the probability distribution. If $\mathbf{X}$ is a discrete random variable with probability $P(x)$, then the expectation donated by $E[\mathbf{X}]$. To compute expectation of $E[\mathbf{X}]$ defined by :

- Discrete : $E[\mathbf{X}] = \sum_{\text{all }k}x_kP(\mathbf{X}=x_k) = \mu$
- Continous : $E[\mathbf{X}] = \int_{-\infty}^{\infty} xf(x)\; dx$

## **Properties of Expectation**

- $E(\mathbf{X}+\mathbf{Y}) = E(\mathbf{X}) + E(\mathbf{Y})$
- If $\mathbf{X}$, $\mathbf{Y}$ are indpendent, then $E(\mathbf{X}\mathbf{Y})=E(\mathbf{X})E(\mathbf{Y})$
- Expectation of function of random variable $\mathbf{Y}=g(\mathbf{X})$, then
 
    $$
    \begin{align*}
    E(\mathbf{Y})=E(g(\mathbf{X}))&=\sum_x g(x)P(x), \text{if discrete random variable} \\
    E(\mathbf{Y})=E(g(\mathbf{X}))&=\int_{-\infty}^{\infty} g(x)f(x)\; dx, \text{if continous random variable}\\
    E(g(\mathbf{X})h(\mathbf{Y}))&=E(g(\mathbf{X}))E(h(\mathbf{Y}))
    \end{align*}
    $$