# **Point to Line Iterative Closest Point**

Iterative Closest Point aims to align two point  clouds  P and Q  by finding a rigid-body transformation $(R, t)$ applied to point  cloud P and iteratively minimizes the error metric. Point to Point distance of vanilla ICP [[1]](#ref1) uses Euclidean distance between cor-respondences points. Point to Line Iterative Closest Point (PLICP) is a variant of ICP with a difference in error metric. PLICP computes error metric by projecting distances of correspondence points to the normal vector of the target point cloud [[2]](#ref2). PLICP can be solved by an optimization process such as Gauss-Newton and Levenberg-Marquardt methods [[3](#ref3), [4](#ref4)]. 

## **Error Metric**

The error metric of PLICP is expressed by: 
<a id="eq1"></a>

$$
\begin{align}
E(\mathbf{x}) = \sum_i\left[ (R(\mathbf{x}_\theta)p_i + t(\mathbf{x}_x, \mathbf{x}_y)-q_p)\cdot n_{q,i}\right]^2, \text{for } R \in SO(2), t \in \mathbb{R}^2
\end{align}
$$

where :

- State $\mathbf{x}(x, y, \theta)$ : Coordinates and rotation angle
- Rotation matrix ($R$) : Rotation matrix obtained from correspondences
- Translation vector ($\mathbf{t}$) : Translation vector obtained from correspondences
- Source point cloud ($p$) : Source point cloud
- Target point cloud ($q$) : Target point cloud
- Normals vector of target point cloud ($n_q$) : Normals vector of the target point cloud

Left terms in the bracket of [Eq. (1)](#eq1) describe the distance of correspondence point  in  the transformed source point to target  point. The right terms describes projecting the distance to normals of target point.

## **Solution by Optimization**

We can minimize $E(\mathbf{x})$ by using optimization process. Minimizing $E(\mathbf{x})$ needs to compute Gradient and Hessian matrix of $E(\mathbf{x})$ at any parameters [[4]](#ref4). Given current initial guess $\breve{\mathbf{x}}$ and perturbation of $\mathbf{x}$ donated by $\Delta\mathbf{x}$.

Let $e_{i}(\mathbf{x})=\left(\boldsymbol{R}(\mathbf{x}_{\theta})p_{i}+t( \mathbf{x}_{x},\mathbf{x}_{y})-q_{i}\right)\cdot n_{q,i}$,
<a id="eq2"></a>

$$
\begin{align}
E(\mathbf{x})=\sum_{i}\underbrace{e_{i}(\mathbf{x})^{T}e_{i}( \mathbf{x})}_{E_{i}}
\end{align}
$$

Thus, optimal parameters $\mathbf{x}^{*}$ expressed by :

$$
\begin{align}
\mathbf{x}^{*}=\underset{\mathbf{x}}{\operatorname{argmin}}E(\mathbf{x})
\end{align}
$$

[Eq. (2)](#eq2) can be approximated by using first-order Taylor expansion current initial guess $\breve{\mathbf{x}}$ [[5](#ref5), [6](#ref6)].
<a id="eq45"></a>

$$
\begin{align}
e_{i}(\breve{\mathbf{x}}+\Delta\mathbf{x}) &= e_{i}(\breve{\mathbf{x}}) + \nabla e_{i}(\breve{\mathbf{x}})\cdot \Delta\mathbf{x} \\
 &\simeq e_{i}(\breve{\mathbf{x}}) + \mathbf{J}_{i}\Delta\mathbf{x} 
\end{align}
$$

Substitute [Eq. (5)](#eq45) to $e_i$ in [Eq. (2)](#eq2), 
<a id="eq67"></a>

$$
\begin{align}
E_i(\breve{\mathbf{x}} + \Delta\mathbf{x}) &\simeq (e_i(\breve{\mathbf{x}}) + \mathbf{J}_i \Delta\mathbf{x}^T)(e_i(\breve{\mathbf{x}}) + \mathbf{J}_i \Delta\mathbf{x})\\
&= \underbrace{e_i^T e_i}_{\mathbf{c}_i} + 2 \underbrace{e_i^T \mathbf{J}_j}_{\mathbf{b}_i} \Delta\mathbf{x} + \Delta\mathbf{x}^T \underbrace{\mathbf{J}_i^T \mathbf{J}_j}_{\mathbf{H}_i} \Delta\mathbf{x}
\end{align}
$$

Rewrite error metric in [Eq. (2)](#eq2) with [Eq. (7)](#eq67) by using the same method in [[13]]().

$$
\begin{align}
\mathbf{c} &= \sum_i \mathbf{c}_i\\
\mathbf{b} &= \sum_i \mathbf{b}_i\\
\mathbf{H} &= \sum_i \mathbf{H}_i\\
E(\breve{\mathbf{x}} + \Delta\mathbf{x}) &= \mathbf{c} + 2\mathbf{b}\Delta\mathbf{x} + \Delta\mathbf{x}^T\mathbf{H}
\end{align}
$$

Thus, we can obtain $\Delta\mathbf{x}^*$ by solving linear system 
<a id="eq1213"></a>

$$
\begin{align}
\Delta\mathbf{x}^* &= -\mathbf{H}^{-1}\mathbf{b}\\
&=-(\mathbf{J}^T\mathbf{J})^{-1}\mathbf{b}
\end{align}
$$

Optimal rigid-body transformation $\mathbf{x}^*$ can be obtained by adding an initial guess $\breve{\mathbf{x}}$ to computed perturbation $\Delta\mathbf{x}^*$.

$$
\begin{align}
\mathbf{x}^* = \breve{\mathbf{x}} + \Delta\mathbf{x}^*
\end{align}
$$

In [Eq. (13)](#eq1213), $\mathbf{J}$ is a Jacobian of $e_i(\mathbf{x})$ and $\mathbf{I}$ is an identity matrix. Jacobian of $e_i(\mathbf{x})$ expressed by taking partial derivatives of $e_i(\mathbf{x})$ w.r.t parameters $\mathbf{x}$:

$$
\begin{align}
\mathbf{J} &= \left[ \frac{\partial e}{\partial x} \quad \frac{\partial e}{\partial y} \quad \frac{\partial e}{\partial\theta} \right]\\
\mathbf{J} &= \left[ n_{q,x} \quad n_{q,y} \quad n_{q,x}\left(-q_x \sin \theta - q_y \cos \theta + n_{q,y}\left(q_x \cos \theta - q_y \sin \theta\right)\right) \right]
\end{align}
$$

### References 

<a id="ref1">[1][ Method for registration of 3-D shapes
](https://graphics.stanford.edu/courses/cs164-09-spring/Handouts/paper_icp.pdf)</a>

<a id="ref2">[2] [Object modeling by registration of multiple range images
](https://graphics.stanford.edu/courses/cs348a-17-winter/Handouts/chen-medioni-align-rob91.pdf)</a>

<a id="ref3">[3] [Numerical  Recipes  in C
](https://www.grad.hr/nastava/gs/prg/NumericalRecipesinC.pdf)</a>

<a id="ref4">[4] [ICP
](https://github.com/niosus/notebooks/blob/master/icp.ipynb)</a>

<a id="ref5">[5] [Robust registration of 2D and 3D point sets
](https://www.robots.ox.ac.uk/~vgg/publications/2001/Fitzgibbon01c/fitzgibbon01c.pdf)</a>

<a id="ref6">[6] [SLAM using ICP and graph optimization  considering  physical properties  of environment
](https://arxiv.org/pdf/2007.00483)</a>