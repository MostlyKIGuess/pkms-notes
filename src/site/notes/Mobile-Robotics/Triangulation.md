---
{"dg-publish":true,"permalink":"/mobile-robotics/triangulation/"}
---


In an ideal world, if you cast a ray from Camera 1 through pixel $x_1$ and a ray from Camera 2 through pixel $x_2$, they would intersect perfectly at the 3D point $X$.
In reality due to noise they just never meet.

Using triangulation you just try to minimize the distance between these two rays.
### The Problem Setup

We have two known camera matrices $P_1, P_2$ and two corresponding image points $x_1, x_2$ (homogeneous). We want $X$.

$$ \lambda_1 x_1 = P_1 X $$
$$ \lambda_2 x_2 = P_2 X $$

We need to eliminate the unknown scale factors $\lambda$. We do this using the cross product. Since $x_1$ and $P_1 X$ are parallel (same direction, just scaled), their cross product is zero.

$$ x_1 \times (P_1 X) = 0 $$

### Direct Linear Transformation (DLT)

This is the standard approach. It minimizes the _Algebraic Error_. It’s not geometrically optimal, but it provides a closed-form solution via SVD.

Let $x_1 = [u, v, 1]^T$. Let $p_1^T, p_2^T, p_3^T$ be the rows of $P_1$.
The cross product expands to:

$$
\begin{bmatrix} u \\ v \\ 1 \end{bmatrix} \times \begin{bmatrix} p_1^T X \\ p_2^T X \\ p_3^T X \end{bmatrix} = \begin{bmatrix} v(p_3^T X) - (p_2^T X) \\ (p_1^T X) - u(p_3^T X) \\ u(p_2^T X) - v(p_1^T X) \end{bmatrix} = \mathbf{0}
$$

The third equation is linearly dependent on the first two. So, each view gives us 2 independent linear constraints on $X$.

For View 1:
$$ (x p_3^T - p_1^T) X = 0 $$
$$ (y p_3^T - p_2^T) X = 0 $$

With two views, we stack these to form a system $AX = 0$, where $A$ is a $4 \times 4$ matrix.

$$
A = \begin{bmatrix}
u_1 p_{1,3}^T - p_{1,1}^T \\
v_1 p_{1,3}^T - p_{1,2}^T \\
u_2 p_{2,3}^T - p_{2,1}^T \\
v_2 p_{2,3}^T - p_{2,2}^T
\end{bmatrix}
$$
*(Notation: $p_{k,i}^T$ is the i-th row of the k-th camera matrix)*

#### Solving $AX=0$

We want a non-trivial solution (since $X=0$ is useless). We constrain $||X|| = 1$.
1.  Compute SVD of A: $A = U \Sigma V^T$.
2.  The solution $X$ is the last column of $V$ (corresponding to the smallest singular value).
3.  Normalize: The resulting $X$ is homogeneous $[X, Y, Z, W]^T$. Convert to Euclidean by dividing by $W$.

### Geometric Triangulation (The "Right" Way)

DLT minimizes algebraic error ($Ax$), which means nothing physically.
Ideally, we want to minimize the Reprojection Error.

$$ \min_X ( d(x_1, \hat{x}_1)^2 + d(x_2, \hat{x}_2)^2 ) $$
Where $\hat{x}_i = P_i X$.

Since DLT is fast, we use DLT to get an initial guess, then run [[Mobile-Robotics/NLS\|NLS]] (Gauss-Newton or Levenberg-Marquardt) to refine $X$ to minimize geometric error.

### Code (DLT)


```python
import numpy as np

def triangulate_point(P1, P2, pts1, pts2):
    """
    Triangulates a single 3D point from two 2D correspondences using DLT.
    
    Args:
        P1, P2: 3x4 Projection matrices
        pts1, pts2: 2D points [u, v]
    Returns:
        X: 3D coordinates [x, y, z]
    """
    u1, v1 = pts1
    u2, v2 = pts2

    # Construct A matrix (4x4)
    # Rows from Camera 1
    r1 = u1 * P1[2] - P1[0]
    r2 = v1 * P1[2] - P1[1]
    
    # Rows from Camera 2
    r3 = u2 * P2[2] - P2[0]
    r4 = v2 * P2[2] - P2[1]

    A = np.vstack((r1, r2, r3, r4))

    # Solve AX = 0 using SVD
    U, S, Vt = np.linalg.svd(A)
    X_homogeneous = Vt[-1]

    # Normalize homogeneous coordinates
    X_euclidean = X_homogeneous[:3] / X_homogeneous[3]

    return X_euclidean

if __name__ == "__main__":
    # Sanity Check
    # Camera 1 at Origin looking along Z
    P1 = np.array([[100, 0, 50, 0],
                   [0, 100, 50, 0],
                   [0,   0,  1, 0]])
    
    # Camera 2 shifted by X=10
    P2 = np.array([[100, 0, 50, -1000], # -f * tx -> 100 * 10
                   [0, 100, 50, 0],
                   [0,   0,  1, 0]])
    
    # Point at (0, 0, 10) projects to center (50, 50) in Cam 1
    # In Cam 2 (at X=10), point is relative (-10, 0, 10).
    # u = f * x/z + cx = 100 * (-10/10) + 50 = -100 + 50 = -50
    
    x1 = [50, 50]
    x2 = [-50, 50]
    
    X_est = triangulate_point(P1, P2, x1, x2)
    print(f"Estimated Point: {X_est}") # Should be close to [0, 0, 10]
```