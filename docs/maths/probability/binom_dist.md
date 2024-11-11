# **Binomial Distribution and Multinomial Distribution**

This usefull when calculating how many our probability of 5 cards out of 52 with 10 independent trails. Use Binomial Distribution if there only 2 possible outcomes (Bernoulli trials) in each trial (success and failure), e.g we want all 5 cards to be hearts. Multinomial Distibution if there more than 2 possible outcomes, e.g we want 3 hearts, 1 spade, and 1 diamond, so there are 3 possible outcomes (hearts, spades, and diamonds).

## **The Binomial**

For a [recall](../intro_probs/), to calculate samples `r` out of `n` when order doesn't matter and w/o replacement we use $\binom {n}{r}$ this is {==Binomial Coefficient==}. Binomial coefficent calculate by following equation: 

$$
\binom{n}{\underbrace{n-k, k}_{\text{2-groups}}}=\binom{n}{k}=\frac{n!}{(n-r)!r!}
$$

To expand the expression of n-order polynomial function, we use {==Binomial Theorem==}.

$$
(x+y)^n = \sum^n_{k=0} \binom{n}{k}x^k y^{n-k}
$$

Above equation leads us to Pascal's Triangle.

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

We can notice it, larger `n` in Binomial Theorem it start converge to `normal distribution`.

In {==Binomial Distribution==}, we modeled the probability of numbers of event succeses in fixed number of independent trials with identical probability distribution (IID) on each trial. Probability of $k$ success of $n$ sample, given as follows :

$$
P(A = k) = \binom{n}{k}p^k(1-p)^{n-k}
$$

where $p$ and $1-p$ are probability of success and failure on single trial, respectively.

## **The Multinomial**

Where binomial distribution handle only 2 possible outcomes, multinomial handled more than 2 possible outcomes or categorical. So, multinomial distribution is a generalization of the binomial distribution. In multinomial we devided our samples to `m` groups, where in binomial it's only 2 groups. So, the following equation give us the {==Multinomial Coefficient==}.

$$
\binom{n}{\underbrace{n_1, n_2, n_3, ..., n_m}_{\text{m-groups}}}=\frac{n!}{n_1! \cdot n_2! \cdot n_3! \cdots n_m!}
$$

with constraint $n_1 + n_2 + n_3 + \cdots + n_m = n$

{++Proof++}

!!! tip "Fun Fact"
    Steve Bruton said, "it's pretty easy" when he explain the proof, that's why I love his videos 😂

$$
\require{cancel}
\begin{align*}
\binom{n}{n_1, n_2, \dots, n_m} &= \binom{n}{n_1} \binom{n - n_1}{n_2} \binom{n - n_1 - n_2}{n_3} \cdots \binom{n - n_1 - n_2 - \dots - n_{m-1}}{n_m} \\
&= \frac{n!}{n_1! \cancel{(n - n_1)!}} \cdot \frac{\cancel{(n - n_1)!}}{n_2! \cancel{(n - n_1 - n_2)!}} \cdot \frac{\cancel{(n - n_1 - n_2)!}}{n_3! \cancel{(n - n_1 - n_2 - n_3)!}} \cdots \frac{\cancel{(n - n_1 - n_2 - \dots - n_{m-1})!}}{n_m! (0)!}
\end{align*}
$$

Binomial theorem handles only 2 variabels $x$ and $y$, where multinomial theorem handles $>2$ variabels $x_1, x_2, x_3, \cdots, x_m$. To expands the polinomial with $>2$ variables we can use {==Multinomial Theorem==}. The equation pretty similar to binomial theorem, with sum over all $n_m$ times all of products of $x_i \in \mathbb{Z}^{+}$ and $n\in\mathbb{Z}^{0+}$ with multinomial coefficient.

$$
(x_1 + x_2 + x_3 + \cdots + x_m)^n = \sum_{\substack{(n_1, n_2, \cdots n_m) : \\n_1 + n_2 + \cdots + n_m = n}} \binom{n}{n_1, n_2, \dots, n_m} x_1^{n_1} \cdot x_2^{n_2} \cdots x_m^{n_m}
$$

The probability of multinomial samples has same working principle with binomial one, works with sequence of n independent trials and identical probability distribution (IID). 

$$
P(X_1 = n_1, X_2 = n_2, \dots, X_m = n_m) = \binom{n}{n_1, n_2, \dots, n_m} p_1^{n_1} \cdot p_2^{n_2} \cdots p_m^{n_m}
$$

where $\sum_{i=1}^m n_i=n$


!!! info
    Most of this section refered to [Review of Probability Theory by Maleki and Do](https://cs229.stanford.edu/section/cs229-prob.pdf), [A First Course in Probability by Sheldon Ross](https://www.cs.utexas.edu/~abdonm/SDS%20321/a_first_course_in_probability.pdf), and [Probability Bootcamp by Steve Bruton](https://www.youtube.com/watch?v=sQqniayndb4&list=PLMrJAkhIeNNR3sNYvfgiKgcStwuPSts9V)