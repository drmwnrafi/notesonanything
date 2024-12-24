# **Pose Graph SLAM**

Given error function $e_{ij}(x)$, which represent the error from node-$i$ to node-$j$,

$$
\begin{align}
e_{ij}(x) = z_{ij}-\hat{z}(x_i, x_j)
\end{align}
$$

Where $z$ is the value from the measurement model (such as [PLICP](../plicp/) or [ICP](../icp/)), $\hat{z}$ is the predicted value from the math model, and $x$ represents the states. Assume the uncertainty on the sensor follows a Gaussian distribution.

$$
\begin{align}
p(x)=\frac{1}{\sqrt{2\pi^n|\Sigma|}}\exp\left(-\frac{1}{2}(x-\mu)^T\Omega(x-\mu)\right)\sim \mathcal{N}(\mu, \Sigma)
\end{align}
$$

$\Omega = \Sigma^{-1}$ is the information matrix, so the error follows a multivariate normal distribution with mean $\mu = \hat{z}$ and variance $\Sigma$. By applying the log-likelihood to $p(x)$, the optimal state $x^*$ can be obtained through Maximum Likelihood Estimation. The optimal $x$ occurs when $\text{ln}(p(x))$ reaches its maximum value.

$$
\begin{align}
x^*=\text{argmax }p(x) = \underset{x}{\text{argmin }}e_{ij}^T\Omega_{ij}e_{ij}
\end{align}
$$

Due to the cumulative error in the trajectory, which is not just an error at a single pose but rather a possibility that the error occurs at multiple poses, the error function is the sum of the errors at each pose.

$$
\begin{align}
F(x) = e_{ij}^T\Omega_ie_{ij}\\
x^*=\underset{x}{\text{argmin }} \sum_{i,j}F(x_{ij})
\end{align}
$$

The equation does not have a definitive solution due to the presence of a rotation matrix, which indicates a nonlinear equation. The problem can be solved using optimization and linearized using Taylor expansion.

$$
\begin{align}
F_{ij}(x+\Delta x)=e_{ij}(x+\Delta x)^T\Omega_{ij} e_{ij}(x+\Delta x)^T
\end{align}
$$

By using the **First-Order Taylor Expansion**,

$$
\begin{align}
e_{ij}(x+\Delta x) = e_{ij}+J_{ij}\Delta x
\end{align}
$$

Substitute into $F_{ij}(x + \Delta x)$,

