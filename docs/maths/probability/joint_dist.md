# **Joint Distribution**

The joint distribution describes the probability of two or more random variables occurring simultaneously.  Joint distribution of $\mathbf{X}$ and $\mathbf{Y}$ in discrete case, defined as :

$$
P(\mathbf{X}=x, \mathbf{Y}=y) = P(\mathbf{X}=x)P(\mathbf{Y}=y)
$$

then, 

- $P(\mathbf{X}=x, \mathbf{Y}=y) \ge 0$
- $\sum_{x,y} P(\mathbf{X}=x, \mathbf{Y}=y)=1$
- The CDF of joint distribution are $P[(x,y)\in A] = \int_A f(x, y) \; dxdy$

Each individual distribution within the joint distribution is called the marginal distribution:

- $P(\mathbf{X}=x) =\sum_y P(\mathbf{X}=x)$
- $P(\mathbf{Y}=Y) =\sum_x P(\mathbf{Y}=y)$

Both called {==marginal distribution==} of $\mathbf{X}$ and $\mathbf{Y}$, and the marginal density is defined as:

 - $f_\mathbf{X}(x) = \int_{-\infty}^{\infty} f(x,y) \; dy$
 - $f_\mathbf{Y}(y) = \int_{-\infty}^{\infty} f(x,y) \; dx$

where $f(x,y)$ is the joint PDF.
The {==conditinal distributions==} of $\mathbf{X}$ and $\mathbf{Y}$, 

$$
P(\mathbf{X}=x|\mathbf{Y}=y)=\frac{P(x, y)}{P_\mathbf{Y}(y)}
$$

and

$$
f_{\mathbf{Y}|\mathbf{X}}(y|x) =\frac{f(x,y)}{f_\mathbf{X}(x)}
$$

## **Independence of Joint Probability**

Independence of joint probability same as single probability. Multiple random variables $X_1, X_2, X_3, \cdots X_n$ are independent, if for all $(x_1, x_2, x_3, \cdots x_n)\in \mathbb{R}^n$.

- If $X_1, X_2, X_3, \cdots X_n$ are discrete, then 
    
    $$
    P(X_1=x_1, X_2=x_2, X_3=x_3, \cdots X_n=x_n) = P(X_1=x_1)P(X_2=x_2)P(X_3=x_3)\cdots P(X_n=x_n)
    $$

- If $X_1, X_2, X_3, \cdots X_n$ are continous, then

    $$
    f(X_1=x_1, X_2=x_2, X_3=x_3, \cdots X_n=x_n) = f(X_1=x_1)f(X_2=x_2)f(X_3=x_3)\cdots f(X_n=x_n)
    $$

- If $X_1, X_2, X_3, \cdots X_n$ are independent and identically distribute (IID), then they have same CDF, the same means, and the same variances

