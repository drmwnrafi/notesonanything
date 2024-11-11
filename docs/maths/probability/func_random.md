# **Function of Random Variables**

A random variabel $\mathbf{X}$ with a function $g(\cdot)$ applied to it results in a new random variable $\mathbf{Y} = g(\mathbf{X})$. The probability density function (PDF) of the distribution cannot be applied directly to $g(\cdot)$, instead, it must be derived using the cumulative distribution function (CDF), which results in a new CDF.

Given random variable $\mathbf{X}$ with normal distribution $\mathbf{X} \sim \mathcal{N}(\mu, \sigma^2)$, and a function $g(\mathbf{X}) = a\mathbf{X}+b$. We have:

$$
\mathbf{Y} = a\mathbf{X}+b \\
$$

The CDF of $\mathbf{Y}$, donated by $F_\mathbf{Y}$:

$$
\begin{align*}
F_\mathbf{Y}(y) = P(\mathbf{Y} < y) &= P(a\mathbf{X}+b)\\
&= P(\mathbf{X} < \frac{y-b}{a})\\
&= F_\mathbf{X}(\frac{y-b}{a})
\end{align*}
$$

To obtain the PDF, take the derivative of the CDF:

$$
\begin{align*}
f_\mathbf{y}(y) &= \frac{d}{dy} F_\mathbf{X}\left(\frac{y-b}{a}\right)\\
&= \frac{1}{a} f_\mathbf{X}\left(\frac{y-b}{a}\right)
\end{align*}
$$ 

Thus,

$$
\mathbf{Y} \sim \mathcal{N}(a\mu+b, a^2\sigma^2)
$$

Example :

Given a random variable $\mathbf{X}$ with standart normal distribution $\mathbf{X} \sim \mathcal{N}(0, 1)$, and the function $g(\mathbf{X}) = \mathbf{X}^2$. The CDF of $\mathbf{Y}$ is:

$$
\begin{align*}
F_\mathbf{Y}(y)=P(\mathbf{Y} < y)&=P(\mathbf{X}^2 < y)\\
&= P(-\sqrt{y} < \mathbf{x} < \sqrt{y})\\
&= \Phi(\sqrt{y}) - \Phi(-\sqrt{y}) \\
\end{align*}
$$

Then, the PDF of $\mathbf{Y}$ is:

$$
\begin{align*}
f_\mathbf{Y}(y)&=\frac{d}{dy} F_\mathbf{Y}(y)\\
&= \Phi'(\sqrt{y})\cdot \frac{y^{-\frac{1}{2}}}{2} - \Phi'(-\sqrt{y})\cdot \frac{y^{-\frac{1}{2}}}{2}\\
&= y^{-\frac{1}{2}} \Phi'(\sqrt{y})\\
&= y^{-\frac{1}{2}} f_\mathbf{X}(\sqrt{y}) 
\end{align*}
$$

where PDF of $f_\mathbf{X}$ is :

$$
f_\mathbf{X}(x)=\frac{e^{-\frac{x^2}{2}}}{\sqrt{2\pi}}
$$

Subtitute $f_\mathbf{X}$ to $f_\mathbf{Y}$,

$$
f_\mathbf{Y}(y) = \frac{y^{-\frac{1}{2}}e^{-\frac{x^2}{2}}}{\sqrt{2\pi}}
$$