$$
\begin{align}
F_{ij}(x+\Delta x) &= (e_{ij}+J_{ij}\Delta x)^T \Omega_{ij}(e_{ij}+J_{ij}\Delta x)\\
&= (e_{ij}^T+(J_{ij}\Delta x)^T)+(\Omega_{ij}e_{ij}+\Omega_{ij}J_{ij}\Delta x)(\\
&= (e_{ij}^T+\Delta x^TJ_{ij}^T)(\Omega_{ij}e_{ij}+\Omega_{ij}J_{ij}\Delta x)\\
&= e_{ij}^T\Omega_{ij}e_{ij}+e_{ij}^T\Omega_{ij}J_{ij}\Delta x + \Delta{x}^TJ_{ij}^T\Omega_{ij}e_{ij}+
\Delta{x}^TJ_{ij}^T\Omega_{ij}J_{ij}\Delta x\\
&= e_{ij}^T\Omega_{ij}e_{ij}+e_{ij}^T\Omega_{ij}J_{ij}\Delta x + \left(\Delta{x}^TJ_{ij}^T\Omega_{ij}e_{ij}\right)^T+
\Delta{x}^TJ_{ij}^T\Omega_{ij}J_{ij}\Delta x\\
&= e_{ij}^T\Omega_{ij}e_{ij}+e_{ij}^T\Omega_{ij}J_{ij}\Delta x +
e_{ij}^T\Omega_{ij}^TJ_{ij}\Delta x+
\Delta{x}^TJ_{ij}^T\Omega_{ij}J_{ij}\Delta x\\
\end{align}
$$

Since $\Omega_{ij}$ is a symmetric matrix, it follows that $\Omega_{ij} = \Omega_{ij}^T$.

$$
\begin{align}
F_{ij}(x+\Delta x) = 
\underbrace{e_{ij}^T\Omega_{ij}e_{ij}}_{c}+
2\underbrace{e_{ij}^T\Omega_{ij}J_{ij}}_{b}\Delta x +
\Delta{x}^T\underbrace{J_{ij}^T\Omega_{ij}J_{ij}}_{H}\Delta x
\end{align}
$$

The first derivative of $F_{ij}(x + \Delta x)$ can be obtained,

$$
\begin{align}
\frac{\partial{F_{ij}(x+\Delta x)}}{\partial\Delta x} &\approx 2b+2H\Delta x = 0\\
\Delta x &= -H^{-1}b\\
x &\leftarrow x+\Delta x
\end{align}
$$

$\hat{z}_{ij}$ is calculated using *inverse pose composition*,

$$
\begin{align}
T_i^{-1} &=
\begin{bmatrix}
R_i^T & -R_i^Tt_i\\
0 & 1
\end{bmatrix}\\
T_j &=
\begin{bmatrix}
R_j & t_j\\
0 & 1
\end{bmatrix}\\
\hat{z}_{ij} &= T_i^{-1}\cdot T_j\\
\end{align}
$$

Where $T \in SE(n)$ and $R \in SO(n)$.

$$
\begin{align}
\hat{z}_{ij}= 
\begin{bmatrix}
R_i^TR_j & R_i^T(t_j-t_i)\\
0 & 1
\end{bmatrix}
\triangleq 
\begin{bmatrix}
R_i^T(t_j-t_i)\\
\theta_j - \theta_i
\end{bmatrix} = 
\begin{bmatrix}
\Delta t\\
\Delta \theta
\end{bmatrix}
\end{align}
$$

$\triangleq$ represented in vector space. 
The value of $e_{ij}$ is obtained in the same way as $\hat{z}_{ij}$. Therefore, $e_{ij} = z_{ij}^{-1} T_i^{-1} T_j$.

$$
\begin{align}
e_{ij} \triangleq \begin{bmatrix}
R_{ij}^T(R_i^T(t_j-t_i)-t_{ij})\\
\theta_j - \theta_i - \theta_{ij} 
\end{bmatrix}
\end{align}
$$

Determine the partial derivatives of $e_{ij}$ with respect to $x_i$ and $x_j$,

$$
\begin{align}
x_i &= \begin{bmatrix}
t_i \\
\theta_i
\end{bmatrix} \\
x_j &= \begin{bmatrix}
t_j \\
\theta_j
\end{bmatrix} \\
\frac{\partial e_{ij}}{\partial x_i} &= 
\begin{bmatrix}
\frac{\partial \Delta t}{\partial t_i} & \frac{\partial \Delta t}{\partial \theta_i} \\
\frac{\partial \Delta \theta}{\partial t_i} & \frac{\partial \Delta \theta}{\partial \theta_i}
\end{bmatrix} \\
\frac{\partial e_{ij}}{\partial x_j} &= 
\begin{bmatrix}
\frac{\partial \Delta t}{\partial t_j} & \frac{\partial \Delta t}{\partial \theta_j} \\
\frac{\partial \Delta \theta}{\partial t_j} & \frac{\partial \Delta \theta}{\partial \theta_j}
\end{bmatrix} \\
\frac{\partial e_{ij}}{\partial x_i} &= 
\begin{bmatrix}
-R_{ij}^T R_i^T & R_{ij}^T \frac{\partial R_i^T}{\partial \theta_i}(t_j - t_i) \\
0 & -1
\end{bmatrix} \\
\frac{\partial e_{ij}}{\partial x_j} &= 
\begin{bmatrix}
R_{ij}^T R_i^T & 0 \\
0 & 1
\end{bmatrix}
\end{align}
$$

The Jacobian matrix $J_{ij}$ contains only the value $\frac{\partial e_{ij}}{\partial x_i}$ in the $i$-th row, $\frac{\partial e_{ij}}{\partial x_j}$ in the $j$-th row, and the other elements are 0.

$$
\begin{align}
J_{ij} &=
\begin{bmatrix}
0&0&0&...&\frac{\partial e_{ij}}{\partial x_i}&...&\frac{\partial e_{ij}}{\partial x_j}&...&0&0&0
\end{bmatrix}\\
\nabla_{ij} &=J_{ij}^T\Omega_{ij}e_{ij}\\
H_{ij} &=J_{ij}^T\Omega_{ij}J_{ij}\\
\nabla &=\sum_{i,j}J_{ij}^T\Omega_{ij}e_{ij}\\
H &=\sum_{i,j}J_{ij}^T\Omega_{ij}J_{ij}
\end{align}
$$

After $\nabla$ and $H$ are obtained, the Non-Linear Least Squares problem can be solved by iterating the optimization process using Gauss-Newton or Levenberg-Marquardt until $\Delta F(x) < \text{tolerance}$.

$$
\begin{align}
\text{(GN) } \Delta x &= -H^{-1}\nabla\\
\text{(LM) } \Delta x &= -(H+\lambda \text{ diag}(H))^{-1}\nabla\\
x&\leftarrow x+\Delta x
\end{align}
$$

### **References**

- [[SLAM]Errors and Jacobian Derivations for SLAM Part 1](https://alida.tistory.com/69#5.-relative-pose-error-pgo)

- [A Technical Walkthrough of the SLAM Back-end](https://www.youtube.com/watch?v=FhwFyA0NQkE&t=4639s)

- [Graph-based SLAM using Pose Graphs (Cyrill Stachniss)](https://www.youtube.com/watch?v=uHbRKvD8TWg&t=2682s)