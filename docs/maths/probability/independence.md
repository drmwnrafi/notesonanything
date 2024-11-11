# **Independence**

Two events are classified as independent if the occurrence of one has no effect on the occurrence of the other. Given events $A$ and $B$, if they are independent, then $P(A\cap B) = P(A) \cdot P(B)$

Thus, 

$$
P(A|B) = P(A) \\
P(B|A) = P(B)
$$

{++Proof++} 

We can use conditional probability to prove this. The event $A$ is independent if its conditional probability is equal to  $P(A)$.

$$
P(A|B) = \frac{P(A \cap B)}{P(B)} = \frac{P(A)\cdot P(B)}{P(B)} = P(A)\\
P(B|A) = \frac{P(A \cap B)}{P(A)} = \frac{P(A)\cdot P(B)}{P(A)} = P(B)
$$

So, when events $A$ and $B$ are independent, knowing that $B$ occurred does not change the probability of $B$, and vice versa.

Let say we want to toss die and coin and calculate the probability of getting both a 5 on the die and a head on the coin. The result of the coin toss does not influence the outcome of the die roll, and vice versa.

The sample spaces for each event are:

$$
\Omega_A = \{H, T\}\\
\Omega_B = \{1, 2, 3, 4, 5, 6 \}
$$

The probability of each event is:

$$
P(A) = \frac{1}{2} \\
P(B) = \frac{1}{6}
$$

The probability of both events happening is:

$$
P(A\cap B) = \frac{1}{2} \cdot \frac{1}{6} = \frac{1}{12}
$$