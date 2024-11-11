# **Poisson Proccess**

Poisson distribution is a distribution of number of events happended given interval time $t$. PDF of posisson distribution is :

$$
P(\mathbf{X}=k)=\frac{\lambda^ke^{-k}}{k!}
$$

where $\lambda$ is expected number of events over time interval or rate $\times$ time interval.

Poisson proccess is a proccess to determine the distribution of the time elapsed between $n-1$ and $n$-th event. To calculate this we used exponential distribution with mean $1/\lambda$.

Poisson process has the following properties:

- Number of events that occur in different time interval are independent 
- Distribution of number of events that occure in given interval depands only on the length not the location on time domains.
- Probability of two events occuring at exact same time is 0. 

Example, a rate of 5 orders in an hour.Compute probability of number of events in interval 2-hour is a Poisson distribution with mean $\lambda \times t$, given $\lambda = 5$ and $t=2$ :

$$
\mathbf{X}(2) \sim \text{Poisson}(10)
$$

Then, the probability of receiving 8 orders in 2-hour :

$$
P(\mathbf{X}=8) = \frac{10^8e^{-10}}{8!} \approx 0.1126
$$

To compute probability of time interval of order arrival is an exponential distribution, so the average time between intervals is :

$$
\text{E}(T) = \frac{1}{\lambda} = 0.2 \text{ hour}
$$

Then, we can compute probability of next order arrive in next 15 minutes by using CDF of exponential distribution :

$$
15 \text{ minutes} = \frac{15}{60} =0.25 \text{ hour}\\
P(T \leq 0.25) = 1-e^{-5\cdot 0.25} \approx 0.7135
$$