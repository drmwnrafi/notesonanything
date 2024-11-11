# **Independence**

Two events classify as independent event if one occured have no influence to other event. Given event $A$ and $B$ because they are independent so $P(A\cap B) = P(A) \cdot P(B)$

Then, 

$$
P(A|B) = P(A) \\
P(B|A) = P(B)
$$

{++Proof++} 

We use conditional probability to proof it. The event $A$ independent, if conditional probability is equal to $P(A)$.

$$
P(A|B) = \frac{P(A \cap B)}{P(B)} = \frac{P(A)\cdot P(B)}{P(B)} = P(A)\\
P(B|A) = \frac{P(A \cap B)}{P(A)} = \frac{P(A)\cdot P(B)}{P(A)} = P(B)
$$

So, when events $A$ and $B$ are independent, knowing that $B$ occurred does not change the probability of $B$, and vice versa.

Let say we want to toss die and coin and we wanna know the probability both 5 side of die and head happen. The result of the coin toss does not influence the outcome of the dice roll, and vice versa.

$$
\Omega_A = \{H, T\}\\
\Omega_B = \{1, 2, 3, 4, 5, 6 \}
$$

$$
P(A) = \frac{1}{2} \\
P(B) = \frac{1}{6}
$$

Probability both events happening is:

$$
P(A\cap B) = \frac{1}{2} \cdot \frac{1}{6} = \frac{1}{12}
$$