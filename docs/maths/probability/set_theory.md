# **Set Theory**

In [this blog](https://drmwnrafi.github.io/notesonanything/Mathematics/Probability/intro_probs/), we donated probability of event $A$ by $P(A)$. 

1. The set of all possible outcomes or **sample space** of random experiment donates by $\Omega$. Our probability $P(A)$ map from subsets of $\Omega$ to $\mathbb{R}$, where it valued with $[0,1] \in \mathbb{R}$. 
Each outcome of experiment $\omega \in \Omega$ encapsulates all information at the end of specific experiment.  
2. **Event space** $\Sigma : A$ is the set of all outcomes of experiment takes. $\Sigma$ is subset of $\Omega$.

3. Properties of $P$, $\Sigma$ has to stisfy three of these properties:

    - $P(\Omega) = 1$
    - If $A\subset\Omega$ then $P(A) \ge 0$
    - If $A_i$ and $A_j$ are disjoint events, then $A_i \cap A_j = \emptyset$ with $i\neq j$. Disjoint means $A_i$ and $A_j$ are separate.

Those three of properties are known as **Axioms of Probability**.
Then, 

$$
P : \Sigma \rightarrow [0, 1]
$$

Other Properties : 

- If $A_i$ and $A_j$ are separate, $P(A_i\cup A_j) = P(A_i) + P(A_j)$
- If $A_i$ and $A_j$ are not separate and independent, $P(A_i \cap A_j) = P(A_i) \times P(A_j)$
- $P(A_i \cup A_j) = P(A_i) + P(A_j) - P(A_i \cap A_j)$
- $A^c = 1-P(A)$
- If $A$ is empty set, then $P(A) = P(\emptyset)=0$
- If $A \subseteq B$, then $P(A) \leq P(B)$

Example :
If we throw 3 coin flips. The sample space we have is :

$$
\Omega = \{ HHH, HHT, HTH, HTT, THH, THT, TTH, TTT\}
$$

There are 8 possible outcomes, with `H is head` and `T is tail`.

We take 2 events where $A$ and $B$ are independent,
the set of outcomes where first flip is a head $\{HHH, HHT, HTH, HTT\}$, donated by $A$.The set of outcomes where second flip is a tail $\{HTH, HTT, TTH, TTT\}$, donated by $B$. So,

$$
P(A) = \frac{4}{8} = \frac{1}{2}\\
P(B) = \frac{4}{8} = \frac{1}{2}
$$

- Every outcomes in both $A$ and $B$, $P(A \cap B) = P(A) \times P(B)$

$$
P(A \cap B) = \{HHH, HTT\} = \frac{1}{2} \times \frac{1}{2} = \frac{1}{4}
$$

- Every outcomes in $A$ or $B$, $P(A \cup B) = P(A) + P(B) - P(A \cap B)$

$$
P(A \cup B) = \{HHH, HHT, HTH, HTT, TTH, TTT\} = \frac{1}{2} + \frac{1}{2} - \frac{1}{4} = \frac{3}{4}
$$

<!-- <img src="../../../assets/media/a_or_b.png" width="500px" style="display: block; margin: auto;"> -->

- Every outcomes are not in $A$, $A^c = 1-P(A)$

$$
A^c = \{THH, THT, TTH, TTT\} = 1 - \frac{1}{2} = \frac{1}{2}
$$


!!! info
    Most of this section refered to [Review of Probability Theory by Maleki and Do](https://cs229.stanford.edu/section/cs229-prob.pdf), [A First Course in Probability by Sheldon Ross](https://www.cs.utexas.edu/~abdonm/SDS%20321/a_first_course_in_probability.pdf), and [Probability Bootcamp by Steve Bruton](https://www.youtube.com/watch?v=sQqniayndb4&list=PLMrJAkhIeNNR3sNYvfgiKgcStwuPSts9V)