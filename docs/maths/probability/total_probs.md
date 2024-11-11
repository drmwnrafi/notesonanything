# **Law of Total Probability**

Given $B_1, B_2, B_3, \cdots$ are partition of $\Omega$, thus $\Omega = \bigcup_{i=1}^n B_i$. Let $P(A)$ is a total probability. The solution to find total probability same as summed all areas of province then the area of whole country covered.

$$
P(A) = \sum_{i=1}^n P(A \cap B_i) = \sum_{i=1}^n P(A|B_i)P(B_i)
$$

{++Proof++}

$B_i$ is a partition of $\Omega$, so

$$
\Omega = \bigcup_{i=1}^n B_i \\
\begin{align*}
A &= A \cap \Omega\\
&= A \cap \left(\bigcup_i B_i \right)\\
&=\bigcup_i(A \cap B_i)
\end{align*}
$$

Then by using the third axioms of probability, 

$$
\begin{align*}
P(A) &= P\left(\bigcup_i(A \cap B_i)\right) \\
&= \sum_i P(A\cap B_i) \\
&= \sum_i P(A|B_i)P(B_i)
\end{align*}\\
$$

To make it more ease to understand, I use illustration on Steve's video.

<img src="../../../assets/media/total_probs.png" width="300px" style="display: block; margin: auto;">

Based on the illustration, 

$$
P(A) = P(A \cap B) + P(A \cap B^c)
$$

By using definition of conditional distribution, we can change the equation into

$$
P(A) = P(A|B)P(B) + P(A|B^c)P(B^c)
$$

It is more easy intuitively, since A overlap with $B_1$ and $B_2$, it means $A \cap B$. Where the entire left area of $B$ is $B^c$, so the total area of left area is $A \cap B^c$.


!!! info
    Most of this section refered to [Review of Probability Theory by Maleki and Do](https://cs229.stanford.edu/section/cs229-prob.pdf), [A First Course in Probability by Sheldon Ross](https://www.cs.utexas.edu/~abdonm/SDS%20321/a_first_course_in_probability.pdf), and [Probability Bootcamp by Steve Bruton](https://www.youtube.com/watch?v=sQqniayndb4&list=PLMrJAkhIeNNR3sNYvfgiKgcStwuPSts9V)