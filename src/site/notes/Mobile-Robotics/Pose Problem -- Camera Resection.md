---
{"dg-publish":true,"permalink":"/mobile-robotics/pose-problem-camera-resection/"}
---


Define the Problem:
Given K and set of N 3D world points to 2D pixel correspondences, find R and t.


### Reprojection Error:

The optimal pose is found my minimizing reprojection error. It is the error between "observed" pixel coordinates and "predicted" pixel coordinates obtained by projecting the 3D points using the estimated pose. 


$$ \hat{x}_{i} = \begin{bmatrix} \hat{u}_{i} \\ \hat{v}_{i} \end{bmatrix} = \begin{bmatrix} \frac{f_x X^{C}_{i}}{Z^{C}_{i}} + c_x \\ \frac{f_y Y^{C}_{i}}{Z^{C}_{i}} + c_y \end{bmatrix} $$

where $X^{C}_{i}, Y^{C}_{i}, Z^{C}_{i}$ are the coordinates of the i-th 3D point in the camera frame obtained by applying the estimated R and t.


#### Cost Function:
The cost function to minimize is the sum of squared reprojection errors over all N points:
$$ J(R, t) = \sum_{i=1}^{N} \left\| x_{i} - \hat{x}_{i} \right\|^2 $$

where $x_{i} = \begin{bmatrix} u_{i} \\ v_{i} \end{bmatrix}$ are the observed pixel coordinates.
Here is the markdown continuing from your notes, followed by the Python implementation for the exam.

### The Optimization Problem (Known Correspondences)

This is [[Mobile-Robotics/NLS\|NLS]] problem.

We linearize the residuals using a first-order Taylor expansion around the current estimate.

Let the update be a small perturbation in the Lie Algebra $\mathfrak{se}(3)$:
$$\delta \xi = [\delta \phi^T, \delta t^T]^T$$
where $\delta \phi$ is the rotational update (twist) and $\delta t$ is the translational update.

The update rule is:
$$T_{new} = T_{old} \cdot \exp(\delta \xi^\wedge)$$

The Jacobian of the residual $r_i = x_i - \hat{x}_i$ with respect to the perturbation $\delta \xi$ is a $2 \times 6$ matrix for each point:

$$J_i = \frac{\partial r_i}{\partial \delta \xi} = \begin{bmatrix} \frac{\partial u}{\partial \delta \xi} \\ \frac{\partial v}{\partial \delta \xi} \end{bmatrix}$$

#### The Interaction Matrix (Jacobian)

Using the chain rule:
$$\frac{\partial \hat{x}}{\partial \delta \xi} = \frac{\partial \hat{x}}{\partial X^C} \frac{\partial X^C}{\partial \delta \xi}$$

1.  Projection Derivative ($\frac{\partial \hat{x}}{\partial X^C}$): How pixel coordinates change with camera coordinates.
$$\frac{\partial \hat{x}}{\partial X^C} = \begin{bmatrix} \frac{f_x}{Z} & 0 & -\frac{f_x X}{Z^2} \\ 0 & \frac{f_y}{Z} & -\frac{f_y Y}{Z^2} \end{bmatrix}$$

2.  Pose Derivative ($\frac{\partial X^C}{\partial \delta \xi}$): How camera coordinates change with pose perturbation.
$$\frac{\partial X^C}{\partial \delta \xi} = [I_{3 \times 3} \mid -[X^C]_\times]$$
$$\frac{\partial X^C}{\partial \delta \xi} = \begin{bmatrix} 1 & 0 & 0 & 0 & Z & -Y \\ 0 & 1 & 0 & -Z & 0 & X \\ 0 & 0 & 1 & Y & -X & 0 \end{bmatrix}$$

Multiplying these gives the full $2 \times 6$ Jacobian for one point:

$$J_i = - \begin{bmatrix} \frac{f_x}{Z} & 0 & -\frac{f_x X}{Z^2} & -\frac{f_x X Y}{Z^2} & f_x(1 + \frac{X^2}{Z^2}) & -\frac{f_x Y}{Z} \\ 0 & \frac{f_y}{Z} & -\frac{f_y Y}{Z^2} & -f_y(1 + \frac{Y^2}{Z^2}) & \frac{f_y X Y}{Z^2} & \frac{f_y X}{Z} \end{bmatrix}$$

