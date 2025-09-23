---
{"dg-publish":true,"permalink":"/mobile-robotics/mr-que-ans-compilation/"}
---


# Mobile Robotics Mid-Semester Q&A Compilation

NOTE: AI AI CAPTAIN

### Q1: Warm-up Table

#### Question
*Fill up the following table by indicating the quantities that are known, to be estimated, or unknown, and the type of measurements that are needed.*
#### Solution Table

| Problem | Structure (Scene Geometry) | Motion (Camera Parameters) | Measurements |
| :--- | :--- | :--- | :--- |
| *F-matrix estimation* | *Unknown* | *To be Estimated* | *2D-2D features* |
| *Camera calibration* | *Known* | *To be Estimated* | *2D-3D features* |
| *Triangulation* | *To be Estimated* | *Known* | *2D-2D features* |
| *Stereo rectification* | *Unknown* | *Known & To be Estimated* | *Unknown* |
| *PnP (Perspective-n-Point)*| *Known* | *To be Estimated* | *2D-3D features* |
| *Bundle adjustment* | *To be Estimated* | *To be Estimated* | *2D-3D features* |

### Q2: Transformations

#### Question (i)

*Derive the expression for $T_{W}^{C}$ if $T_{C}^{W} = \begin{bmatrix} R_{C}^{W} & P_{CORG}^{W} \\ 0_{1 \times 3} & 1 \end{bmatrix}$.*

#### Solution (i)

We know that the transformation from a frame to itself is the identity matrix. Therefore, the product of a transform and its inverse must be the identity matrix:
$$ T_{C}^{W} T_{W}^{C} = I $$
Expanding this with the block matrix form gives:
$$ \begin{bmatrix} R_{C}^{W} & P_{CORG}^{W} \\ 0 & 1 \end{bmatrix} \begin{bmatrix} R_{W}^{C} & P_{WORG}^{C} \\ 0 & 1 \end{bmatrix} = \begin{bmatrix} I & 0 \\ 0 & 1 \end{bmatrix} $$
From the block matrix multiplication, we get two key equations:
1.  For the rotation part: $R_{C}^{W} R_{W}^{C} = I$. This implies that $R_{W}^{C} = (R_{C}^{W})^{-1}$. Since rotation matrices are orthogonal, the inverse is equal to the transpose, so $R_{W}^{C} = (R_{C}^{W})^T$.
2.  For the translation part: $R_{C}^{W} P_{WORG}^{C} + P_{CORG}^{W} = 0$.

Solving the second equation for the unknown translation vector $P_{WORG}^{C}$:
$$ P_{WORG}^{C} = -(R_{C}^{W})^{-1} P_{CORG}^{W} = -(R_{C}^{W})^T P_{CORG}^{W} $$
Combining these results, the final expression for the inverse transformation is:
$$ T_{W}^{C} = \begin{bmatrix} (R_{C}^{W})^T & -(R_{C}^{W})^T P_{CORG}^{W} \\ 0 & 1 \end{bmatrix} $$

---
#### Question (ii)

*Consider the figure below, where {W} is the world frame and {C} is the camera frame. The Z-axis of {W} comes out of the plane, while the Y-axis of {C} goes into the plane. Find (a) $R_{C}^{W}$, (b) the YXZ-Euler angles for $R_{C}^{W}$, and (c) $P_{WORG}^{C}$ and $T_{W}^{C}$.*



#### Solution (ii)

**(a) Find the Rotation Matrix $R_{C}^{W}$**

To find the rotation matrix that describes the orientation of frame {C} with respect to frame {W}, we express the unit axes of {C} in {W}'s coordinates.
From the diagram:
* The X-axis of {C} ($X_C$) points along the Y-axis of {W} ($Y_W$). So, $x_c^w = [0, 1, 0]^T$.
* The Y-axis of {C} ($Y_C$) points into the plane, which is along the negative Z-axis of {W} ($-Z_W$). So, $y_c^w = [0, 0, -1]^T$.
* The Z-axis of {C} ($Z_C$) points along the negative X-axis of {W} ($-X_W$). So, $z_c^w = [-1, 0, 0]^T$.

Stacking these as column vectors gives the rotation matrix:
$$ R_{C}^{W} = \begin{bmatrix} 0 & 0 & -1 \\ 1 & 0 & 0 \\ 0 & -1 & 0 \end{bmatrix} $$

**(b) Find the YXZ-Euler Angles**

