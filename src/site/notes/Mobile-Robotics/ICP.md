---
{"dg-publish":true,"permalink":"/mobile-robotics/icp/"}
---

# Iterative Closest Point (ICP) Algorithm

### Point Cloud Registration

Point cloud registration is the process of aligning two or more 3D point clouds of the same scene that have been captured from different viewpoints. Given a *source* point cloud, which we can call $P_{s'}$, and a *target* point cloud, $P_s$, the goal is to find the optimal rigid body transformation that best aligns $P_{s'}$ with $P_s$. This transformation consists of a rotation matrix, $R$, and a translation vector, $t$.

### The ICP Algorithm

We don't know which point in the source cloud corresponds to which point in the target cloud. ICP algorithm provides an elegant solution to this by a simple heuristic: it assumes that, for a given point in the source cloud, its *nearest neighbor* in the target cloud is its correct correspondence. While this assumption is not always true, especially at the beginning, it becomes increasingly accurate as the clouds get closer to alignment.

The algorithm is an iterative process that can be broken down into four steps:

1.  *Initial Guess*: The process begins with an initial estimate for the transformation, $\{R, t\}$. This initial guess can come from other sensors, like an Inertial Measurement Unit (IMU), or from the result of the previous alignment between a different pair of scans. A good initial guess is very crucial for convergence.
2.  *Data Association*: For each point in the source cloud, its nearest point in the target cloud is found. This establishes a set of tentative correspondences.
3.  *Optimize Transformation*: With these correspondences established, the algorithm then computes the optimal transformation $\{R, t\}$ that minimizes a cost function, which typically is the sum of squared distances between these associated pairs.
4.  *Iterate*: The newly computed transformation is applied to the source point cloud. Steps 2 and 3 are then repeated with this updated point cloud. This loop continues until the algorithm converges, which is usually checked by checking if the change in the error between iterations falls below decided threshold.

### The Optimization Step: A Closed-Form Solution

For a *given* set of point correspondences, step 3 of the ICP algorithm has a closed-form analytical solution. This method, often called the *[Kabsch algorithm](https://hunterheidenreich.com/posts/kabsch-algorithm/)*, finds the optimal rotation and translation by minimizing the following least-squares cost function:

$$ \min_{R, t} \sum_{j=1}^{n} \| R p_j + t - q_j \|^2 $$

Here, $p_j$ represents the points in the source cloud and $q_j$ represents their corresponding points in the target cloud. The solution is in two parts:

1.  *Solving for Translation*: First, the optimal translation is found by recognizing that it must align the centroids (or geometric means) of the two point sets.
   
 Derivation for optimal translation, $t$

 We start with the cost function, $\mathcal{L}(R, t)$:
 $$ \mathcal{L}(R, t) = \sum_{j=1}^{n} \| R p_j + t - q_j \|^2 = \sum_{j=1}^{n} (R p_j + t - q_j)^T (R p_j + t - q_j) $$
 Find minimum wrt $t$, we take the partial derivative of $\mathcal{L}$ with respect to $t$ and set it to zero:
 $$ \frac{\partial \mathcal{L}}{\partial t} = \sum_{j=1}^{n} 2(R p_j + t - q_j) = 0 $$
 We can then solve for $t$:
 $$ \sum_{j=1}^{n} R p_j + \sum_{j=1}^{n} t - \sum_{j=1}^{n} q_j = 0 $$
 $$ R \left(\sum_{j=1}^{n} p_j\right) + n t - \sum_{j=1}^{n} q_j = 0 $$
 $$ n t = \sum_{j=1}^{n} q_j - R \left(\sum_{j=1}^{n} p_j\right) $$
Dividing by $n$ gives the optimal translation in terms of the centroids, $\mu_p$ and $\mu_q$:
 $$ t = \frac{1}{n}\sum_{j=1}^{n} q_j - R \left(\frac{1}{n}\sum_{j=1}^{n} p_j\right) = \mu_q - R \mu_p $$
   
   The optimal translation, $t$, is given by:
   $$ t = \mu_q - R \mu_p $$
   where $\mu_p$ and $\mu_q$ are the centroids of the source and target point clouds, respectively.
   
   This is also pretty intuitive as $u_q$ being points from target point cloud, when we would have a perfect ICP we would ideally rotate the perfect amount for source point cloud making the difference only the translation, also when rotation is identity , the translation would simply be the difference between the centroids of the two point clouds. 

2.  *Solving for Rotation*: By substituting this expression for $t$ back into the original cost function, the problem simplifies to finding the optimal rotation for the *centered* point clouds.( _Think how PCA works here , PC1 is calculated by first centering the data, then finding the direction of maximum variance, just a random thought can be ignored for reading the proof_)
   
   Derivation for optimal rotation, $R$
  
 Substitute $t = \mu_q - R \mu_p$ into the cost function:
  $$ \mathcal{L}(R) = \sum_{j=1}^{n} \| R p_j + (\mu_q - R \mu_p) - q_j \|^2 = \sum_{j=1}^{n} \| R(p_j - \mu_p) - (q_j - \mu_q) \|^2 $$
Let $p'_j = p_j - \mu_p$ and $q'_j = q_j - \mu_q$ be the centered points. The problem is now to find the rotation that minimizes the sum of squared distances between these centered points:
  $$ \min_{R} \sum_{j=1}^{n} \| R p'_j - q'_j \|^2 $$
  Expanding the squared norm:
 $$ \sum \| R p'_j \|^2 - 2(R p'_j)^T q'_j + \|q'_j \|^2 $$
 Since rotation preserves length, $\| R p'_j \|^2 = \| p'_j \|^2$. The expression becomes:
 $$ \sum \| p'_j \|^2 - 2 \sum (R p'_j)^T q'_j + \sum \|q'_j \|^2 $$
