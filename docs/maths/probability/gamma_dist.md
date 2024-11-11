# **Gamma Distribution**

The exponential distribution computes the probability of the interval time between a single event. The {==Gamma distribution==} is used for calculating the probability of the interval time for the $r$-th arrivals. The probability density function (PDF) of the Gamma distribution is:

$$
f(t, r, \lambda) = \frac{e^{-\lambda t} \lambda^r t^{r-1}}{\Gamma(r)} 
$$

where $\Gamma(r)=(r-1)!$, and $\lambda$ is rate. Denote $T_r$ as the random time for the $r$-th arrival, and $I$ as the single event interval time: 

$$
T_r = I_1 + I_2 + I_3 + \cdots + I_r \\
T_r \sim \text{Gamma}(r, \lambda) 
$$

The cumulative distribution function (CDF) of the Gamma distribution for  $r > 0$ :

$$
P(T_r > t) = P(N_t \leq r-1) = \sum_{k=0}^{r-1} \frac{e^{-\lambda t}(\lambda t)^t}{k!}
$$

where $T_r$ is the continuous time interval for the $r$-th arrival, and $N_t$ is the number of Poisson events that occur over the time interval $t$.