The YXZ-Euler angle representation corresponds to the matrix product $R_Y(\alpha) R_X(\beta) R_Z(\gamma)$. We need to find angles $\alpha, \beta, \gamma$ that produce the $R_{C}^{W}$ matrix. One possible solution is a rotation of $-90^{\circ}$ around the Y-axis, followed by $0^{\circ}$ around the new X-axis, and then $90^{\circ}$ around the new Z-axis.
$$ R_{C}^{W} = R_Y(-90^{\circ}) R_X(0^{\circ}) R_Z(90^{\circ}) $$
$$ = \begin{bmatrix} 0 & 0 & -1 \\ 0 & 1 & 0 \\ 1 & 0 & 0 \end{bmatrix} \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} 0 & -1 & 0 \\ 1 & 0 & 0 \\ 0 & 0 & 1 \end{bmatrix} = \begin{bmatrix} 0 & 0 & -1 \\ 1 & 0 & 0 \\ 0 & -1 & 0 \end{bmatrix} $$
So, the YXZ-Euler angles are $(\alpha, \beta, \gamma) = (-90^{\circ}, 0^{\circ}, 90^{\circ})$. *Note: Euler angle representations are not unique*.

**( c ) Find $P_{WORG}^{C}$ and $T_{W}^{C}$**

The origin of frame {C} is located at $(1, 5, -2)$ in the world frame {W}, so $P_{CORG}^{W} = [1, 5, -2]^T$. We want to find the position of the world origin as seen from the camera frame, $P_{WORG}^{C}$. Using the formula from part (i):
$$ P_{WORG}^{C} = -(R_{C}^{W})^T P_{CORG}^{W} = - \begin{bmatrix} 0 & 1 & 0 \\ 0 & 0 & -1 \\ -1 & 0 & 0 \end{bmatrix} \begin{bmatrix} 1 \\ 5 \\ -2 \end{bmatrix} = - \begin{bmatrix} 5 \\ 2 \\ -1 \end{bmatrix} = \begin{bmatrix} -5 \\ -2 \\ 1 \end{bmatrix} $$
Finally, we construct the full transformation matrix $T_{W}^{C}$:
$$ T_{W}^{C} = \begin{bmatrix} R_{W}^{C} & P_{WORG}^{C} \\ 0 & 1 \end{bmatrix} = \begin{bmatrix} (R_{C}^{W})^T & P_{WORG}^{C} \\ 0 & 1 \end{bmatrix} = \begin{bmatrix} 0 & 1 & 0 & -5 \\ 0 & 0 & -1 & -2 \\ -1 & 0 & 0 & 1 \\ 0 & 0 & 0 & 1 \end{bmatrix} $$

### Q3: Single-View Geometry and Reconstruction

#### Question 3.1

*Given a camera matrix P, detail how you can obtain the camera center C and the rotation matrix R without knowing the intrinsic parameter matrix K.*
#### Solution 3.1

The camera matrix P is a $3 \times 4$ matrix that can be decomposed as $P = K[R|t]$, where K is the $3 \times 3$ intrinsic matrix, R is the $3 \times 3$ rotation matrix, and t is the $3 \times 1$ translation vector. This can be written as $P = [KR | Kt]$.

1.  *Finding the Camera Center*: The camera center C is the point in 3D space that projects to the null-space of P. That is, $PC = 0$. Since P is $3 \times 4$, its null-space is one-dimensional, so C is uniquely determined up to a scale factor.

2.  *Finding the Rotation Matrix*: Without knowing K, we cannot directly recover R. However, we can use the fact that the first $3 \times 3$ submatrix of P, let's call it $M = KR$, is the product of an upper-triangular matrix (K) and an orthogonal matrix (R). We can use a *QR decomposition* on $M^{-1}$. If $M^{-1} = R_{qr} K_{qr}$, where $R_{qr}$ is orthogonal and $K_{qr}$ is upper-triangular, then $M = K_{qr}^{-1} R_{qr}^{-1}$. From this, we can identify $R = R_{qr}^{-1}$ and $K = K_{qr}^{-1}$.

Alternatively, we can use the property that $R R^T = I$.
$$ M M^T = (KR)(KR)^T = K R R^T K^T = K K^T $$
This equation relates the known matrix $M M^T$ to the unknown intrinsic matrix K. Since $K K^T$ is symmetric, we can solve for K (up to the sign ambiguities inherent in Cholesky decomposition). Once K is found, R can be recovered simply as $R = K^{-1}M$.

#### Question 3.2

*State and justify the cases when the 3D reconstruction obtained from two views is (a) Unambiguous (b) Up to an unknown scaling factor (c) Up to an unknown projective transformation.*

#### Solution 3.2

**(a) Unambiguous (Euclidean) Reconstruction:**
Reconstruction is unambiguous (up to a global rigid motion) when the intrinsic matrices of both cameras ($K_1, K_2$) and the relative extrinsic parameters (rotation R, translation t) between them are all known. With this information, the camera matrices $P_1 = K_1[I|0]$ and $P_2 = K_2[R|t]$ are fully defined. Corresponding image points can be back-projected into 3D rays, and their intersection gives the unique 3D point.

