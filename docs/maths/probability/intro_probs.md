# **Introduction to Probablity**

Probability measures how likely an event $A$ can happen. It is symbolized by $P(A)$. To compute the probability, we can use the following equation : 

$$
P(A) = \frac{\text{Number of event } A \text{ can happen }}{\text{Total number of possible outcomes}}
$$

**Example**:
If we roll a die and want to measure how likely side 3 (event $A$) will show up, there are 6 possible outcomes (1, 2, 3, 4, 5, 6).

$$
P(A) = \frac{1}{6} \approx 0.1667
$$

This is only one sample. To calculate total number of possible outcomes, we can use this formula : 

Where $n$ is the number of unique items to choose and $r$ is the number of being chosen.

In **permutation**, order is important. For example, 1234 is different with 4321, as in case of a PIN-based lock.
Otherwise in **combination**, order not important, so 1234 is the same as 4321.

**Replacement** means that the same items can be chosen more than once.
**No replacement** means that the same item cannot be selected more then once.

|Order Matters|Replacement|Formula|Example|
|:--:|:--:|:--:|:--:|
|Yes|Yes|$n^r$|Rolling 2 dice for 10 times and order matters (side 1 & 3 is different from 3 & 1), each roll is independent, $(6 \times 6)^{10}$|
|Yes|No|${}_{n}P_r = \frac{n!}{(n-r)!}$|Choosing 3 books from 10 books in a specific order, without putting any back, $\frac{10!}{7!}$|
|No|Yes|${}_{n+r-1}C_r = \binom {n+r-1}{r} = \frac{(n+r-1)!}{(n-1)!r!}$|Selecting 3 scoops of ice cream flavors from 5 flavors, where can pick same flavor more than once, $\binom {7}{4}$|
|No|No|${}_{n}C_r=\binom {n}{r}=\frac{n!}{(n-r)!r!}$|Choosing 3 books from a shelf of 10 without regard to order and without replacement, $\binom {10}{3}$|

*How can that be?*

- **Permutation with replacement**, when we have 2 dice, we have 36 choices one time. When we throw them 10 times, each time we still have 36 choices.
- **Permutation without replacement**, when choosing 3 books from 10, so the number of ways to choose all 10 books is 10!, because the number of available choices decreasws each time. But remember permutation care about the order, so we devided with (10-3)!.
- **Combinations without replacement**, if we are choosing 2 books (maths and physics) from 10 books where the order doesn't matter, we have $\frac{10!}{8!}$ possible choices. So, we have 4 possible choices; maths & physics, maths & maths, physics & physics, physics & maths. Imagine the order matters, like first math book and then physics book, so there is only one choice. Then we can reduce the permutation w/o replacement by changing formula with multiply it to $\frac{1}{r!}$ to erase the copies from permutation.
- **Combinations with replacement**, this [blog](https://www.mathsisfun.com/combinatorics/combinations-permutations.html) makes it easy to understand the intuitive behind $n+r-1$. That blog explains by counting the actions. In example, there are 5 different ice cream flavors (A, B, C, D, E) and we take 3 scoops. We want all 3 scoops to be of the flavor C. Imagine starting from A, we skip A coz we want 3 of C, count 1. Then, we are in B, skip again, count 2. Now, arrive at C, scoop it, count 3, remember we need 3 of C, right now we only have one C so we need to scoop it 2 more, the count becomes 5. Then we skip C and move to D, count 6. We skip D and move to E, count 7. It is same value as $n+r-1$, where `n = 5` and `r = 3`, giving us 7, same as I illustrated before.

!!! info
    Most of this section refered to [Review of Probability Theory by Maleki and Do](https://cs229.stanford.edu/section/cs229-prob.pdf), [A First Course in Probability by Sheldon Ross](https://www.cs.utexas.edu/~abdonm/SDS%20321/a_first_course_in_probability.pdf), and [Probability Bootcamp by Steve Bruton](https://www.youtube.com/watch?v=sQqniayndb4&list=PLMrJAkhIeNNR3sNYvfgiKgcStwuPSts9V)