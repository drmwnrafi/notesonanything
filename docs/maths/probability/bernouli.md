# **BernoulLi Distribution**

Bernoulli distribution is a distribution has two possible outcomes, event happen or not happen. Random variable $\mathbf{X}$ that can take two values.

$$
\mathbf{X} = \{0, 1\}
$$

Where 0 means event doesnt happen and 1 means event does happen.

Thus, 

$$
P(\mathbf{X} = 1) = p \\
P(\mathbf{X} = 0) = q = 1-p 
$$

Where $p$ indicates the probability of success/happen, so $1-p$ is the probability of failure/not happen.

## **Bernoulli $\rightarrow$ Binomial**

When we conduct $n$ independent Bernoulli random trials $(B_1, B_2, B_3, \cdots, B_n)$ with each trial has same probability of success $p$, the distibution will leads the distribution to {==Binomial Distibution==}.

Let $\mathbf{X} =$ total number of success $=B_1+B_2+\cdots+B_n$, where $B_i \sim Bernoulli(p)$ 

$$
\mathbf{X} \sim \text{Binomial}(n, p) \\
P(\mathbf{X}=k) = \binom{n}{k}p^kq^{n-k}
$$

where $n$ and $p$ the parameters, number of trials and probability of success, respectively.