**(b) Up to an Unknown Scaling Factor (Metric Reconstruction):**
Reconstruction is determined up to a scale factor when the intrinsic matrices ($K_1, K_2$) are known, but the relative motion (R, t) is unknown. In this case, we can estimate the *Essential Matrix (E)* from 2D point correspondences. The Essential Matrix can be decomposed to find R and t, but the translation vector t can only be recovered up to a scale. Since the magnitude of the translation (the baseline) is unknown, the absolute scale of the reconstructed 3D scene is also unknown.

**( c ) Up to an Unknown Projective Transformation:**
Reconstruction is only determined up to a projective transformation when the intrinsic ($K_1, K_2$) and extrinsic (R, t) parameters are all unknown. With only 2D point correspondences, we can only estimate the *Fundamental Matrix (F)*. The Fundamental Matrix does not encode the full Euclidean geometry of the scene. Since F is determined only by image point correspondences, there is a projective ambiguity in the reconstruction; any non-singular $4 \times 4$ matrix H applied to the scene points and camera matrices will result in the same image points ($x = PX = (PH^{-1})(HX)$).

---
### Q4: Essential Matrix

#### Question

*Two cameras fixate on a point P in 3D space such that their optical axes intersect at this point. Show that the $E_{33}$ element of their associated Essential matrix E is zero.*

#### Solution

When a camera fixates on a point, its optical axis passes through that point. The optical axis intersects the image plane at the *principal point*. If both cameras fixate on the same 3D point X, it means that X is imaged onto the principal point of each camera.

In normalized image coordinates, the principal point is at the origin $(0, 0)$. In homogeneous coordinates, this corresponds to $x_1 = [0, 0, 1]^T$ for the first camera and $x_2 = [0, 0, 1]^T$ for the second camera.

The epipolar constraint is defined by the Essential matrix E as:
$$ x_2^T E x_1 = 0 $$
Substituting the coordinates of the principal points into this equation:
$$ \begin{bmatrix} 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} E_{11} & E_{12} & E_{13} \\ E_{21} & E_{22} & E_{23} \\ E_{31} & E_{32} & E_{33} \end{bmatrix} \begin{bmatrix} 0 \\ 0 \\ 1 \end{bmatrix} = 0 $$
Performing the matrix multiplication:
$$ \begin{bmatrix} 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} E_{13} \\ E_{23} \\ E_{33} \end{bmatrix} = 0 $$
$$ (0 \cdot E_{13}) + (0 \cdot E_{23}) + (1 \cdot E_{33}) = 0 $$
This simplifies directly to:
$$ E_{33} = 0 $$
Thus, the (3,3) element of the Essential matrix must be zero.

---
### Q5: Homography

#### Question

*Suppose a camera with intrinsic matrix K rotates about its optical centre by a rotation matrix R. (a) Show that its two views are related by a homography H such that $x_2 = Hx_1$. (b) Show that if $\theta$ is the rotation between two views, then the angle $2\theta$ corresponds to the homography $H^2$.*

#### Solution

**(a) Derivation of the Homography**

Let a point in 3D space be $X$. Its projection $x_1$ in the first camera view is given by:
$$ \lambda_1 x_1 = K [I | 0] X $$
Since the camera only rotates, the translation is zero. The 3D point $X$ can be expressed in terms of its image point $x_1$ by inverting this relationship:
$$ X = \lambda_1 K^{-1} x_1 $$

The projection of the same 3D point $X$ in the second camera view, after a rotation R, is given by:
$$ \lambda_2 x_2 = K [R | 0] X $$
Now, we substitute the expression for $X$ from the first view into the equation for the second view:
$$ \lambda_2 x_2 = K [R | 0] (\lambda_1 K^{-1} x_1) $$
Since $[R|0]$ operates on a 3D vector, this becomes:
$$ \lambda_2 x_2 = \lambda_1 K R K^{-1} x_1 $$
Dropping the scalar depth constants $\lambda_1$ and $\lambda_2$ (as we are in homogeneous coordinates), we get the relationship:
$$ x_2 = (K R K^{-1}) x_1 $$
We define the homography matrix $H$ as:
$$ H = K R K^{-1} $$
Thus, the two image points are related by the homography $x_2 = Hx_1$.

**(b) Homography for Rotation $2\theta$**

