# **Bernoulli Distribution**

A Bernoulli distribution is a distribution with two possible outcomes: the event happens or does not happen. A random variable $\mathbf{X}$ in a Bernoulli distribution can take two values:

$$
\mathbf{X} = \{0, 1\}
$$

where 0 means the event does not happen, and 1 means the event does happen.

Thus,

$$
P(\mathbf{X} = 1) = p \\
P(\mathbf{X} = 0) = q = 1 - p 
$$

where $p$ indicates the probability of success (or occurrence of the event), and $1 - p$ is the probability of failure (or non-occurrence).

## **Bernoulli $\rightarrow$ Binomial**

When we conduct $n$ independent Bernoulli trials $(B_1, B_2, B_3, \dots, B_n)$, each with the same probability of success $p$, the distribution of the sum of successes follows a **Binomial Distribution**.

Let $\mathbf{X} =$ total number of successes $= B_1 + B_2 + \dots + B_n$, where $B_i \sim \text{Bernoulli}(p)$.

Then,

$$
\mathbf{X} \sim \text{Binomial}(n, p) \\
P(\mathbf{X} = k) = \binom{n}{k} p^k q^{n - k}
$$

where $n$ and $p$ are the parameters representing the number of trials and the probability of success, respectively.