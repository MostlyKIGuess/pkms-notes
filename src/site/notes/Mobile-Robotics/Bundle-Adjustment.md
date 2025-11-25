---
{"dg-publish":true,"permalink":"/mobile-robotics/bundle-adjustment/"}
---

MVG Chapter 18: N-View Computational Methods, Check that for deeper analysis.



If Kalman Filters are the "quick and dirty" recursive solution, Bundle Adjustment (BA) is the "slow and perfect" batch solution.
It is _the_ de facto standard for offline SLAM and 3D reconstruction.

Definition: Simultaneous refinement of the 3D coordinates of scene geometry ($X_j$) and the parameters of the relative motion ($R_i, t_i$) and optical characteristics ($K$) to minimize the reprojection error.

### 1. The Visual Objective Function
Unlike ICP-SLAM (which minimizes 3D-3D distance), Visual BA minimizes 2D-3D Reprojection Error.

$$ \min_{T_i, X_j} \sum_{i=1}^{m} \sum_{j=1}^{n} v_{ij} \left\| \mathbf{z}_{ij} - \pi(T_i, X_j) \right\|_{\Sigma}^2 $$

* $\mathbf{z}_{ij}$: Observed pixel coordinates $(u, v)$ of point $j$ in frame $i$.
* $\pi(\cdot)$: The Camera Projection function (World $\to$ Camera $\to$ Image).
* $T_i$: Camera Pose ($R, t$).
* $X_j$: 3D Landmark position.

### 2. The Projection Model $\pi$
This is where it differs from ICP. We have to project 3D points to the image plane.

1.  Transform: $P' = R X_w + t = [X', Y', Z']^T$
2.  Normalize: $p_n = [X'/Z', Y'/Z']^T$ (Perspective Division)
3.  Pixelate: $\begin{bmatrix} u \\ v \end{bmatrix} = \begin{bmatrix} f_x & 0 & c_x \\ 0 & f_y & c_y \end{bmatrix} \begin{bmatrix} X'/Z' \\ Y'/Z' \\ 1 \end{bmatrix}$

### 3. The Jacobian (The Chain Rule)
The residual is $\mathbf{r} = \mathbf{z} - \pi(T, X)$. We need $\frac{\partial \mathbf{r}}{\partial \delta \xi}$.
Using the Chain Rule:

$$ J_{pose} = \frac{\partial \mathbf{r}}{\partial P'} \cdot \frac{\partial P'}{\partial \delta \xi} $$

Part A: The Projection Derivative ($\partial \mathbf{r} / \partial P'$)**
How pixel $u,v$ changes when the camera-frame point $X',Y',Z'$ moves.
$$ \frac{\partial \pi}{\partial P'} = \begin{bmatrix} f_x/Z' & 0 & -f_x X' / (Z')^2 \\ 0 & f_y/Z' & -f_y Y' / (Z')^2 \end{bmatrix} $$
*Note the $1/Z^2$ term. This is why points close to the camera cause massive gradients (instability).*

Part B: The Manifold Derivative ($\partial P' / \partial \delta \xi$)
Same as ICP-SLAM.
$$ \frac{\partial P'}{\partial \delta \xi} = [I_3 \mid -[P']_\times] $$

Multiply A and B to get the $2 \times 6$ Jacobian for one observation.

### 4. The Schur Complement (The Computational Trick)
We have a system $H \delta = b$.
$H$ is size $(6m + 3n)^2$. For 1000 points and 100 cameras, $H$ is $3600 \times 3600$. Inverting this is $O(N^3)$. Too slow.

**The Structure:**
$$ H = \begin{bmatrix} B & E \\ E^T & C \end{bmatrix} $$
* $B$: Camera-Camera block (Sparse).
* $C$: Point-Point block (Diagonal! Because points don't see each other).
* $E$: Camera-Point interaction.

Since $C$ is diagonal, it is trivial to invert. We can "marginalize out" the points to solve for cameras first:

1.  Solve for Cameras (Reduced Camera System):
$$ (B - E C^{-1} E^T) \delta_{\text{camera}} = b_{\text{camera}} - E C^{-1} b_{\text{point}} $$
The matrix $S = B - E C^{-1} E^T$ is the Schur Complement. It is much smaller ($6m \times 6m$).

2. Back-Substitute for Points:
$$ \delta_{\text{point}} = C^{-1} (b_{\text{point}} - E^T \delta_{\text{camera}}) $$

This reduces complexity from cubic in points to linear in points. This is why we can run BA on thousands of landmarks.