If the camera is rotated again by the same rotation R (corresponding to an angle $\theta$), the new point $x_3$ would be related to $x_2$ by the same homography H:
$$ x_3 = H x_2 $$
Substituting the expression for $x_2$:
$$ x_3 = H (H x_1) = H^2 x_1 $$
The total rotation from the first view to the third view is $R \cdot R = R^2$. A rotation by an angle $\theta$ followed by another rotation by $\theta$ around the same axis results in a total rotation by $2\theta$. Therefore, the homography corresponding to a rotation of $2\theta$ is indeed $H^2$.

---
### Q6: Dense Visual Odometry

#### Question

*The photometric error for Dense-VO is given as $\sum_{x \in I_1} || I_1(x) - I_2(w(x, (R|t))) ||^2$. Assuming the depth $d$ of a point $x$ in image $I_1$ is known, (a) describe the steps to map this point to the second image and provide an expression for the warping function $w(x, (R|t))$. (b) What is the nature of this error and how can it be solved?*

#### Solution

**(a) The Warping Function $w(x, (R|t))$**

The function $w$ maps a pixel coordinate $x$ from the first image ($I_1$) to its corresponding pixel coordinate in the second image ($I_2$) given the relative camera motion $\{R, t\}$ and the pixel's depth $d$. The steps are:

1.  *Back-projection to 3D*: First, we lift the 2D pixel $x$ (in homogeneous coordinates) back into a 3D point $X$ in the first camera's coordinate frame. This is done by inverting the pinhole camera projection equation, using the known intrinsic matrix K and depth $d$.
    $$ X = d \cdot K^{-1} x $$

2.  *Transform to Second Camera Frame*: Next, we transform this 3D point $X$ into the coordinate system of the second camera using the rigid body motion $\{R, t\}$.
    $$ X' = R X + t $$

3.  *Re-projection to 2D*: Finally, we project the transformed 3D point $X'$ back onto the image plane of the second camera using the intrinsic matrix K.
    $$ x' = K X' $$

Combining these steps gives the full mathematical expression for the warping function $w(x, (R|t))$:
$$ w(x, (R|t)) = K (d \cdot R K^{-1} x + t) $$

**(b) Nature of the Photometric Error and its Solution**

The photometric error is the difference in intensity values between a pixel in the first image and its corresponding warped location in the second image. This error function is *non-linear and non-convex* with respect to the motion parameters R and t.

Because it's a non-linear optimization problem, we cannot solve for the best camera motion $\{R, t\}$ in closed form. Instead, we must use an iterative estimation method like *Gauss-Newton* or *Levenberg-Marquardt*. These methods start with an initial guess for the motion and iteratively refine it by solving a sequence of linear approximations of the problem, with the goal of finding the motion parameters that minimize the total photometric error.


---
---
### Q7: Bundle Adjustment

#### Question
*What is Bundle Adjustment? Describe its objective function mathematically. Explain the structure of the Jacobian matrix used in its optimization.*

#### Solution
*Bundle Adjustment (BA)* is a large-scale non-linear optimization problem that is fundamental to 3D reconstruction tasks like Structure from Motion (SfM) and SLAM. Its primary purpose is to *simultaneously* refine the 3D coordinates of all map points and the parameters of all camera poses (position and orientation) to achieve a globally consistent and optimal reconstruction. The name comes from the "bundles" of light rays that project from each 3D point to each camera center, which are adjusted to minimize error.

#### *The Objective Function*
The goal of Bundle Adjustment is to minimize the total *reprojection error*. This error is the sum of squared distances between the observed 2D feature locations in the images and the predicted 2D projections of the 3D map points based on the current camera pose estimates.

Mathematically, if we have $m$ cameras and $n$ 3D points, the objective function is:
$$ \min_{C_i, X_j} \sum_{i=1}^{m} \sum_{j=1}^{n} v_{ij} \| x_{ij} - \pi(C_i, X_j) \|^2 $$
Where:
- $C_i$ represents the parameters of the $i$-th camera (rotation $R_i$ and translation $t_i$).
- $X_j$ represents the coordinates of the $j$-th 3D point.
- $x_{ij}$ is the observed 2D location of point $j$ in image $i$.
- $\pi(C_i, X_j)$ is the projection function that maps the 3D point $X_j$ into image $i$ using camera parameters $C_i$.
- $v_{ij}$ is a binary variable that is 1 if point $j$ is visible in camera $i$, and 0 otherwise.

#### *The Jacobian Structure*
This non-linear least squares problem is solved iteratively using methods like Levenberg-Marquardt. This requires computing the Jacobian matrix of the residuals. The state vector is partitioned into two sets of variables: the camera parameters and the 3D point coordinates.
$$ \text{State Vector} = [c_1, \dots, c_m, x_1, \dots, x_n]^T $$
The Jacobian matrix has a very specific sparse block structure. For a single residual term $r_{ij} = x_{ij} - \pi(C_i, X_j)$, the derivatives are non-zero only with respect to the parameters of the $i$-th camera and the $j$-th point. All other derivatives are zero.

