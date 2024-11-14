# **Approximation Hessian $H=J^TJ$**

The loss function $E$ is a nonlinear, differentiable, and smooth function,   allowing us to use {==Taylor expansion==} to approximate the nonlinear function 
$E$.

Taylor Series:

$$
F(x+\Delta x)=F(x)+\frac{1}{1!}F'(x)\Delta x+\frac{1}{2!}F''(x)\Delta x^2+\cdots
$$

Thus,

$$
y(x)=f(x; s)\\
r_i = y_i - f(x_i; s)\\
$$

where $f(x; s)$ is a nonlinear function, $r_i$ is the residual, $y_i$ is the target, and $s$ is the state vector containing $( t_x, t_y, \theta)$  which we aim to optimize. We apply a first-order Taylor expansion to compute changes in the state.

$$
f(x_i; s') = \underbrace{f(x_i; s)}_{y_i-r_i} + \frac{\partial f}{\partial s}(s'-s)=y_i
$$

To satisfy $f(x_1; s') = y_i$ we have $\frac{\partial f}{\partial s}(s'-s) = r_i$. Substituting $s$, we get :

$$
\frac{\partial f}{\partial t_x}\Delta t_x+\frac{\partial f}{\partial t_y}\Delta t_y+\frac{\partial f}{\partial \theta}\Delta \theta =r_1\\
\frac{\partial f}{\partial t_x}\Delta t_x+\frac{\partial f}{\partial t_y}\Delta t_y+\frac{\partial f}{\partial \theta}\Delta \theta =r_2\\
\;\;\;\;\;\;\vdots\;\;\;\;\;+\;\;\;\;\;\;\vdots \;\;\;\;\;\;\;+ \;\;\;\;\;\vdots\;\;\;=\;\vdots\\
\frac{\partial f}{\partial t_x}\Delta t_x+\frac{\partial f}{\partial t_y}\Delta t_y+\frac{\partial f}{\partial \theta}\Delta \theta =r_n
$$

This can be simplified as follows:

$$
\begin{matrix}
x=x_1\\
\vdots\\
x=x_n
\end{matrix}
\begin{bmatrix}
\frac{\partial f}{\partial t_x} & \frac{\partial f}{\partial t_y}& \frac{\partial f}{\partial \theta}\\
\vdots&\vdots&\vdots\\
\frac{\partial f}{\partial t_x} & \frac{\partial f}{\partial t_y}& \frac{\partial f}{\partial \theta}\\
\end{bmatrix}
\begin{bmatrix}
\Delta t_x\\
\Delta t_y\\
\Delta \theta
\end{bmatrix} =
\begin{bmatrix}
r_1\\
\vdots\\
r_n\\
\end{bmatrix}
$$

Thus,

$$
J \Delta s=r
$$

Since $J$ is not a square matrix, it is **non-invertible**. To solve this equation, we multiply both sides by $J^T$, yeilding :

$$
(J^TJ)\Delta s=J^Tr\\
\Delta s = (J^TJ)^{-1}J^Tr
$$

### **References** 

- [METHODS FOR NON-LINEAR LEAST SQUARES PROBLEMS](https://www.researchgate.net/publication/261652064_Methods_for_Non-Linear_Least_Squares_Problems_2nd_ed)
- [Nonlinear Least Squares](https://www.youtube.com/watch?v=8evmj2L-iCY)