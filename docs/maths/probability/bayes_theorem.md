# **Bayes' Theorem**

Conditional probability describes the likelihood of an event occurring given that another event has already occurred. {==Bayes Theorem==} is the "inverse" of this relationship, allowing us to update the probability of an event based on new evidence.

$$
P(B|A) = \frac{P(A|B)P(B)}{P(A)}
$$

By substituting $P(A)$ from the [laws of total probability](../total_probs/), the equation becomes :

$$
P(B|A) = \frac{P(A|B)P(B)}{P(A \cap B) + P(A \cap B^c)}
$$

I took examples from this [blog](https://www.mathsisfun.com/data/bayes-theorem.html). 

Given information :

- Total people: 100
- Men: 40
- Not men: 60
- Men who wear pink: 5
- Men who do not wear pink: 35
- Not men who wear pink: 20
- Not men who do not wear pink: 40

Thus, the probability of a person being a man is $P(\text{man}) = \frac{40}{100} = 0.4$ and the probability of a person wearing pink is $P(\text{pink}) = \frac{25}{100} = 0.25$. To calculate the {==conditional probability==} that a man wears pink, note that $P(\text{man} \cap \text{pink}) \neq P(\text{man}) \times P(\text{text})$ because the events are not independent event. Then,

$$
P(\text{man} \cap \text{pink}) = \frac{5}{100} = 0.05 \\
P(\text{pink}|\text{man}) = \frac{P(\text{pink} \cap \text{man})}{P(\text{man})} = \frac{0.5}{0.4} = 0.125
$$

{==Bayes' theorem==} provides the "inverse" of the conditional probability $P(A|B)$, allowing us to find $P(B|A)$. In this case, $P(\text{man}|\text{pink})$ represents the probability that a person who wears pink is a man:  

$$
P(\text{man}|\text{pink})=\frac{P(\text{pink}|\text{man})P(\text{man})}{P(\text{pink})} = \frac{0.4 \times 0.125}{0.25} = 0.2
$$