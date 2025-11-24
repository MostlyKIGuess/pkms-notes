---
{"dg-publish":true,"permalink":"/mobile-robotics/graph-slam-pose-graph-optimization/"}
---

Once the frontend gives us a set of poses and constraints, we have a probability problem.
We have a graph where nodes are poses $x_i$ and edges are measurements $z_{ij}$.

### The Probability Formulation
We want to find the trajectory $X = \{x_0, \dots, x_n\}$ that maximizes the likelihood of the measurements $Z$.
Assuming Gaussian noise, this is equivalent to minimizing the negative log-likelihood, which turns into a [[Mobile-Robotics/NLS\|NLS]] problem.

### The Error Function
An edge $z_{ij}$ represents the measured transformation from pose $x_i$ to $x_j$.
The *expected* measurement based on our current nodes is $h(x_i, x_j) = x_i^{-1} x_j$.

The error (residual) vector $\mathbf{e}_{ij}$ is the difference between the measurement and the expectation.
**Crucial Note:** We are on a manifold. We cannot just subtract matrices. We calculate the error in the tangent space $\mathfrak{se}(3)$ using the Log map.

$$ \mathbf{e}_{ij}(x_i, x_j) = \ln \left( z_{ij}^{-1} (x_i^{-1} x_j) \right)^\vee $$

This gives us a 6x1 error vector for each edge.

### The Cost Function
We minimize the Mahalanobis distance (weighted sum of squares):

$$ F(X) = \sum_{<i,j> \in \mathcal{C}} \mathbf{e}_{ij}^T \Omega_{ij} \mathbf{e}_{ij} $$

Where $\Omega_{ij}$ is the **Information Matrix** (inverse of the covariance matrix $\Sigma_{ij}$). If we trust the odometry less, $\Sigma$ is large, so $\Omega$ is small (low weight).

### Solving the Optimization
This is solved using Gauss-Newton or Levenberg-Marquardt
We linearize the error term around the current guess $x$ by adding a perturbation $\Delta x$.

$$ \mathbf{e}_{ij}(x \boxplus \Delta x) \approx \mathbf{e}_{ij}(x) + J_{ij} \Delta x $$

We solve the normal equation:
$$ H \Delta x = -b $$
$$ (\sum J^T \Omega J) \Delta x = - \sum J^T \Omega \mathbf{e} $$

1.  Build H and b: Iterate over all edges, compute Jacobians, add to the sparse matrix H.
2.  Solve: Use a sparse Cholesky solver (standard) or Preconditioned Conjugate Gradient (for massive maps).
3.  Update: $x_{new} = x_{old} \boxplus \Delta x$.

### Why is this efficient?
The H matrix is *sparse*.
Node $i$ is only connected to Node $i-1$, $i+1$, and maybe a few loop closures. It is not fully connected.
We exploit this sparsity to solve systems with 100,000+ variables in real-time.