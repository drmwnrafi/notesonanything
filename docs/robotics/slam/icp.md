# **Point to Point Iterative Closest Point**

Given two point clouds, $P$ and $Q$:

$$
P = \left \{ p_1, p_2, p_3, ..., p_n \right \}\\
Q = \left \{ q_1, q_2,q_3, ..., q_n \right \}\\
$$

The goal is to find the optimal transformation $(R, t)$ that aligns the source point cloud $P$ to the reference point cloud $Q$ by minimizing the following error function :

$$
E = \underset{R,t} {\mathrm{argmin}} \sum_{i=1}^n \left||Rp_i + t -q_i \right||^2
$$

where $E$ is the loss function, $P$ is the set of source points, and $Q$ is the set of reference points. The objective of ICP is to determine the best transformation $(R, t)$ that aligns point cloud $P$ to point cloud $Q$.


## **Closed-form Solution**

Point to point ICP can solved by using {==Singular Value Decomposition==}. First, the centroids (center of mass) of point clouds $P$ and $Q$ are defined as:

$$
\mu_p = \frac{1}{N_Q} \sum_{n=1}^{N_Q} q_i\\
\mu_q = \frac{1}{N_P} \sum_{n=1}^{N_P} p_i
$$

Let $Q'$ and $P'$ be the sets of points in $Q$ and $P$, respectively, with their centroids subtracted. This step removes any translation effects by shifting both point clouds to a common origin:

$$
Q' = {Q_i-\mu_q}=\{Q'_i\}\\
P' = {P_i-\mu_p}=\{P'_i\}\\
$$

Define the cross-covariance matrix $W$ as follows:

$$
W = \sum_{i=1}^N q'_ip'^T_i = Q'^TP'\\
W = \begin{bmatrix}
cov(p_x,q_x) & cov(p_x,q_y)\\
cov(p_y,q_x) & cov(p_y,q_y)
\end{bmatrix}
$$

The cross-covariance matrix $W$ provides information about how changes in coordinates in the $p$ points correlate with changes in the $q$ points. An ideal cross-covariance matrix would be an identity matrix, indicating no correlation between the x- and y-axes.

## **Translation**
    
The translation vector $t$ aligns the centroids of $P$ and $Q$ and can be derived by minimizing $E$ with respect to $t$:

$$
\frac{\partial E}{\partial t} = 2\sum_{i=1}^n (q_i - Rp_i-t) = 0\\
t = q_i-Rp_i
$$

## **Rotation**
The optimal rotation matrix $R$ can be derived by focusing on minimizing the rotational alignment error when $t = 0$. This minimization yields:

$$
R= \underset{R\in SO(n)}{argmin}\sum_{i=1}^n\left\|Rp_i - q_i \right\|^2
$$

Expanding this norm gives:

$$
\begin{align*}
\left\|Rp_i - q_i \right\|^2 &= (Rp_i - q_i)^T(Rp_i - q_i) \\
&= p_i^T R^T R p_i - q_i^T R p_i - p_i^T R^T q_i - q_i^T q_i \\
&= p_i^T p_i - q_i^T R p_i - p_i^T R^T q_i - q_i^T q_i
\end{align*}
$$

Where $p_i^TR^Tq_i = \left ( p_i^TR^Tq_i\right)^T= q_i^TRp_i$, then

$$
\begin{align*}
\underset{R\in SO(d)}{\mathrm{argmin}}\sum_{i=1}^n \left\|Rp_i - q_i \right\|^2 &= 
\underset{R\in SO(d)}{\mathrm{argmin}} \sum _{i=n}^n (p_i^T p_i - 2q_i^T R p_i - q_i^T q_i)
\end{align*}
$$

Since $p_i^T p_i$ and $q_i^T q_i$ do not depend on $R$, we simplify the objective to:

$$
\underset{R\in SO(d)}{\mathrm{argmax}}\sum_{i=1}^nq_i^T R p_i
$$

Using the matrix trace property $tr(AB)=tr(BA)$, and $PQ^T=W$, we can rewrite the objective as:

$$
\begin{align*}
\sum_{i=1}^nq_i^T R p_i &= \text{tr}(Q^TRP)\\
&= \text{tr}(RPQ^T) \\
&=\text{tr}(RW)
\end{align*}
$$

Where $W$ is a cross-covariance matrix, then perform the {==Singular Value Decomposition (SVD)==} of $W$:

$$
SVD(W) = U \Sigma V^T
$$
Subtitute $U \Sigma V^T$ to $tr(RW)$

$$
\text{tr}(RU\Sigma V^T) = \text{tr}(\Sigma V^TRU)
$$

Define an orthogonal matrix $M =V^TRU$. Therefore,

$$
\text{tr}(\Sigma V^TRU) = \text{tr}(\Sigma M)
$$

By applying the Cauchy-Schwarz inequality :

$$
\text{tr}(\Sigma M)= \sum_{i=1}^r\sigma_im_{ii}\\
\left(\sum_{i=1}^r\sigma_im_{ii}\right)^2 \leq \sum_{i=1}^r\sigma_i^2  \sum_{i=1}^r m_{ii}^2\\
\sum_{i=1}^r\sigma_im_{ii} \leq \sqrt{\sum_{i=1}^r\sigma_i^2  \sum_{i=1}^r m_{ii}^2}\\
\sum_{i=1}^r\sigma_im_{ii} \leq \sum_{i=1}^r\sigma_i^2
$$

Since $M$ is an orthogonal matrix, $m_{ii}^2 = |m_{ii}|\leq1$. Therefore, to obtain the maximum value, $m_{ii}$ bernilai 1. should be 1. As explained earlier $M$ is an orthogonal matrix, so $m_{ii}$ equals 1 if $M = I$​

$$
M = I = V^TRU\\
R^T = V^TU\\
R = UV^T
$$

### **References** 

- Sorkine-Hornung, O., & Rabinovich, M. (2017). Least-squares rigid motion using svd. *Computing*, *1*(1), 1-5. https://igl.ethz.ch/projects/ARAP/svd_rot.pdf
- Arun, K. S., Huang, T. S., & Blostein, S. D. (1987). Least-Squares fitting of two 3-D point sets. *IEEE Transactions on Pattern Analysis and Machine Intelligence*, *PAMI-9*(5), 698–700. https://doi.org/10.1109/tpami.1987.4767965