This results in a Jacobian matrix that looks like this:
$$ J = \begin{bmatrix} A & B \end{bmatrix} = \begin{bmatrix} \frac{\partial r}{\partial C} & \frac{\partial r}{\partial X} \end{bmatrix} $$
- The matrix $A$ (derivatives with respect to camera parameters) is block-diagonal.
- The matrix $B$ (derivatives with respect to point parameters) is also sparse.

This sparse structure is critical because it can be exploited by specialized solvers (like the Schur complement trick) to solve the large linear system required at each iteration much more efficiently than a dense solver could.



---
### Q8: EKF-SLAM

#### Question
*Describe the key steps of the EKF-SLAM algorithm. What are the main challenges or limitations of this approach?*

#### Solution
*Extended Kalman Filter (EKF) SLAM* is a classic probabilistic approach to solving the SLAM problem. It maintains a state estimate as a single multivariate Gaussian distribution, represented by a mean vector $\mu$ and a covariance matrix $\Sigma$. The state vector includes both the robot's pose and the positions of all landmarks in the map.

$$ x_t = \begin{bmatrix} x_{robot} \\ x_{landmark_1} \\ \vdots \\ x_{landmark_N} \end{bmatrix}, \quad \Sigma_t = \begin{bmatrix} \Sigma_{rr} & \Sigma_{rL} \\ \Sigma_{Lr} & \Sigma_{LL} \end{bmatrix} $$

The algorithm operates in a two-step prediction-correction cycle:

1.  *Prediction Step*: When the robot moves, a motion model $g$ is used to predict its new state. This step propagates the robot's uncertainty, causing the covariance to grow.
    * *Predict Mean*: $\bar{\mu}_t = g(\mu_{t-1}, u_t)$
    * *Predict Covariance*: $\bar{\Sigma}_t = G_t \Sigma_{t-1} G_t^T + R_t$
    Here, $u_t$ is the control input, $G_t$ is the Jacobian of the motion model with respect to the state, and $R_t$ is the motion noise covariance.

2.  *Correction (Update) Step*: When the robot observes a landmark, the measurement $z_t$ is used to correct the state estimate. This step reduces uncertainty.
    * *Compute Kalman Gain*: $K_t = \bar{\Sigma}_t H_t^T (H_t \bar{\Sigma}_t H_t^T + Q_t)^{-1}$
    * *Update Mean*: $\mu_t = \bar{\mu}_t + K_t(z_t - h(\bar{\mu}_t))$
    * *Update Covariance*: $\Sigma_t = (I - K_t H_t) \bar{\Sigma}_t$
    Here, $h$ is the observation model, $H_t$ is its Jacobian with respect to the state, and $Q_t$ is the measurement noise covariance. The term $(z_t - h(\bar{\mu}_t))$ is the *innovation* or measurement residual.

#### *Challenges and Limitations of EKF-SLAM*

* *Computational Complexity*: The covariance matrix $\Sigma$ is of size $(3+2N) \times (3+2N)$ for a 2D robot with N point landmarks. Updating this matrix at each step requires $O(N^2)$ computation. This quadratic complexity makes the standard EKF-SLAM algorithm computationally infeasible for large-scale maps with many landmarks.

* *Data Association*: The algorithm assumes that correspondences between observations and landmarks are known. In practice, determining which landmark an observation corresponds to (the data association problem) is very difficult. An incorrect association can be catastrophic, leading to a corrupted map and a lost robot.

* *Linearization Errors*: The EKF relies on a first-order Taylor series approximation (linearization) of the typically non-linear motion and observation models. If the models are highly non-linear or the uncertainty is large, this linearization can introduce significant errors, potentially causing the filter to diverge and become inconsistent.

* *Gaussian Assumption*: EKF represents the state with a single, unimodal Gaussian distribution. It cannot represent multi-modal beliefs, which can arise in ambiguous situations (e.g., seeing two identical-looking corridors). If the true belief is not Gaussian, the EKF's estimate can be a poor approximation.

---
### Q9: Image Formation

#### Question
*Derive the pinhole camera model. Show the full relationship between a 3D world coordinate and a 2D pixel coordinate, explaining the role of the intrinsic and extrinsic parameter matrices.*

#### Solution
The pinhole camera model is the simplest mathematical model for describing how a 3D scene is projected onto a 2D image plane.

#### *Ideal Pinhole Model and Similar Triangles*
Imagine a camera as an ideal box with a tiny aperture (a pinhole) on the front and an image plane at the back. A light ray from a 3D point $X = [X, Y, Z]^T$ in the camera's coordinate system passes through the pinhole (the origin) and strikes the image plane, which is located at a distance $f$ (the focal length) from the pinhole.

