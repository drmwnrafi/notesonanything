# **Geometric Control of Quadrotor on SE(3)**

!!! warning "This page is under construction"

Nonlinear dynamics naturally evolve on non-flat spaces, and controlling them using flat-space representations often introduces singularities and global instability. For systems like quadcopters, whose dynamics lie on the manifold $SE(3)$, geometric control offers a framework to design controllers that intrinsically respect the manifold’s structure. 

Euler angle-based controllers exhibit singularities (gimbal lock) when representing complex rotational maneuvers. Quaternion-based controllers can avoid singularities, but they represent attitude by double-covering the configuration of special orthogonal group $SO(3)$. Geometric control, based on Lie theory, avoids complexities, discontinuities, and ambiguities [[1]](#ref1).

Although quadcopters are underactuated systems with four inputs and six degrees of freedom, geometric control achieves asymptotic tracking of a subset of outputs, such as position or attitude, while maintaining stability in other degrees of freedom [[1]](#ref1).

!!! info
    This page focuses on derivation of the error equations from [[1]](#ref1) and providing some intuition behind them. We don't explain the dynamics of quadcopters here; for that, you can refer to [Quadcopter Dynamics](../../models_sys/quadcopter/) for details on quadcopter equations of motion.

Quadcopters equations of motion in short, 
<a id="eq1234"></a>

$$
\begin{gather}
\dot{x} = v \\
\dot{v} = gz_W-\frac{f}{m}Rz_B \\
\dot{R} = R \hat{\Omega} \\
J\dot{\Omega} + \Omega \times J \Omega = M \\
\end{gather}
$$

where :

- $m \in \mathbb{R}$ is the total mass  
- $J \in \mathbb{R}^{3 \times 3}$ is the inertia matrix with respect to the body-fixed frame  
- $R \in SO(3)$ is the rotation matrix from the body frame to the inertial frame
- $\dot{x}$ is the position derivative (velocity) in the inertial frame
- $\dot{v}$ is the acceleration in the inertial frame
- $\hat{\Omega}$ is the skew-symmetric matrix of angular velocity $\Omega$  
- $f$ is the total thrust force
- $M$ is the moment vector applied to the body-fixed frame
- $z_W$ is z-axis in inertial reference frame
- $z_B$ is z-axis in body frame

??? note "Lie Algebra $\mathfrak{g}$ $\leftrightharpoons$ Euclidean Space $\mathbb{R}$"
    - *Hat map* $(\hat{\cdot}) : \mathbb{R}^3 \rightarrow \mathfrak{so}(3)$ for mapping vector in $\mathbb{R}^3$ to lie algebra of $SO(3)$.
    - *Vee map* $({\cdot}^\vee) : \mathfrak{so}(3) \rightarrow \mathbb{R}^3$ is the inverse of *hat map*.

    Lie Theory for robotics explain more [here](../../../maths/lie_theory/preface/).

## **The Error Functions**

[Lee, et. al](#ref1) use multiple controller to achieve asymptotic output tracking of both attitude and position; attitude control, position control, and velocity control. 
Complex flight maneuver can be defined by specifying a concatenation of flight modes together with conditions for switching between them.

### **Attitude Error**

This type of control mode requires desired attitude $R_d(t) \in SO(3)$ and current attitude $R(t) \in SO(3)$ are function of time, where represented as $R_d$ and $R$ for simplified the derivation. Then, they choose the error function on $SO(3) \times SO(3)$ as follows :

$$
\begin{align}
\Gamma(R, R_d) = \frac{1}{2}\mathrm{tr} \left[I_3-R_d^TR \right] 
\end{align}
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
\Gamma(R, R_d) &= \frac{1}{2}\left[\mathrm{tr}(I_3)-\mathrm{tr}(R_d^TR)\right]\\
&= \frac{1}{2}\left[ 3-1-2\cos(\theta) \right]\\
&= 1-\cos(\theta)\\
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
\begin{align}
D_R\Gamma(R, R_d) \cdot \partial R 
\end{align}
$$

By using $\frac{\partial}{\partial X} \mathrm{tr}(XA) = A^T$ [[3]](#ref3)

$$
\begin{align}
D_R \Gamma(R, R_d) = \frac{\partial \Gamma (R, R_d)}{\partial R} = -\frac{1}{2}R_d 
\end{align}
$$

Then, apply [Frobenius inner product](../../../maths/) for both $D_R \Gamma$ and $\partial R$ 
, $\left< A, B \right> = \mathrm{tr}(A^TB)$

$$
\begin{align}
\left< D_R\Gamma(R, R_d), \partial R \right> &= \mathrm{tr}\left(\left( -\frac{1}{2}R_d \right)^T \partial R \right) \\
&= \mathrm{tr}\left(-\frac{1}{2}R_d^T \partial R \right) \\
&= -\frac{1}{2} \mathrm{tr}\left(R_d^T \partial R \right) \\
&= -\frac{1}{2} \mathrm{tr}\left(R_d^T R\hat{\omega} \right) \\
\end{align}
$$

Use property of hat map, $-\frac{1}{2}\mathrm{tr}\left(\hat{x}\hat{y}\right) = x^Ty = x\cdot y$, where $(\cdot)$ is a vector dot product. 
To construct the skew-symmetric, we can use $\hat{a} = \frac{1}{2}(A-A^T)$ for $A \in  G$ where $G$ is Lie groups, 

$$
\begin{align}
\left< D_R\Gamma(R, R_d), \partial R \right> &= -\frac{1}{2}\cdot \frac{1}{2} \mathrm{tr}\left(\left(R_d^T R \right)\hat{\omega} \right) \\
&= -\frac{1}{2} \cdot \frac{1}{2}\mathrm{tr}\left(\left(R_d^T R - \left(R_d^T R\right)^T \right)\hat{\omega} \right) \\
&= \frac{1}{2} \left( -\frac{1}{2} \mathrm{tr}\left(\left[R_d^T R - R^TR_d\right]\hat{\omega}\right) \right) \\
&= \frac{1}{2}\left(R_d^T R - R^TR_d\right)^\vee \cdot \omega \\
\end{align}
$$

Then, rotation error $e_R \in \mathbb{R}^3$ expressed as follows :

$$
\begin{align}
e_R = \frac{1}{2}\left(R_d^T R - R^TR_d\right)^\vee 
\end{align}
$$

??? info "Related to geodesic distance on $SO(3)$"
    Geodesic distance in the special orthogonal group $SO(3)$ [[4]](#ref4) is given by:
    
    $$
    \begin{align}
    d_{\text{geo}}(R, R_d) = ||\log(R_d^TR)||_F 
    \end{align}
    $$

    Where logarithm of $R \in SO(3)$ is defined as:

    $$
    \begin{equation}
    \log R = \begin{cases}
        0 & \text{if} & \theta = 0 \\
        \frac{\theta}{2 \sin (\theta)} [R - R^T] &\text{if}&\theta \neq 0
    \end{cases} 
    \end{equation}
    $$

    with $\theta = \cos^{-1} \left( \frac{\mathrm{tr}(R) - 1}{2} \right)$.

    Starting from **Eq.(20)**, we derive the geodesic distance, where $||\cdot||_F$ is Frobenius norm.

    $$
    \begin{align}
    \log (R_d^TR) = \frac{\theta}{2 \sin (\theta)} [R_d^TR - R^TR_d] 
    \end{align}
    $$

    Subtitute **Eq.(22)** to **Eq.(20),** where $||A||_F = \sqrt{\left< A, A\right>} = \sqrt{\mathrm{tr}(A^TA)}$

    $$
    \begin{align}
    d_{\text{geo}}(R, R_d) &= \sqrt{\mathrm{tr}\left[\log(R_{d}^{T}R)^{T}\log(R_{d}^{T}R)\right]} \\
    &= \sqrt{\mathrm{tr}\left[\left(\frac{\theta}{2\sin(\theta)}\left(R_{d}^{T}R - R^{T}R_{d}\right)\right)^{T} \frac{\theta}{2\sin(\theta)}\left(R_{d}^{T}R - R^{T}R_{d}\right)\right]} \\
    &= \frac{\theta}{2\sin(\theta)} \sqrt{\mathrm{tr}\left[\left(\left(R_{d}^{T}R - R^{T}R_{d}\right)\right)^{T} \left(R_{d}^{T}R - R^{T}R_{d}\right)\right]} \\
    \end{align}
    $$

    Since the Frobenius norm is always non-negative, the minus sign $\mathrm{tr}[\hat{x}\hat{y}] = -2x \cdot y$ is irrelevant. Frobenious norm take $\sqrt{\|\cdot \|^2}$ [[5]](#ref5) for rotation matrix makes minus sign irrelevant.

    $$
    \begin{align}
    d_{\text{geo}}(R, R_d) = \frac{\theta}{\sin(\theta)}\left\|\left(R_d^TR - R^TR_d\right)^\vee \right\| \\
    \end{align}
    $$

From dynamics of quadrotor we can get $\dot{R} = R\hat{\Omega}$ and $\dot{R}_d = R_d\hat{\Omega}_d$. To compare angular velocity we have to calculate in same tangent space either $\mathsf{T}_RSO(3)$ or $\mathsf{T}_{R_d}SO(3)$. 
Thus, the different of time derivative of rotation $R$ and $R$ equivalent to $R\hat{e}_\Omega$.

$$
\begin{align}
\dot{R} - \dot{R}_d(R_d^TR) &= R\hat{\Omega} -R_d\hat{\Omega}_dR_d^TR \\
&= R\hat{\Omega} -(RR^T)R_d\hat{\Omega}_dR_d^TR \\
&= R\hat{\Omega} - R(R^TR_d\hat{\Omega}_dR_d^TR)^\hat \\
\end{align}
$$

Where $RR^T=I$. Then, re-arrange **Eq. (29)** to match with $R\hat{e}_\Omega$.

$$
\begin{align}
\dot{R} - \dot{R}_d(R_d^TR) = R(\hat{\Omega} - R^TR_d\hat{\Omega}_dR_d^TR) 
\end{align}
$$

Thus, we can get $\hat{e}_\Omega$

$$
\begin{align}
\hat{e}_\Omega = \hat{\Omega} - R^TR_d\hat{\Omega}_dR_d^TR 
\end{align}
$$

In vector space $e_\Omega \in \mathbb{R}^3$,
<a id="eq39"></a>

$$
\begin{align}
e_\Omega = \Omega - R^TR_d{\Omega}_d 
\end{align}
$$

??? info "Angular Velocity Error $e_\Omega$ from Lie Groups Adjoint Action"
    Since tangent $\dot{R}\in \mathsf{T}_RSO(3)$ and $\dot{R}_d \mathsf{T}_{R_d}SO(3)$ are "live" on different tangent spaces, so we need "transform" one of them. Thanks to [adjoint action](../../../maths/lie_theory/adjoint/), a distinguished operator on the Lie algebra, this transformation can be achieved.

    We have to "transform" $\dot{R}_d$ to $\mathsf{T}_RSO(3)$. The relative rotation $R_{rel}$ of it expressed as $R^TR_d$ to rotate the vector from $R_d$-frame to $R$-frame. Where $R_{rel}$ is an element of $SO(3)$, then

    $$
    \begin{align}
    \mathsf{Adj}_{R_{rel}}\left(\hat{\Omega}_d\right)&= R_{rel}\hat{\Omega}_d\left(R_{rel}\right)^{-1} \in \mathfrak{so}(3) \\
    &= \widehat{R_{rel}\Omega} \\
    \mathsf{Adj}_{R_{rel}} &= R_{rel}
    \end{align}
    $$

    So, we can applied directly to tangent vector, where $\hat{\Omega}, \hat{\Omega}_d \in \mathfrak{so}(3)$ and $\Omega, \Omega_d \in \mathbb{R}^3$

    $$
    \begin{align}
    \Omega_d = R^TR_d\Omega_d \in \mathsf{T}_RSO(3) 
    \end{align}
    $$

    Thus,

    $$
    \begin{align}
    e_\Omega = \Omega - R^TR_d\Omega_d
    \end{align}
    $$

### **Position and Velocity Errors**

Error on position control $e_p$ pretty straightforward, we need desired position $p_d \in \mathbb{R}^3$, current position $p_d \in \mathbb{R}^3$, 
desired linear velocity $\dot{p}_d \in \mathbb{R}^3$, and current velocity $v \in \mathbb{R}^3$. 

$$
\begin{align}
e_p = p - p_d \\
e_v = v - \dot{p}_d 
\end{align}
$$

## **Control Law**

Three different types of control modes derive different control law. 
We derived thrust $f$ and total moment $M$ control law by using Eq. [(2)](#eq1234) and Eq. [(4)](#eq1234), respectively. 
To recall those,

$$
\begin{align*}
\dot{v} = gz_W-\frac{f}{m}Rz_B \tag{2}\\
J\dot{\Omega} + \Omega \times J \Omega = M \tag{4}\\
\end{align*}
$$


### **Attitude Control**

In this mode, it is possible to neglected translation equation of motion [Eq. (2)](#eq1234).
Then, we need to define gain $k_R \in \mathbb{R}^{3}_+$ for rotation and $k_\Omega \in \mathbb{R}^3_+$ for angular velocity.
Chosen $f$ and $M$ input control : 
<a id="eq38"></a>

$$
\begin{align}
M = -k_Re_R -k_\Omega e_\Omega + \Omega \times J\Omega - J\left( \hat{\Omega}R^TR_d\Omega_d-R^TR_d\dot{\Omega}_d\right) 
\end{align}
$$

Subtitute [Eq. (38)](#eq38) to [Eq. (4)](#eq1234),
<a id="eq3940"></a>

$$
\require{cancel}
\begin{align}
J\dot{\Omega} + \cancel{\Omega \times J \Omega} &= -k_Re_R -k_\Omega e_\Omega + \cancel{\Omega \times J \Omega} - J\left( \hat{\Omega}R^TR_d\Omega_d-R^TR_d\dot{\Omega}_d\right) \\
J\dot{\Omega} &= -k_Re_R -k_\Omega e_\Omega - J\left( \hat{\Omega}R^TR_d\Omega_d-R^TR_d\dot{\Omega}_d\right) 
\end{align}
$$

Take derivative of time of angular velocity error [$\dot{e}_\Omega$](#eq39) to analyze moments for quadcopter $M$.
<a id="eq4144"></a>

$$
\begin{align}
\dot{e}_\Omega &= \frac{d}{dt} \Omega-R^TR_d\Omega_d \\
&= \dot{\Omega}-\dot{R}^TR_d\Omega_d - R^T\dot{R}_d\Omega_d - RR_d^T\dot{\Omega}_d  \\ 
&= \dot{\Omega}-(R\hat{\Omega})^TR_d\Omega_d - R^T(R_d\hat{\Omega}_d)\Omega_d - RR_d^T\dot{\Omega}_d \\ 
&= \dot{\Omega}+\hat{\Omega}R^TR_d\Omega_d - RR_d^T\dot{\Omega}_d \\ 
\end{align}
$$

Note that $\hat{\Omega}^T=-\hat{\Omega}$, $\dot{R}=R\hat{\Omega}$ and $\hat{x}x = 0$. Subtitute [Eq. (40)](#eq3940) to [Eq. (44)](#eq4144). It leads to,

$$
\begin{align}
J\dot{e}_\Omega &= J\dot{\Omega}+J \left(\hat{\Omega}R^TR_d\Omega_d - RR_d^T\dot{\Omega}_d\right)   \\
J\dot{e}_\Omega &= -k_Re_R -k_\Omega e_\Omega - J\left( \hat{\Omega}R^TR_d\Omega_d-R^TR_d\dot{\Omega}_d\right) + J(\hat{\Omega}R^TR_d\Omega_d - R^TR_d\dot{\Omega}_d)   \\
J\dot{e}_\Omega &= -k_Re_R -k_\Omega e_\Omega \\
\end{align}
$$

To find error attitude dynamics of $\Gamma, e_R, e_\Omega$. We take time derivative of error configuration $\Gamma$.

$$
\Gamma(R, R_d) = \frac{1}{2}\mathrm{tr} \left[I_3-R_d^TR \right] \\
$$

Terms $R_d^TR$ is the function in time. 
<a id="eq5053"></a>

$$
\begin{align}
\frac{d}{dt}(R_d^TR) &= \dot{R}^T_dR + R_d^T\dot{R} \\
&= \left(R_d\hat{\Omega}_d\right)^TR + R_d^TR\hat{\Omega} \\
&= \hat{\Omega}_d^TR_d^T R + R_d^TR\hat{\Omega} \\
&= -\hat{\Omega}_dR_d^T R + R_d^TR\hat{\Omega}
\end{align}
$$

Where $\dot{R} = R\hat{\Omega}$, $\dot{R}_d = R_d\hat{\Omega}_d$, and $-\hat{A}=\hat{A}^T$. Simplified [Eq. (53)](#eq5053) as follows :
<a id="eq5455"></a>


$$
\begin{align}
\frac{d}{dt}(R_d^TR) &= R_d^TR\left(\hat{\Omega}-R^TR_d\hat{\Omega}_dR_d^T R\right)\\
&= (R_d^TR)\hat{e}_\Omega
\end{align}
$$

Then, take the time derivative of $\Gamma$,
<a id="eq5657"></a>

$$
\begin{align}
\dot{\Gamma}(R, R_d) &= -\frac{1}{2}\left( \frac{d}{dt}(R_d^TR) \right)\\
&= -\frac{1}{2}\left((R_d^TR)\hat{e}_\Omega\right)\\
\end{align}
$$

Applied hat map property $\mathrm{tr}\left[ A\hat{x}\right] = -x^T\left(A-A^T\right)^\vee$ to [Eq. 57](#eq5657). Thus, error attitude dynamics of $\Gamma(R_d, R)$ donated by :

$$
\begin{align}
\dot{\Gamma}(R, R_d) &= \frac{1}{2}{e}_\Omega^T\left(R_d^TR-R^TR_d \right)^\vee\\
&=e_R \cdot {e}_\Omega\\
\end{align}
$$

Then, we need to find error attitude dynamics of $e_R$. Given rotation error $e_R$, 

$$
e_R = \frac{1}{2}\left(R_d^T R - R^TR_d\right)^\vee 
$$

The derivative of $e_R$ as follows :
<a id="eq60"></a>

$$
\begin{align}
\dot{e}_R &= \frac{1}{2}\left(\frac{d}{dt}\left(R_d^T R\right) - \frac{d}{dt}\left(R^TR_d\right)\right)^\vee \\
\end{align}
$$

Subtitute [Eq. (55)](#eq5455) to [Eq. (60)](#eq60). Note that $RR_d^T = \left(\frac{d}{dt}\left(R_d^TR\right)\right)^T$.
<a id="eq6162"></a>

$$
\begin{align}
\dot{e}_R &= \frac{1}{2}\left(\frac{d}{dt}\left(R_d^T R\right) - \left(\frac{d}{dt}\left(R_d^TR\right)\right)^T\right)^\vee \\
&= \frac{1}{2}\left(R_d^TR\hat{e}_\Omega + \hat{e}_\Omega R^TR_d\right)^\vee
\end{align}
$$

Applied $\hat{x}A + A^T \hat{x} = ([\mathrm{tr}[A]I-A]x)^\wedge$ to [Eq. [62]](#eq6162), and the hat map $(\cdot)^\wedge$ dissapear due to $(\hat{A})^\vee = A$ where $A \in \mathbb{R}^n$.

$$
\begin{align}
\dot{e}_R &= \frac{1}{2}\left(\mathrm{tr}\left[R^TR_d\right]I-R^TR_d\right)e_\Omega \\
&\equiv C(R^TR)e_\Omega
\end{align}
$$

Assume $R^TR_d = \exp{\hat{x}} \in SO(3)$ by using Rodrigues formuka for $x \in \mathbb{R}^3$. Then, $\|C(R^TR)\|_2 \leq 1$ it occurs from eigenvalues of $\left(C(\exp{\hat{x}})\right)^T C(\exp{\hat{x}})$. It eigenvalues is $\cos^2(\|x\|), \frac{1}{2}(1+\cos(\|x\|))$, and $\frac{1}{2}(1+\cos(\|x\|))$ [[1]](#ref1), which the three of them are less or equal to 1.


??? question "How are the eigenvalues like that?"
    I’m not entirely sure how they compute the eigenvalues of $\left(C(\exp{\hat{x}})\right)^T C(\exp{\hat{x}})$. So, it still a open question for me.

### **Position Control**
### **Velocity Control**

#### References

<a id="ref1">[1] [Control of Complex Maneuvers for a Quadrotor UAV using Geometric Methods on SE(3) (v4)
](https://arxiv.org/pdf/1003.2005v4)</a>

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

<a id="ref7">[7] [Minimum snap trajectory generation and control for quadrotors
](https://ieeexplore.ieee.org/document/5980409)</a>

<a id="ref8">[8] [Control of Complex Maneuvers for a Quadrotor UAV using Geometric Methods on SE(3) (v3)
](https://arxiv.org/pdf/1003.2005v3)</a>
