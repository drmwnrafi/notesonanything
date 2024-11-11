# **Poisson Proccess**

The Poisson distribution is a distribution of the number of events that occur within a given time interval $t$. PThe probability mass function (PMF) of the Poisson distribution is:

$$
P(\mathbf{X}=k)=\frac{\lambda^ke^{-k}}{k!}
$$

where $\lambda$ is the expected number of events over the time interval, or the rate times the time interval.

A Poisson process is a process that determines the distribution of the time elapsed between the $(n-1)$-th and $n$-th event. To calculate this, we use the exponential distribution with mean $1/\lambda$.

Poisson process has the following properties:

- The number of events that occur in different time intervals are independent.
- The distribution of the number of events that occur in a given interval depends only on the length of the interval, not the location in the time domain.
- The probability of two events occurring at the exact same time is 0.

Example, If the rate is 5 orders per hour, compute the probability of the number of events in a 2-hour interval. This is a Poisson distribution with mean 
$\lambda \times t$, given $\lambda = 5$ and $t=2$ :

$$
\mathbf{X}(2) \sim \text{Poisson}(10)
$$

Then, the probability of receiving 8 orders in 2 hours is:

$$
P(\mathbf{X}=8) = \frac{10^8e^{-10}}{8!} \approx 0.1126
$$

To compute the probability of the time interval between order arrivals, we use the exponential distribution. The average time between intervals is:

$$
\text{E}(T) = \frac{1}{\lambda} = 0.2 \text{ hour}
$$

Next, we compute the probability that the next order arrives in the next 15 minutes using the CDF of the exponential distribution:

$$
15 \text{ minutes} = \frac{15}{60} =0.25 \text{ hour}$$

Thus, the probability is:

$$
P(T \leq 0.25) = 1-e^{-5\cdot 0.25} \approx 0.7135
$$