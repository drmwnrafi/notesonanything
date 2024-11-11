# **Binomial Distribution and Multinomial Distribution**

This is useful when calculating the probability of drawing 5 cards out of 52 with 10 independent trials. Use the Binomial Distribution if there are only 2 possible outcomes (Bernoulli trials) in each trial (success and failure), e.g., if we want all 5 cards to be hearts. Use the Multinomial Distribution if there are more than 2 possible outcomes, e.g., if we want 3 hearts, 1 spade, and 1 diamond, which involve 3 possible outcomes (hearts, spades, and diamonds).

## **The Binomial**

For a [recall](../intro_probs/), to calculate the number of samples `r` out of `n` when order doesn't matter and w/o replacement we use $\binom {n}{r}$,  which is the {==Binomial Coefficient==}. Binomial coefficent is calculated using the following equation: 

$$
\binom{n}{\underbrace{n-k, k}_{\text{2-groups}}}=\binom{n}{k}=\frac{n!}{(n-r)!r!}
$$

To expand the expression of $n$-th order polynomial function, we use {==Binomial Theorem==}:

$$
(x+y)^n = \sum^n_{k=0} \binom{n}{k}x^k y^{n-k}
$$

This leads us to Pascal's Triangle:

$$
\begin{matrix}
(x+y)^0 &&&&&&&1&&&&&&\\
(x+y)^1 &&&&&&1&&1&&&&&\\
(x+y)^2 &&&&&1&&2&&1&&&&\\
(x+y)^3 &&&&1&&3&&3&&1&&&\\
(x+y)^4 &&&1&&4&&6&&4&&1&&\\
(x+y)^5 &&1&&5&&10&&10&&5&&1&\\
(x+y)^6 &1&&6&&15&&20&&15&&6&&1\\
\end{matrix}
$$

We can notice that as $n$ increases in the Binomial Theorem, it starts to converge to the normal distribution.

In {==Binomial Distribution==}, we model the probability of the number of event successes in a fixed number of independent trials with identical probability distributions (IID) for each trial. The probability of $k$ successes out of $n$ trials is given by :

$$
P(A = k) = \binom{n}{k}p^k(1-p)^{n-k}
$$

where $p$ and $1-p$ are probabilities of success and failure in a single trial, respectively.

## **The Multinomial**

Whereas the Binomial Distribution handles only 2 possible outcomes, the Multinomial Distribution handles more than 2 possible outcomes, or categorical outcomes. Therefore, the multinomial distribution is a generalization of the binomial distribution. In the multinomial case, we divide our samples into $m$ groups, whereas in the binomial case, there are only 2 groups. The following equation gives us the {==Multinomial Coefficient==}.

$$
\binom{n}{\underbrace{n_1, n_2, n_3, ..., n_m}_{\text{m-groups}}}=\frac{n!}{n_1! \cdot n_2! \cdot n_3! \cdots n_m!}
$$

with constraint $n_1 + n_2 + n_3 + \cdots + n_m = n$

{++Proof++}

!!! tip "Fun Fact"
    Steve Bruton said, "it's pretty simple" when he explain the proof, that's why I love his videos 😂

$$
\require{cancel}
\begin{align*}
\binom{n}{n_1, n_2, \dots, n_m} &= \binom{n}{n_1} \binom{n - n_1}{n_2} \binom{n - n_1 - n_2}{n_3} \cdots \binom{n - n_1 - n_2 - \dots - n_{m-1}}{n_m} \\
&= \frac{n!}{n_1! \cancel{(n - n_1)!}} \cdot \frac{\cancel{(n - n_1)!}}{n_2! \cancel{(n - n_1 - n_2)!}} \cdot \frac{\cancel{(n - n_1 - n_2)!}}{n_3! \cancel{(n - n_1 - n_2 - n_3)!}} \cdots \frac{\cancel{(n - n_1 - n_2 - \dots - n_{m-1})!}}{n_m! (0)!}
\end{align*}
$$

The Binomial Theorem handles only 2 variables $x$ and $y$, whereas the Multinomial Theorem handles more than 2 variables $x_1, x_2, x_3, \cdots, x_m$. To expand the polynomial with more than 2 variables $>2$, we can use {==Multinomial Theorem==}. The equation is quite similar to the Binomial Theorem, with a sum over all the $n_m$ terms and products $x_i \in \mathbb{Z}^{+}$ and $n\in\mathbb{Z}^{0+}$ , using multinomial coefficient.

$$
(x_1 + x_2 + x_3 + \cdots + x_m)^n = \sum_{\substack{(n_1, n_2, \cdots n_m) : \\n_1 + n_2 + \cdots + n_m = n}} \binom{n}{n_1, n_2, \dots, n_m} x_1^{n_1} \cdot x_2^{n_2} \cdots x_m^{n_m}
$$

The probability of multinomial samples works on the same principle as the binomial case, involving a sequence of $n$ independent trials with identical probability distributions (IID). The probability is given by:

$$
P(X_1 = n_1, X_2 = n_2, \dots, X_m = n_m) = \binom{n}{n_1, n_2, \dots, n_m} p_1^{n_1} \cdot p_2^{n_2} \cdots p_m^{n_m}
$$

where $\sum_{i=1}^m n_i=n$