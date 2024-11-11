# **Gamma Distribution**

Exponential distribution compute probability of interval time between single event. {==Gamma distribution==} used for calculating probability of interval time for $r$-th arrivals. Then, PDF of Gamma distribution is :

$$
f(t, r, \lambda) = \frac{e^{-\lambda t} \lambda^r t^{r-1}}{\Gamma(r)} 
$$

where $\Gamma(r)=(r-1)!$, $\lambda$ is rate. Donated $T_r$ is random time and $I$ is single event interval time, 

$$
T_r = I_1 + I_2 + I_3 + \cdots + I_r \\
T_r \sim \text{Gamma}(r, \lambda) 
$$

So, the cummulative distribution function of Gamma Distribution for $r > 0$ :

$$
P(T_r > t) = P(N_t \leq r-1) = \sum_{k=0}^{r-1} \frac{e^{-\lambda t}(\lambda t)^t}{k!}
$$

where $T_r$ is continious time interval for $r$-th arrival, and $N_t$ is number of Poisson events over time $t$.

