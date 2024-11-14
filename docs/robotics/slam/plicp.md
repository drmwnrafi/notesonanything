# **Point to Line Iterative Closest Point**

$$
E = \underset{R,t} {\mathrm{argmin}} \sum_{i=1}^n \left||(y_i - Rx_i + t)n_{y,i} \right||^2
$$

$n_{y,i}$ vektor normal pada point cloud reference dihitung dengan $\frac{-dy}{dx}$
