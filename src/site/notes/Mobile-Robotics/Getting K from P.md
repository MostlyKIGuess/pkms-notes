---
{"dg-publish":true,"permalink":"/mobile-robotics/getting-k-from-p/"}
---


From P we can decompose to get K, R, t using RQ Decomposition.


$$
P = K [ R \mid t ].
$$

After solving the DLT system $Ap = 0$ using SVD, we recover **P** (up to scale).

### Step 1: Split P into $[M \mid \mathbf{p}_4]$

Write the $3 \times 4$ projection matrix as:

$$
P =
\begin{bmatrix}
p_{11} & p_{12} & p_{13} & p_{14} \\
p_{21} & p_{22} & p_{23} & p_{24} \\
p_{31} & p_{32} & p_{33} & p_{34}
\end{bmatrix}
=

[ M \mid \mathbf{p}_4 ,]
$$

where:

* $M = P_{:,1:3}$ is the left $3\times3$ block
* $\mathbf{p}_4 = P_{:,4}$ is the last column ( sorry for the python notation 😭)

From the camera model:

$$
P = K [R \mid t] \quad\Rightarrow\quad
M = K R,\qquad
\mathbf{p}_4 = K t.
$$

So if we can factor $M$ into $K$ and $R$, we are done.


###  Step 2: Fix the Scale of P

P is only defined **up to scale**.
Multiply P by any nonzero scalar and it still represents the same camera.

You usually pick a convention like:

* $K_{33} = 1$
* or $p_{34} = 1$
* or normalize $|M|$

Pick one normalization and stick with it. STICK WITH IT.
###  Step 3: RQ Decomposition of M

We want:

$$
M = K R
$$

with:

* $K$ upper triangular
* $R$ orthonormal (rotation matrix)

This is exactly what *RQ decomposition* gives you. ( note: not QR decomposition )

So you compute:

$$
M = K R.
$$

Now you have initial versions of the intrinsic matrix (K) and rotation matrix (R).

###  Step 4: Fix Camera Conventions

Raw RQ output may be awkward. Fix it.

####  1. Make diagonal entries of K positive

If some diagonal $K_{ii}$ is negative:

* multiply column $i$ of $K$ by $-1$
* multiply row $i$ of $R$ by $-1$

This preserves $M = K R$ but cleans up $K$.

####  2. Ensure $\det(R)=+1$

If $\det(R) = -1$:

* flip the sign of one column of $K$ and corresponding row of $R$

Now:

* $K$ has positive focal lengths
* $R$ is a proper rotation

All good.

### Step 5: Recover Translation t

We know:

$$
P = K [R \mid t] = [K R \mid K t] = [M \mid \mathbf{p}_4].
$$

Thus:

$$
\mathbf{p}_4 = K t
\quad\Rightarrow\quad
t = K^{-1} \mathbf{p}_4.
$$

Now you have the translation vector.

### Step 6: (Optional) Compute Camera Center C

The camera center $C$ in world coordinates satisfies:

$$
P \begin{bmatrix} C \ 1 \end{bmatrix} = 0.
$$

Assuming $M$ is invertible:

$$
M C + \mathbf{p}_4 = 0
\quad\Rightarrow\quad
C = -M^{-1} \mathbf{p}_4.
$$

And note:

$$
t = - R C.
$$

This gives a sanity check between $C$ and $t$.



In short:
Given $P$:
1. Split: $P = [M \mid \mathbf{p}_4]$
2. Normalize P (fix scale).
3. RQ-decompose $M = K R$.
4. Fix signs so diagonal of $K$ is positive and $\det(R)=1$.
5. Compute translation:

$$
t = K^{-1} \mathbf{p}_4.
$$

6. (Optional) Compute camera center:

$$
C = -M^{-1} \mathbf{p}_4.
$$



Code:

```python

import numpy as np
from scipy.linalg import rq


def decompose_projection_matrix(P):
    """
    Decomposes P = K [R | t] using RQ decomposition.

    Args:
        P: 3x4 Projection Matrix

    Returns:
        K: 3x3 Intrinsic Matrix
        R: 3x3 Rotation Matrix
        t: 3x1 Translation Vector
        C: 3x1 Camera Center in World Coordinates
    """
    # 1. Split P into M (3x3) and p4 (3x1)
    M = P[:, 0:3]
    p4 = P[:, 3]

    # 2. RQ Decomposition of M
    # scipy.linalg.rq returns K, R such that M = K @ R
    # K is upper triangular, R is orthogonal
    K, R = rq(M)

    # 3. Fix Signs (Enforce positive diagonal for K)
    # The decomposition is not unique. We need K[i,i] > 0.
    # If K[i,i] < 0, we negate the i-th column of K and i-th row of R.
    T = np.diag(np.sign(np.diag(K)))

    K = K @ T
    R = T @ R

    # 4. Enforce det(R) = 1 (Proper Rotation)
    # If det(R) = -1, it's a reflection. Negate the whole matrix?
    # Actually, standard RQ usually handles this, but strictly:
    # If det(R) < 0, we might have a coordinate system flip.
    # Usually scaling P by -1 fixes this if P was homogeneous.

    # Normalize K so K[2,2] = 1
    K = K / K[2, 2]

    # 5. Recover Translation t
    # p4 = K @ t  =>  t = inv(K) @ p4
    t = np.linalg.inv(K) @ p4

    # 6. Recover Camera Center C
    # C = -inv(M) @ p4
    C = -np.linalg.inv(M) @ p4

    return K, R, t, C


if __name__ == "__main__":
    # Ground Truth
    K_true = np.array([[1000, 0, 320], [0, 1000, 240], [0, 0, 1]])
    # Rotation 90 deg around Z
    R_true = np.array([[0, -1, 0], [1, 0, 0], [0, 0, 1]])
    # Translation
    t_true = np.array([10, 20, 5])

    # Construct P
    P_true = K_true @ np.column_stack((R_true, t_true))

    print("Ground Truth P:\n", P_true)

    # Decompose
    K_est, R_est, t_est, C_est = decompose_projection_matrix(P_true)

    print("Estimated Parameters")
    print("K:\n", np.round(K_est, 2))
    print("R:\n", np.round(R_est, 2))
    print("t:", np.round(t_est, 2))
    print("Camera Center C:", np.round(C_est, 2))

    # Sanity Check: Does K[R|t] reconstruct P?
    P_reconst = K_est @ np.column_stack((R_est, t_est))
    print("\nReconstructed P:\n", np.round(P_reconst, 2))

```
