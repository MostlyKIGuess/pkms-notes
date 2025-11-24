---
{"dg-publish":true,"permalink":"/mobile-robotics/icp-slam/"}
---

# ICP-SLAM

AI AI CAPTAIN. ( so pls be alert, altho I have edited a lot )

We know ICP aligns two scans. But if you just chain pairwise ICPs ($T_{1}^{0}, T_{2}^{1}, \dots$), you are doing Odometry. Errors accumulate. The transforms drift. The map warps 

ICP-SLAM is the grown-up version. It treats the trajectory and the map as a *joint optimization problem*. We don't just trust the previous frame; we minimize the error across *all* observed frames and *all* points simultaneously.

### 1. The Problem Statement

We have:
1.  Poses: $m$ frames with transforms $\hat{T}_{0}^{i}$ (Base to Frame $i$).
2.  Points: $n$ landmarks, observed as $X_{(j)}^0$ (in Base frame).

If we knew the exact poses, we could just accumulate points (Mapping). If we knew the exact map, we could just locate the robot (Localization).
In SLAM, we know neither. We estimate both.

Variables to Optimize:
* $m$ Poses ($\hat{T}_{0}^{i}$) $\in SE(3)$
* $n$ Points ($\hat{X}_{(j)}^{0}$) $\in \mathbb{R}^3$

Total variables: $12m + 3n$ (naive) $\to$ $6m + 3n$ (using [[Mobile-Robotics/Lie-Algebra\|Lie-Algebra]] ).

### 2. The Objective Function

We minimize the Multi-View Reprojection Error*.
The residual $\vec{r}_{(j)}^{i}$ is the difference between where we *saw* point $j$ in frame $i$ and where our current estimate says it should be.

$$ \vec{r}_{(j)}^{i} = \vec{X}_{(j)}^{i} - \hat{T}_{0}^{i} \cdot \hat{X}_{(j)}^{0} $$

The total cost function (Loss) is the sum of squared residuals:

$$ \mathcal{L} = \sum_{i=1}^{m} \sum_{j=1}^{n} || \vec{r}_{(j)}^{i} ||_2^2 $$

To solve this non-linear least squares problem, we use *Levenberg-Marquardt (LM)*.
The update rule is:
$$ (J^T J + \lambda I) \vec{\delta} = J^T \vec{r} $$

But wait. $\hat{T}$ is a matrix. **You cannot just add $\Delta T$ to $T$.**

### 3. The Manifold: Why Addition Fails

[[Mobile-Robotics/Lie-Algebra\|Lie-Algebra]]

Solution: We optimize in the *angent Space $\mathfrak{se}(3)$ (Lie Algebra), which is a flat vector space $\mathbb{R}^6$ .

#### The Maps 
1.  Logarithmic Map ($\log^\vee$): Group $\to$ Algebra.
    * Takes a curved $T$ and gives us a straight vector $\xi \in \mathbb{R}^6$.
    * $\xi = [\omega, v]^T$ (Rotation axis-angle, Translation).
2. Exponential Map ($\exp^\wedge$):Algebra $\to$ Group.
    * Takes a straight vector $\delta \xi$ and maps it back to a valid rotation/translation matrix.
    * $T = \exp(\hat{\xi})$.

### 4. The Jacobians 

We linearize the residual $\vec{r}$ with respect to the perturbation vector $\vec{\delta}$.
The update vector contains both pose perturbations and point perturbations:
$$ \vec{\delta} = [\delta \xi_1, \dots, \delta \xi_m, \delta X_1, \dots, \delta X_n]^T $$

The Jacobian $J$ splits into two blocks: *Localization*($J_L$) and *Mapping*($J_M$).

#### A. Localization Jacobian ($J_L$) 
Derivative of residual w.r.t. Pose perturbation $\delta \xi_i$.
Let $P = \hat{T}_{0}^{i} \hat{X}_{(j)}^{0}$ be the point in the current frame.

$$ J_{(j)L}^{i} = \frac{\partial \vec{r}}{\partial \delta \xi} = \left[ I_{3} \mid -[P]_{\times} \right] $$

* $I_3$: Identity (3x3). Derivative w.r.t translation.
* $-[P]_\times$: Negative Skew-symmetric matrix of the transformed point. Derivative w.r.t rotation.
* Dimension: $3 \times 6$.

#### B. Mapping Jacobian ($J_M$) 
Derivative of residual w.r.t. Map Point perturbation $\delta X_j$.

$$ J_{(j)M}^{i} = \frac{\partial \vec{r}}{\partial \hat{X}^0} = \hat{R}_{0}^{i} $$

* $\hat{R}_{0}^{i}$: The rotation matrix from Base to Frame $i$.
* Dimension: $3 \times 3$.

We exploit this sparsity to solve the system efficiently.
### 5. Sparsity Structure (The Arrowhead)


The full Jacobian $J$ is massive ($3mn \times (6m+3n)$), but mostly empty.
* Residual $r_{ij}$ depends only on Pose $i$ and Point $j$.
* It does not care about Pose $k$ or Point $l$.

Structure:
* Top Left ($J_L$): Block diagonal (mostly).
* Top Right ($J_M$): Block diagonal (mostly).

We exploit this sparsity to solve the system efficiently.
### 6. The Update Step 

We solve the normal equations $(J^T J + \lambda I) \vec{\delta} = J^T \vec{r}$ to get $\vec{\delta}$.
Then we apply the updates. Pay attention here.

#### Pose Update (Manifold)
We use the exponential map to apply the 6D twist perturbation to the matrix.
$$ \hat{T}_{0}^{i} \leftarrow \hat{T}_{0}^{i} \cdot \exp(\hat{\delta \xi}_i) $$
*(Right multiplication ensures we update in the local tangent space)*.
#### Point Update (Euclidean)
Points are just vectors. We can add them.
$$ \hat{X}_{(j)}^{0} \leftarrow \hat{X}_{(j)}^{0} + \delta \hat{X}_{(j)}^{0} $$

### Summary 

1.  Initialize: Guess poses (from Odometry/ICP) and points (from Depth).
2.  Compute Residuals: Error between Observation and Projection.
3.  Compute Jacobians: $J_L$ (Poses) and $J_M$ (Points).
4.  Build System: Construct $H = J^T J + \lambda I$ and $b = J^T r$.
5.  Solve: Find $\delta$.
6.  Update:
    * $T \leftarrow T \cdot \exp(\delta \xi)$
    * $X \leftarrow X + \delta X$
7.  Iterate: Until convergence ($\delta \to 0$).

This turns a drifting odometry path into a globally consistent map.