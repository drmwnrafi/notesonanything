# **Geometric Control of Quadrotor on SE(3)**

!!! warning "This Page is under construction"

Nonlinear dynamics naturally evolve on non-flat spaces, and controlling them using flat-space representations often introduces singularities and global instability. For systems like quadcopters, whose dynamics lie on the manifold $SE(3)$, geometric control offers a framework to design controllers that intrinsically respect the manifold’s structure. 

Euler angle-based controllers exhibit singularities (gimbal lock) when representing complex rotational maneuvers. Quaternion-based controllers can avoid singularities, but they represent attitude by double-covering the configuration of special orthogonal group $SO(3)$. Geometric control, based on Lie theory, avoids complexities, discontinuities, and ambiguities [[1]](#ref1).

Although quadcopters are underactuated systems with four inputs and six degrees of freedom, geometric control achieves asymptotic tracking of a subset of outputs, such as position or attitude, while maintaining stability in other degrees of freedom [[1]](#ref1).

!!! info
    This page focuses on derivation of the error equations from [[1]](#ref1) and providing some intuition behind them. We don't explain the dynamics of quadcopters here; for that, you can refer to [Quadcopter Dynamics](../../models_sys/quadcopter/) for details on quadcopter equations of motion.

Quadcopters equations of motion in short, 

$$
\begin{gather}
\dot{x} = v \tag{1}\\
m \dot{v} = mge_3-fRe_3 \tag{2}\\
\dot{R} = R \hat{\Omega} \tag{3}\\
J\dot{\Omega} + \Omega \times J \Omega = M \tag{4}\\
\end{gather}
$$

where :

- $m \in \mathbb{R}$ is the total mass.  
- $J \in \mathbb{R}^{3 \times 3}$ is the inertia matrix with respect to the body-fixed frame.  
- $R \in SO(3)$ is the rotation matrix from the body frame to the inertial frame.  
- $\dot{x}$ is the position derivative (velocity) in the inertial frame.  
- $\dot{v}$ is the acceleration in the inertial frame.  
- $\hat{\Omega}$ is the skew-symmetric matrix of angular velocity $\Omega$  
- $f$ is the total thrust force.  
- $M$ is the moment vector applied to the body.
- $e_3$ is z-axis in inertial reference frame.

??? note "Lie Algebra $\mathfrak{g}$ $\leftrightharpoons$ Euclidean Space $\mathbb{R}$"
    - *Hat map* $(\hat{\cdot}) : \mathbb{R}^3 \rightarrow \mathfrak{so}(3)$ for mapping vector in $\mathbb{R}^3$ to lie algebra of $SO(3)$.
    - *Vee map* $({\cdot}^\vee) : \mathfrak{so}(3) \rightarrow \mathbb{R}^3$ is the inverse of *hat map*.

    Lie Theory for robotics explain more [here](../../../maths/lie_theory/preface/).

## **The Error Functions**

[Lee, et. al](#ref1) use multiple controller to achieve asymptotic output tracking of both attitude and position; attitude control, position control, and velocity control. 
Complex flight maneuver can be defined by specifying a concatenation of flight modes together with conditions for switching between them.

### **Attitude Control**

This type of control mode requires desired attitude $R_d(t) \in SO(3)$ and current attitude $R(t) \in SO(3)$ are function of time, where represented as $R_d$ and $R$ for simplified the derivation. Then, they choose the error function on $SO(3) \times SO(3)$ as follows :

$$
\Gamma(R, R_d) = \frac{1}{2}\mathrm{tr} \left[I_3-R_d^TR \right] \tag{5}
$$

$R_d^TR$ is relative attitude that transforms a vector from the current frame to the desired frame. 
If current attitude $R$ align to $R_d$ it makes $R_d^TR$ equals the identity matrix $I$, and the error function $\Gamma(R, R_d)$ becomes zero. 

??? question "How $R_d^TR=I$?"
    This follows from the fundamental axiom of groups, which states that the product of an element and its inverse is the identity element: 
    
    $$
    AA^{-1}=I
    $$
    
    For $R \in SO(3)$, where $R^{-1}=R^T$ because of rotation matrices are orthogonal. Therefore, the product of a rotation matrix and its inverse is:
    
    $$
    R^{-1}R=R^TR=I
    $$
    
    So, if $R_d=R$ it will leads to identity. 

Analyze **Eq. (5)**, 

$$
\begin{align}
\Gamma(R, R_d) &= \frac{1}{2}\left[\mathrm{tr}(I_3)-\mathrm{tr}(R_d^TR)\right]\tag{6}\\
&= \frac{1}{2}\left[ 3-1-2\cos(\theta) \right]\tag{7}\\
&= 1-\cos(\theta)\tag{8}\\
\end{align}
$$

where $\theta$ is angle of rotation. The function $\Gamma$ is locally positive definite $(\Gamma \ge 0)$ when relative angle less than $180^\circ$. 

- The $\cos(\theta) \in [−1, 1]$ for $\theta \in [0, \pi]$.
- $\Gamma(R, R_d)=0$, if $\theta=0^\circ$. 
- $\Gamma(R, R_d) > 0$, for $0^\circ < \theta < 180^\circ$.

??? question "Why exclude $\theta = 180^\circ$?"


Based on [[2] (Chapter 11)](#ref2), we can take attitude error $e_R$ by take derivative of error function $\Gamma \in SO(3) \times SO(3)$ 
with variation of rotation matrix expressed as $\partial R = R\hat{\omega}$ for $\omega \in \mathbb{R}^3$.
Then, the variational derivative when dealing with variation of rotation expressed as follows :

$$
D_R\Gamma(R, R_d) \cdot \partial R \tag{9}
$$

By using $\frac{\partial}{\partial X} \mathrm{tr}(XA) = A^T$ [[3]](#ref3)

$$
D_R \Gamma(R, R_d) = \frac{\partial \Gamma (R, R_d)}{\partial R} = -\frac{1}{2}R_d \tag{10}
$$

Then, apply [Frobenius inner product](../../../maths/) for both $D_R \Gamma$ and $\partial R$ 
, $\left< A, B \right> = \mathrm{tr}(A^TB)$

$$
\begin{align}
\left< D_R\Gamma(R, R_d), \partial R \right> &= \mathrm{tr}\left(\left( -\frac{1}{2}R_d \right)^T \partial R \right) \tag{11}\\
&= \mathrm{tr}\left(-\frac{1}{2}R_d^T \partial R \right) \tag{12}\\
&= -\frac{1}{2} \mathrm{tr}\left(R_d^T \partial R \right) \tag{13}\\
&= -\frac{1}{2} \mathrm{tr}\left(R_d^T R\hat{\omega} \right) \tag{14}\\
\end{align}
$$

Use property of hat map, $-\frac{1}{2}\mathrm{tr}\left(\hat{x}\hat{y}\right) = x^Ty = x\cdot y$, where $(\cdot)$ is a vector dot product. 
To construct the skew-symmetric, we can use $\hat{a} = \frac{1}{2}(A-A^T)$ for $A \in  G$ where $G$ is Lie groups, 

$$
\begin{align}
\left< D_R\Gamma(R, R_d), \partial R \right> &= -\frac{1}{2}\cdot \frac{1}{2} \mathrm{tr}\left(\left(R_d^T R \right)\hat{\omega} \right) \tag{15}\\
&= -\frac{1}{2} \cdot \frac{1}{2}\mathrm{tr}\left(\left(R_d^T R - \left(R_d^T R\right)^T \right)\hat{\omega} \right) \tag{16}\\
&= \frac{1}{2} \left( -\frac{1}{2} \mathrm{tr}\left(\left[R_d^T R - R^TR_d\right]\hat{\omega}\right) \right) \tag{17}\\
&= \frac{1}{2}\left(R_d^T R - R^TR_d\right)^\vee \cdot \omega \tag{18}\\
\end{align}
$$

Then, rotation error $e_R \in \mathbb{R}^3$ expressed as follows :

$$
e_R = \frac{1}{2}\left(R_d^T R - R^TR_d\right)^\vee \tag{19}
$$

??? info "Related to geodesic distance on $SO(3)$"
    Geodesic distance in the special orthogonal group $SO(3)$ [[4]](#ref4) is given by:
    
    $$
    d_{\text{geo}}(R, R_d) = ||\log(R_d^TR)||_F \tag{20}
    $$

    Where logarithm of $R \in SO(3)$ is defined as:

    $$
    \begin{equation}
    \log R = \begin{cases}
        0 & \text{if} & \theta = 0 \\
        \frac{\theta}{2 \sin (\theta)} [R - R^T] &\text{if}&\theta \neq 0
    \end{cases} \tag{21}
    \end{equation}
    $$

    with $\theta = \cos^{-1} \left( \frac{\mathrm{tr}(R) - 1}{2} \right)$.

    Starting from **Eq.(20)**, we derive the geodesic distance, where $||\cdot||_F$ is Frobenius norm.

    $$
    \log (R_d^TR) = \frac{\theta}{2 \sin (\theta)} [R_d^TR - R^TR_d] \tag{22}
    $$

    Subtitute **Eq.(22)** to **Eq.(20),** where $||A||_F = \sqrt{\left< A, A\right>} = \sqrt{\mathrm{tr}(A^TA)}$

    $$
    \begin{align}
    d_{\text{geo}}(R, R_d) &= \sqrt{\mathrm{tr}\left[\log(R_{d}^{T}R)^{T}\log(R_{d}^{T}R)\right]} \tag{32}\\
    &= \sqrt{\mathrm{tr}\left[\left(\frac{\theta}{2\sin(\theta)}\left(R_{d}^{T}R - R^{T}R_{d}\right)\right)^{T} \frac{\theta}{2\sin(\theta)}\left(R_{d}^{T}R - R^{T}R_{d}\right)\right]} \tag{33}\\
    &= \frac{\theta}{2\sin(\theta)} \sqrt{\mathrm{tr}\left[\left(\left(R_{d}^{T}R - R^{T}R_{d}\right)\right)^{T} \left(R_{d}^{T}R - R^{T}R_{d}\right)\right]} \tag{34}\\
    \end{align}
    $$

    Since the Frobenius norm is always non-negative, the minus sign $\mathrm{tr}[\hat{x}\hat{y}] = -2x \cdot y$ is irrelevant. Frobenious norm take $\sqrt{\|\cdot \|^2}$ [[5]](#ref5) for rotation matrix makes minus sign irrelevant.

    $$
    d_{\text{geo}}(R, R_d) = \frac{\theta}{\sin(\theta)}\left\|\left(R_d^TR - R^TR_d\right)^\vee \right\| \tag{35}\\
    $$

From dynamics of quadrotor we can get $\dot{R} = R\hat{\Omega}$ and $\dot{R}_d = R_d\hat{\Omega}_d$. To compare angular velocity we have to calculate in same tangent space either $\mathsf{T}_RSO(3)$ or $\mathsf{T}_{R_d}SO(3)$. 
Thus, the different of time derivative of rotation $R$ and $R$ equivalent to $R\hat{e_\Omega}$.

$$
\dot{R} - \dot{R}_d(R_d^TR) = R\hat{\Omega} -R_d\hat{\Omega}_dR_d^TR \tag{36}
$$

Then, re-arrange **Eq.(36)** to match with $R\hat{e_\Omega}$.

$$
\dot{R} - \dot{R}_d(R_d^TR) = R(\hat{\Omega} - R^TR_d\hat{\Omega}_dR_d^TR) \tag{37}
$$

Where $RR^T=I$, then we can get $\hat{e}_\Omega$

$$
\hat{e}_\Omega = \hat{\Omega} - R^TR_d\hat{\Omega}_dR_d^TR \tag{38}
$$

In vector space $e_\Omega \in \mathbb{R}^3$,

$$
e_\Omega = \Omega - R^TR_d{\Omega}_d \tag{39}
$$

??? info "Angular Velocity Error $e_\Omega$ from Lie Groups Adjoint Action"
    Since tangent $\dot{R}\in \mathsf{T}_RSO(3)$ and $\dot{R}_d \mathsf{T}_{R_d}SO(3)$ are "live" on different tangent spaces, so we need "transform" one of them. Thanks to [adjoint action](../../../maths/lie_theory/adjoint/), a distinguished operator on the Lie algebra, this transformation can be achieved.

    We have to "transform" $\dot{R}_d$ to $\mathsf{T}_RSO(3)$. The relative rotation $R_{rel}$ of it expressed as $R^TR_d$ to rotate the vector from $R_d$-frame to $R$-frame. Where $R_{rel}$ is an element of $SO(3)$, then

    $$
    \begin{align*}
    \mathsf{Adj}_{R_{rel}}\left(\hat{\Omega}_d\right)&= R_{rel}\hat{\Omega}_d\left(R_{rel}\right)^{-1} \in \mathfrak{so}(3) \\
    &= \widehat{R_{rel}\Omega} \\
    \mathsf{Adj}_{R_{rel}} &= R_{rel}
    \end{align*}
    $$

    So, we can applied directly to tangent vector, where $\hat{\Omega}, \hat{\Omega}_d \in \mathfrak{so}(3)$ and $\Omega, \Omega_d \in \mathbb{R}^3$

    $$
    \Omega_d = R^TR_d\Omega_d \in \mathsf{T}_RSO(3) 
    $$

    Thus,

    $$
    e_\Omega = \Omega - R^TR_d\Omega_d
    $$

### **Position and Velocity Control**

Error on position control $e_p$ pretty straightforward, we need desired position $p_d \in \mathbb{R}^3$, current position $p_d \in \mathbb{R}^3$, desired linear velocity $\dot{p}_d \in \mathbb{R}^3$, and current velocity $v \in \mathbb{R}^3$. 

$$
\begin{align}
e_p = p - p_d \tag{40}\\
e_v = v - \dot{p}_d \tag{41}
\end{align}
$$

## **Differential Flatness**

We applied differential flatness approach from Mellinger et. al [[7]](#ref7) for the controller. Differentially flat systems are those in which all states can be defined by using the outputs and its derivatives. Mellinger et. al [[7]](#ref7) use 4-th derivatives of positions (snap) and 2-nd derivatives of orientation (yaw) to determined all quadcopter states.

$$
\sigma = \left[ x, y, z, \psi \right]^T
$$

We donated position of CoG as $p = [x, y, z]$ and orientation w.r.t z-axis as $\psi$.


#### References

<a id="ref1">[1] [Control of Complex Maneuvers for a Quadrotor UAV using Geometric Methods on SE(3)
](https://arxiv.org/abs/1003.2005)</a>

<a id="ref2">[2] [Geometric control of mechanical systems
](https://link.springer.com/book/10.1007/978-1-4899-7276-7)</a>

<a id="ref3">[3] [Matrix Cookbook
](https://www.math.uwaterloo.ca/~hwolkowi/matrixcookbook.pdf)</a>

<a id="ref4">[4] [Axis–angle Representation
](https://en.wikipedia.org/wiki/Axis%E2%80%93angle_representation)</a>

<a id="ref5">[5] [Learning with 3D rotations, a hitchhiker’s guide to SO(3)
](https://arxiv.org/pdf/2404.11735v1)</a>

<a id="ref6">[6] [A micro Lie theory for state estimation in robotics
](https://arxiv.org/pdf/1812.01537)</a>

<a id="ref7">[6] [Minimum snap trajectory generation and control for quadrotors
](https://ieeexplore.ieee.org/document/5980409)</a>