Using similar triangles, we can derive the coordinates $(x', y')$ of the projected point on the image plane:
$$ \frac{x'}{f} = \frac{X}{Z} \quad \implies \quad x' = f \frac{X}{Z} $$
$$ \frac{y'}{f} = \frac{Y}{Z} \quad \implies \quad y' = f \frac{Y}{Z} $$

#### *From Ideal to Pixel Coordinates: The Intrinsic Matrix*
The ideal coordinates $(x', y')$ are in metric units on the image plane. We need to convert them to pixel coordinates $(u, v)$. This involves two steps:
1.  Scaling by pixel size: The focal lengths $f_x$ and $f_y$ (in units of pixels per meter) scale the metric coordinates.
2.  Offsetting by the principal point: The origin of the pixel coordinate system is usually at the top-left corner, while the optical axis intersects the image plane at the *principal point* $(c_x, c_y)$.

This relationship can be expressed in matrix form using homogeneous coordinates:
$$ \begin{bmatrix} u \\ v \\ 1 \end{bmatrix} = \begin{bmatrix} f_x & 0 & c_x \\ 0 & f_y & c_y \\ 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} X/Z \\ Y/Z \\ 1 \end{bmatrix} $$
The $3 \times 3$ matrix is the **intrinsic matrix K**, which contains the internal parameters of the camera.

#### *From World to Camera Coordinates: The Extrinsic Matrix*
A 3D point is usually defined in a world coordinate system, not the camera's. We need to transform the world point $X_{world}$ into the camera's coordinate system, $X_{cam}$. This is a standard rigid body transformation defined by a rotation matrix $R$ and a translation vector $t$.
$$ X_{cam} = R X_{world} + t $$
This can be written in homogeneous coordinates using a $3 \times 4$ matrix $[R|t]$, known as the **extrinsic matrix**.

#### *The Full Camera Projection Matrix*
Combining all steps, we can write a single equation that maps a 3D point in the world frame directly to a 2D point in the image frame.
Let $x_{img} = [u, v, 1]^T$ and $X_{world} = [X_w, Y_w, Z_w, 1]^T$.
$$ \lambda \begin{bmatrix} u \\ v \\ 1 \end{bmatrix} = \begin{bmatrix} f_x & 0 & c_x \\ 0 & f_y & c_y \\ 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} r_{11} & r_{12} & r_{13} & t_x \\ r_{21} & r_{22} & r_{23} & t_y \\ r_{31} & r_{32} & r_{33} & t_z \end{bmatrix} \begin{bmatrix} X_w \\ Y_w \\ Z_w \\ 1 \end{bmatrix} $$
Here, $\lambda$ is the depth of the point in the camera frame. This is often written more compactly using the $3 \times 4$ **camera projection matrix P**:
$$ \lambda x_{img} = P X_{world} \quad \text{where} \quad P = K[R|t] $$

---
### Q10: Triangulation

#### Question
*What is triangulation? Given two camera matrices $P_1$ and $P_2$ and a pair of corresponding image points $x_1$ and $x_2$, explain the mathematical procedure for finding the 3D point X.*

#### Solution
*Triangulation* is the geometric process of determining the 3D position of a point by observing it from two or more different viewpoints with known positions and orientations. Given a pair of corresponding 2D points in two images, we can back-project them as rays into 3D space. The intersection of these rays reveals the location of the 3D point.

#### *Mathematical Procedure*
Let the two camera projection matrices be $P_1$ and $P_2$, and the corresponding homogeneous image points be $x_1$ and $x_2$. The projection equations are:
$$ \lambda_1 x_1 = P_1 X $$
$$ \lambda_2 x_2 = P_2 X $$
where $\lambda_1$ and $\lambda_2$ are the unknown depths. We can eliminate these unknown scalars by using the vector cross product property, $v \times v = 0$. Since $P_1 X$ must be parallel to $x_1$, their cross product is zero:
$$ x_1 \times (P_1 X) = 0 $$
$$ x_2 \times (P_2 X) = 0 $$
Let $x_1 = [u_1, v_1, 1]^T$ and let the rows of $P_1$ be $p_1^{(1)T}$, $p_2^{(1)T}$, and $p_3^{(1)T}$. The cross product $x_1 \times (P_1 X)$ yields three linear equations in the components of $X$:
$$ \begin{cases} v_1 (p_3^{(1)T} X) - 1 (p_2^{(1)T} X) = 0 \\ 1 (p_1^{(1)T} X) - u_1 (p_3^{(1)T} X) = 0 \\ u_1 (p_2^{(1)T} X) - v_1 (p_1^{(1)T} X) = 0 \end{cases} $$
Only two of these equations are linearly independent. We select two from the first camera and two from the second camera to form a system of four linear equations in the four components of the homogeneous 3D point $X$. This system can be written in the form:
$$ A X = 0 $$
where A is a $4 \times 4$ matrix constructed from the elements of $x_1, x_2, P_1,$ and $P_2$.