*(Note: The negative sign comes from $r = \text{obs} - \text{pred}$. be careful if you are doing pred-obs!)*

### The Algorithm (Gauss-Newton)

1.  Initialize $R, t$ (e.g., using P3P or identity).
2.  Iterate:
    a.  Project 3D points using current $R, t$ to get $\hat{x}_i$.
    b.  Compute residuals $r_i = x_i - \hat{x}_i$.
    c.  Compute Jacobian $J_i$ for each point.
    d.  Stack $J$ (matrix $2N \times 6$) and $r$ (vector $2N \times 1$).
    e.  Solve normal equations: $(J^T J) \delta \xi = J^T r$.
    f.  Update pose: $T \leftarrow T \cdot \exp(\delta \xi^\wedge)$.
    g.  Check convergence.

### Code: Pose Estimation (PnP)

Use `cv2.solvePnP`. It implements exactly the algorithm above (uses Levenberg-Marquardt).

```python
import cv2
import numpy as np


def solve_pnp_example():
# Camera Matrix (K)
# fx=800, fy=800, cx=320, cy=240
K = np.array([[800, 0, 320], [0, 800, 240], [0, 0, 1]], dtype=np.float32)

# Distortion Coefficients (Assuming zero for simplicity, but PnP handles them)
dist_coeffs = np.zeros((4, 1))

# 2. DATA (3D Points and 2D Observations)
# Define a 3D object (e.g., a rectangle in space)
# Points in Object Frame (X, Y, Z)
object_points = np.array(
	[[0, 0, 0], [1, 0, 0], [1, 1, 0], [0, 1, 0]], dtype=np.float32
)

# Define the true pose (to generate synthetic 2D points)
# Rotate 45 deg around Y, Move to Z=10
rvec_true = np.array([0, np.pi / 4, 0], dtype=np.float32)
tvec_true = np.array([0, 0, 5], dtype=np.float32)

# Project points to create "Observations"
image_points, _ = cv2.projectPoints(
	object_points, rvec_true, tvec_true, K, dist_coeffs
)

# Add a little noise to simulate real world
# Doing the bottom causes Error as image_points is of float32
# image_points = image_points + np.random.normal(0, 0.5, image_points.shape)
noise = np.random.normal(0, 0.5, image_points.shape).astype(np.float32)
image_points = image_points + noise

print("Observed Image Points:\n", image_points.reshape(-1, 2))

# 3. SOLVE PNP
# Method: cv2.SOLVEPNP_ITERATIVE (Levenberg-Marquardt optimization)
success, rvec_est, tvec_est = cv2.solvePnP(
	object_points, image_points, K, dist_coeffs, flags=cv2.SOLVEPNP_ITERATIVE
)

if not success:
	print("PnP solution failed.")
	return

print("Estimated Pose ( R and t ) ")
print("Rotation Vector (Rodrigues):\n", rvec_est.flatten())
print("Translation Vector:\n", tvec_est.flatten())

# 4. VERIFICATION
# Calculate Reprojection Error
projected_points, _ = cv2.projectPoints(
	object_points, rvec_est, tvec_est, K, dist_coeffs
)
error = cv2.norm(image_points, projected_points, cv2.NORM_L2) / len(
	projected_points
)

print(f"\nReprojection Error (RMS per point): {error:.4f} pixels")

# Convert Rotation Vector to Matrix
R_est, _ = cv2.Rodrigues(rvec_est)
print("\nRotation Matrix:\n", np.round(R_est, 3))

# Print Ground Truth
print("\nGround Truth Pose ( R and t ) ")
print("Rotation Vector (Rodrigues):\n", rvec_true.flatten())
print("Translation Vector:\n", tvec_true.flatten())
# Rotation Matrix
R_true = cv2.Rodrigues(rvec_true)[0]
print("Rotation Matrix:\n", np.round(R_true, 3))

# Verify Rotation Matrix
print("\nVerification of Rotation Matrix:")
print("Estimated Rotation Matrix:\n", np.round(R_est, 3))
print("Ground Truth Rotation Matrix:\n", np.round(R_true, 3))


if __name__ == "__main__":
solve_pnp_example()
```

