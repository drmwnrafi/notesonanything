# **Random Variables**

A random variable ($\mathbf{X}$) is a function that assigns a numerical value to each outcome of an event in a sample space, quantifying a specific characteristic of the outcomes.

$$
\mathbf{X} : \Omega \rightarrow \mathbb{R}
$$

This is read as: the function $\mathbf{X}$ maps outcomes from the sample space $\Omega$ to real numbers $\mathbb{R}$, where each outcome $\omega \in \Omega$ is represented by $\omega$.

- **Continuous Random Variables**: Take values within a continuous range (uncountably infinite), e.g., the lifetime of a transistor.
- **Discrete Random Variables**: Take values within a discrete (countable) range, e.g., the number of coin flips resulting in heads.

## **Probability of Random Variables**

The probability of a random variable $P(\mathbf{X})$ is a function that defines the distribution associated with the random variable. For this, we use the Probability Mass Function (PMF) for discrete variables and the Probability Density Function (PDF) for continuous variables. To calculate probabilities, we need:

1. Specific values or ranges for $\mathbf{X}$.
2. The probability of each value or range for $\mathbf{X}$.

## **The Importance of Random Variables**

1. Summarize probabilities through $P(\mathbf{X})$ without calculating the probability of each outcome in an $n$-trial experiment individually.
2. Model random processes with the PDF, enabling us to hypothesize about the distribution.
3. Simplify visualization and computation for values within the PDF.
4. Construct new functions based on $\mathbf{X}$, such as $\mathbf{X}^2$, $E(\mathbf{X})$ (expected value), and $Var(\mathbf{X})$ (variance).