In practice, due to noise in the image point locations, the back-projected rays will not perfectly intersect. Therefore, the system $AX=0$ will not have a trivial solution. The problem becomes finding the point $X$ that is "closest" to the intersection, which means finding the $X$ that minimizes $\|AX\|$. This is a linear least-squares problem. The solution for $X$ is the eigenvector of the matrix $A^T A$ that corresponds to its smallest eigenvalue.


---
### Q11: Recovering Rotation from Homography

#### Question
*If a homography H is known to be induced by a pure camera rotation R, and the camera's intrinsic matrix K is also known, how can the rotation R be recovered?*

#### Solution
When a camera undergoes a pure rotation R about its optical center, with no translation, the relationship between a point $x_1$ in the first view and its corresponding point $x_2$ in the second view is described by the homography matrix $H$:
$$ x_2 = H x_1 $$
This homography matrix $H$ is related to the camera's intrinsic matrix $K$ and the rotation matrix $R$ by the following equation:
$$ H = K R K^{-1} $$
Our goal is to solve this equation for $R$. This can be achieved through straightforward algebraic manipulation.

*Derivation*:
1.  Start with the known relationship:
    $$ H = K R K^{-1} $$
2.  Pre-multiply both sides by the inverse of the intrinsic matrix, $K^{-1}$:
    $$ K^{-1} H = K^{-1} (K R K^{-1}) $$
3.  Since matrix multiplication is associative, we can regroup the terms on the right side:
    $$ K^{-1} H = (K^{-1} K) R K^{-1} $$
4.  The product of a matrix and its inverse is the identity matrix, $I$:
    $$ K^{-1} H = I R K^{-1} = R K^{-1} $$
5.  Now, to isolate R, post-multiply both sides by the intrinsic matrix, $K$:
    $$ (K^{-1} H) K = (R K^{-1}) K $$
6.  Again, using associativity and the property of the inverse:
    $$ K^{-1} H K = R (K^{-1} K) = R I = R $$

This gives us the final expression for recovering the rotation matrix:
$$ R = K^{-1} H K $$
*Intuition*: This result shows that the homography matrix $H$ is a *similarity transform* of the pure rotation matrix $R$. The rotation is "conjugated" or "twisted" by the camera's internal geometry, represented by $K$. To recover the pure rotation, we must "un-twist" the homography by applying the inverse intrinsics from the left and the regular intrinsics from the right.

---
### Q12: Properties of the Fundamental Matrix

#### Question
*What is the relationship between the Fundamental Matrix F and the epipoles $e_1$ and $e_2$? Prove these relationships mathematically.*

#### Solution
The Fundamental Matrix $F$ is a $3 \times 3$ matrix of rank 2 that encapsulates the epipolar geometry between two camera views. The epipoles, $e_1$ and $e_2$, are the projections of one camera's center onto the other camera's image plane. There are two fundamental relationships that define the epipoles as the null-spaces of the Fundamental Matrix.

#### *Relationship 1: The Left Null-Space*
The epipole in the second image, $e_2$, is the left null-space of the Fundamental Matrix.
$$ e_2^T F = 0 $$

*Intuition and Derivation*:
The epipolar line $l_2$ in the second image corresponding to a point $x_1$ in the first image is given by the equation $l_2 = F x_1$.
The epipole $e_2$ is the point in the second image where all epipolar lines intersect. Therefore, for any and every epipolar line $l_2$, the epipole $e_2$ must lie on that line.

The condition for a point to lie on a line in homogeneous coordinates is that their dot product is zero. Thus, for any $x_1$:
$$ e_2^T l_2 = 0 $$
Substituting the expression for $l_2$:
$$ e_2^T (F x_1) = 0 $$
Using associativity of matrix multiplication:
$$ (e_2^T F) x_1 = 0 $$
This equation must hold true for *all* possible image points $x_1$. The only way a vector-vector product can be zero for any arbitrary vector $x_1$ is if the vector multiplying it is the zero vector. Therefore:
$$ e_2^T F = \mathbf{0}^T $$
This proves that $e_2$ is the left null-space of $F$.

#### *Relationship 2: The Right Null-Space*
The epipole in the first image, $e_1$, is the right null-space (or simply, the null-space) of the Fundamental Matrix.
$$ F e_1 = 0 $$