The terms $\sum \| p'_j \|^2$ and $\sum \|q'_j \|^2$ are constant with respect to $R$. Therefore, minimizing the entire expression is equivalent to *maximizing* the term $\sum (R p'_j)^T q'_j$. This sum of dot products can be written using the trace operator:
 $$ \max_{R} \sum q_j'^T R p'_j = \max_{R} \text{Tr}(Q'^T R P') $$
where $P'$ and $Q'$ are $3 \times n$ matrices whose columns are the centered points. Using the cyclic property of the trace, $Tr(A B C) = Tr(C A B)$, we can rewrite this as:
 $$ \max_{R} \text{Tr}(R P' Q'^T) $$
 The term $P' Q'^T$ is the transpose of the covariance matrix $W$ we defined earlier. So the problem is to find the rotation $R$ that maximizes $\text{Tr}(R W^T)$. It is a standard result from linear algebra (the Orthogonal Procrustes problem) that this trace is maximized when $R = V U^T$, where $U$ and $V$ come from the SVD of $W = U S V^T$.
   
   This is solved using Singular Value Decomposition (SVD). We first construct a $3 \times 3$ covariance matrix, $W$:
   $$ W = \sum_{j=1}^{n} (p_j - \mu_{p})(q_j - \mu_{q})^T $$
   Next, we compute the SVD of this matrix:
   $$ W = U S V^T $$
   The optimal rotation matrix, $R$, is then given by:
   $$ R = V U^T $$
   It is important to ensure that the resulting matrix is a proper rotation and not a reflection, which can happen in noisy or degenerate cases. This is checked by ensuring its determinant is +1. A correction can be applied if necessary:
   $$ R_{corrected} = V \begin{pmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & \det(V U^T) \end{pmatrix} U^T $$


### Wrapping Up

At this point, we’ve got everything we need ladies and gentleman. 
The ICP loop boils down to finding correspondences, then solving for the rigid transform using the closed-form Kabsch solution:

$$
R = V U^T, \qquad 
t = \mu_q - R \mu_p
$$

where $U, S, V$ come from the SVD of the covariance matrix $W = \sum (p_j - \mu_p)(q_j - \mu_q)^T$. 


## Applications

fir kabhi 🙏🏻🙏🏻🙏🏻🙏🏻
## ICP-SLAM

[[Mobile-Robotics/ICP-SLAM\|ICP-SLAM]]

**