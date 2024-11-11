# **Variance and Standart Deviation**

Means $\mu$ is defined as center of mass of distribution, variance $\sigma^2$ is the average of square differences from mean $\mu$, and standart deviation $\sigma$ tells how spread the distribution. 

If we applied function $g(\cdot)$ to $\mathbf{X}$, with $g(\mathbf{X})=\mathbf{X}^2$, then

$$
\begin{align*}
\mathbf{Y} &= \mathbf{X}^2\\
E(\mathbf{Y}) &= \int_{-\infty}^{\infty}x^2f(x) \; dx
\end{align*}
$$

If $\mathbf{X}$ and $\mathbf{Y}$ are independent, then

$$
\text{Var}(\mathbf{X}+\mathbf{Y}) = \text{Var}(\mathbf{X})+\text{Var}(\mathbf{Y})
$$

Let random variable $\mathbf{X}$ is normal distribution,

$$
\mathbf{X}\sim \mathcal(\mu, \sigma^2)
$$

Then, the PDF of normal distribution

$$
f(x) = \frac{1}{\sqrt{2\pi}\sigma}e^{\frac{-(x-\mu)^2}{2\sigma^2}}
$$

where $\mu$ is $E(\mathbf{X})$ and $\sigma^2$ is variance of $\sigma^2$.

How to compute $\text{Var}(\mathbf{X})$

$$
\begin{align*}
\text{Var}(\mathbf{X}) &= E((\mathbf{X}-\mu)^2)\\
&= E(\mathbf{X}^2)-[E(x)]^2
\end{align*}
$$

{++Proof++}

$$
\begin{align*}
\text{Var}(\mathbf{X}) &= E((\mathbf{X}-\mu)^2)\\
&= E(\mathbf{X}^2-2\mu\mathbf{X}+\mu^2)\\
&= E(\mathbf{X}^2)-E(2\mu\mathbf{X})+E(\mu^2)\\
&= E(\mathbf{X}^2)-E(2\mu^2)+E(\mu^2)\\
&= E(\mathbf{X}^2-\mu^2)\\
\end{align*}
$$

Example :

Given $T\sim \text{Exp}(\lambda)$ where the PDF $f(t) = \lambda e^{\lambda t}$,

Means : 

$$
\begin{align*}
E(T)&= \int_0^\infty tf(t) \; dt\\
&= \int_0^\infty t\;\lambda e^{-\lambda t} \; dt\\
&=\Biggr[-te^{-\lambda t}\Biggr]_0^\infty + \int_0^\infty e^{-\lambda t}\\
&= 0 -\frac{1}{\lambda} e^{-\lambda t}\Biggr]_0^\infty\\
&= \frac{1}{\lambda}\\
\end{align*}
$$

Variance :

$$
\begin{align*}
\text{Var}(T)&= E(T^2)-[E(T)]^2\\
\end{align*}
$$

where $E(T^2)$ :

$$
\begin{align*}
E(T^2) &= \int_0^{\infty} t^2 \lambda e^{-\lambda t} \; dt\\
&= \frac{2}{\lambda^2}
\end{align*}
$$

then, 

$$
\begin{align*}
\text{Var}(T)&= E(T^2)-[E(T)]^2\\
&= \frac{2}{\lambda^2} -\frac{1}{\lambda^2}\\
&= \frac{1}{\lambda^2}
\end{align*}
$$

Standart Deviation :

$$
\sigma(T) = \sqrt{\frac{1}{\lambda^2}} = \frac{1}{\lambda} 
$$

Median :

$$
\begin{align*}
F(t)&=\int_0^tf(t) \;dt\\
&=1-e^{-\lambda t} = \frac{1}{2}
\end{align*}
$$

then, we can get $e^{-\lambda t} =\frac{1}{2}$ and $t=\frac{ln(2)}{\lambda}$, so

$$
\text{Median}(T) = \frac{ln(2)}{\lambda}
$$