*Intuition and Derivation*:
Similarly, the epipolar line $l_1$ in the first image corresponding to a point $x_2$ in the second image is given by $l_1 = F^T x_2$.
The epipole $e_1$ is the intersection point of all epipolar lines in the first image. Therefore, $e_1$ must lie on every line $l_1$. The condition is:
$$ e_1^T l_1 = 0 $$
Substituting the expression for $l_1$:
$$ e_1^T (F^T x_2) = 0 $$
Using the property of the transpose, $(AB)^T = B^T A^T$:
$$ ( (F e_1)^T )^T x_2 = 0 \implies (F e_1)^T x_2 = 0 $$
This equation must hold for *all* possible image points $x_2$. For this to be true, the vector $(F e_1)$ must be the zero vector. Therefore:
$$ F e_1 = \mathbf{0} $$
This proves that $e_1$ is the right null-space of $F$.

---
### Q13: Lie Algebra for Robotics Optimization

#### Question
*What are the Lie Group SE(3) and the Lie Algebra $\mathfrak{se}(3)$? Explain the role of the exponential and logarithmic maps in connecting them, especially in the context of optimization for SLAM.*

#### Solution
Lie theory provides the mathematical foundation for representing and optimizing rigid body motions in a way that correctly respects their geometric structure.

#### *The Lie Group SE(3): The Space of Poses*
*SE(3)* is the *Special Euclidean Group* in 3D. It is the set of all $4 \times 4$ homogeneous transformation matrices that represent rigid body motions (a rotation and a translation).
$$ T = \begin{bmatrix} R & t \\ 0 & 1 \end{bmatrix} \quad \text{where } R \in SO(3), t \in \mathbb{R}^3 $$
It is called a *group* because it satisfies the group axioms (closure under multiplication, identity element, inverse for every element, associativity). More importantly, it is a *smooth manifold*. This means it is a space that locally looks like a standard Euclidean space, but has a global curved structure. This curvature is the reason why standard vector addition is not a valid operation for poses; the sum of two transformation matrices is not, in general, a valid transformation matrix.

#### *The Lie Algebra $\mathfrak{se}(3)$: The Space of Velocities*
The *Lie Algebra $\mathfrak{se}(3)$* can be intuitively understood as the *tangent space* to the SE(3) manifold at the identity element. It is the space of all possible infinitesimal motions, or velocities. It is a 6-dimensional vector space.

An element $\xi \in \mathfrak{se}(3)$ is a 6-dimensional vector called a *twist*, which combines a linear velocity $v$ and an angular velocity $\omega$:
$$ \xi = \begin{bmatrix} v \\ \omega \end{bmatrix} \in \mathbb{R}^6 $$
This vector can be mapped to a $4 \times 4$ matrix form, often written with a "hat" or "wedge" operator:
$$ [\xi]_\times = \begin{bmatrix} [\omega]_\times & v \\ 0 & 0 \end{bmatrix} \quad \text{where } [\omega]_\times = \begin{bmatrix} 0 & -\omega_z & \omega_y \\ \omega_z & 0 & -\omega_x \\ -\omega_y & \omega_x & 0 \end{bmatrix} $$

#### *Connecting the Spaces: Exponential and Logarithmic Maps*

* ***The Exponential Map***, $\exp: \mathfrak{se}(3) \to SE(3)$, is the function that converts an element of the Lie algebra (a velocity vector $\xi$) into a finite transformation on the Lie group (a pose matrix $T$). Intuitively, it is equivalent to integrating a constant velocity $\xi$ for one unit of time to get a finite displacement $T$.

* ***The Logarithmic Map***, $\log: SE(3) \to \mathfrak{se}(3)$, is the inverse operation. It takes a transformation matrix $T$ and returns the unique twist vector $\xi$ that would generate that transformation in one unit of time.

#### *Role in SLAM Optimization*
In optimization problems like SLAM or Bundle Adjustment, we need to iteratively update an estimate of a camera pose, $T_{old}$. Since we cannot simply add an update to $T_{old}$ in the curved SE(3) manifold, we use Lie theory as follows:

1.  The optimization algorithm (e.g., Gauss-Newton) computes an update step. This update step, $\delta\xi$, is calculated in the local tangent space, which is a well-behaved 6D Euclidean vector space.
2.  This small update vector $\delta\xi$ is then mapped from the Lie algebra back onto the manifold using the exponential map to become a small transformation matrix, $\exp([\delta\xi]_\times)$.
3.  This small transformation is then applied *multiplicatively* to the current pose estimate to get the new, refined pose:
    $$ T_{new} = T_{old} \cdot \exp([\delta\xi]_\times) $$
This procedure guarantees that the updated pose $T_{new}$ is always a valid member of SE(3), correctly respecting the geometry of the problem.