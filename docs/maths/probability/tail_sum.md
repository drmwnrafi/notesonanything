# **Tail Sum**

If is $\mathbf{X}$ is a non-negative random variable, then

- For discrete :

    $$
    E(\mathbf{X})=\sum_{k=1}^n P(\mathbf{X}=k)
    $$

- For continious :

    $$
    E(\mathbf{X})=\int_0^{\infty} P(\mathbf{X}>k) \quad dt
    $$

Example :

$$
E(\mathbf{X})=\sum_{k=1}^n P(\mathbf{X}=k)=\sum_{k=0}^n kP(\mathbf{X}=k) \\
\begin{matrix}
P(\mathbf{X}\ge 1) & P_1+P_2+P_3+\cdots+P_{n-1}+P_n \\
P(\mathbf{X}\ge 2) & \quad\quad+P_2+P_3+\cdots+P_{n-1}+P_n \\
P(\mathbf{X}\ge 2) & \quad\quad\quad\quad+P_3+\cdots+P_{n-1}+P_n \\
\vdots & \vdots \\
P(\mathbf{X}\ge n-1) & \quad\quad\quad\quad\quad\quad\quad\quad+P_{n-1}+P_n \\
P(\mathbf{X}\ge n) & \quad\quad\quad\quad\quad\quad\quad\quad\quad\quad+P_n \\
\end{matrix} \\
P(\mathbf{X}\ge k) = 1-P(\mathbf{X}\leq k)
$$