# **Function of Random Variables**

Random variabels $\mathbf{X}$ with function for random variables $g(\cdot)$ and output new random variables $\mathbf{Y} = g(\mathbf{X})$. PDF of a distribution can not applied directly to $g(\cdot)$, but have to apply with CDF than result in CDF too.

Given random variable $\mathbf{X}$ with normal distribution $\mathbf{X} \sim \mathcal{N}(\mu, \sigma^2)$, and function $g(\mathbf{X}) = a\mathbf{X}+b$. So,

$$
\mathbf{Y} = a\mathbf{X}+b \\
$$

Then, the CDF $F_\mathbf{Y}$:

$$
\begin{align*}
F_\mathbf{Y}(y) = P(\mathbf{Y} < y) &= P(a\mathbf{X}+b)\\
&= P(\mathbf{X} < \frac{y-b}{a})\\
&= F_\mathbf{X}(\frac{y-b}{a})
\end{align*}
$$

Then, take the derivatives to convert CDF to PDF :

$$
\begin{align*}
f_\mathbf{y}(y) &= \frac{d}{dy} F_\mathbf{X}\left(\frac{y-b}{a}\right)\\
&= \frac{1}{a} f_\mathbf{X}\left(\frac{y-b}{a}\right)
\end{align*}
$$ 

So,

$$
\mathbf{Y} \sim \mathcal{N}(a\mu+b, a^2\sigma^2)
$$

Example :

Given random variable $\mathbf{X}$ with standart normal distribution $\mathbf{X} \sim \mathcal{N}(0, 1)$, and function $g(\mathbf{X}) = \mathbf{X}^2$. So, the CDF of $\mathbf{Y}$ :

$$
\begin{align*}
F_\mathbf{Y}(y)=P(\mathbf{Y} < y)&=P(\mathbf{X}^2 < y)\\
&= P(-\sqrt{y} < \mathbf{x} < \sqrt{y})\\
&= \Phi(\sqrt{y}) - \Phi(-\sqrt{y}) \\
\end{align*}
$$

Then, the PDF of $\mathbf{Y}$

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

