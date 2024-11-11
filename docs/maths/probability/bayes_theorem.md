---
toc: TRUE
---

# **Bayes' Theorem**

If conditional distribution based on occurring probability of an event given another occured event. {==Bayes Theorem==} is the "inverse" of it, allowing us to update the occured probability of an event based on new evidence.

$$
P(B|A) = \frac{P(A|B)P(B)}{P(A)}
$$

Subtitutes $P(A)$ from [laws of total probability](../total_probs/), so the equation changed as follows

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

Therefore, the probability of a person being a man is $P(\text{man}) = \frac{40}{100} = 0.4$ and probability of person wearing pink is $P(\text{pink}) = \frac{25}{100} = 0.25$. To calculate {==conditional probability==} of a man wearing pink, where probability of man wearing pink is $P(\text{man} \cap \text{pink}) \neq P(\text{man}) \times P(\text{text})$ because the events are not independent event. Then,

$$
P(\text{man} \cap \text{pink}) = \frac{5}{100} = 0.05 \\
P(\text{pink}|\text{man}) = \frac{P(\text{pink} \cap \text{man})}{P(\text{man})} = \frac{0.5}{0.4} = 0.125
$$

{==Bayes' theorem==} is a "inverse" of conditional probability $P(A|B)$, so it is $P(B|A)$. On our case, $P(\text{man}|\text{pink})$ is the probability of person wears pink is a man.  

$$
P(\text{man}|\text{pink})=\frac{P(\text{pink}|\text{man})P(\text{man})}{P(\text{pink})} = \frac{0.4 \times 0.125}{0.25} = 0.2
$$

!!! info
    Most of this section refered to [Review of Probability Theory by Maleki and Do](https://cs229.stanford.edu/section/cs229-prob.pdf), [A First Course in Probability by Sheldon Ross](https://www.cs.utexas.edu/~abdonm/SDS%20321/a_first_course_in_probability.pdf), and [Probability Bootcamp by Steve Bruton](https://www.youtube.com/watch?v=sQqniayndb4&list=PLMrJAkhIeNNR3sNYvfgiKgcStwuPSts9V)