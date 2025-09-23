---
{"dg-publish":true,"permalink":"/robotics-dynamics-control/forward-inverse-kinematics/"}
---


# Forward and Inverse Kinematics

This note covers the core kinematic problems for manipulators. *Forward Kinematics (FK)* is the process of calculating the end-effector's position and orientation from a given set of joint angles. *Inverse Kinematics (IK)* is the reverse and more challenging problem of finding the required joint angles to place the end-effector at a desired pose. These calculations rely heavily on the principles of `[[Co-ordinate-Transforms]]`.



### Forward Kinematics (FK)

The goal of forward kinematics is to find the function $\mathcal{K}$ that maps the robot's configuration in joint space, $q$, to the end-effector's pose, $T_{BE}$, in the task space.
$$T_{BE} = \mathcal{K}(q)$$

The fundamental method for solving the FK problem is by chaining together the homogeneous transformation matrices for each link, starting from the base and moving outward to the end-effector. For a robot with $n$ joints, the transformation is:
$$T_{WE} = T_{W1} T_{12} \dots T_{n-1, E}$$
Each transformation $T_{i-1, i}$ represents the pose of frame $\{i\}$ relative to frame $\{i-1\}$.

### Derivation for a 2R Planar Arm
For a 2-joint planar arm with link lengths $L_1$ and $L_2$, the transformation for the end-effector pose is found by composing the transformations across each link. This involves a rotation by the joint angle followed by a translation along the link's length.
$$T_{WE} = (R(\theta_1) \cdot \text{Trans}(L_1, 0)) \cdot (R(\theta_2) \cdot \text{Trans}(L_2, 0))$$
Multiplying the homogeneous matrices gives us the final end-effector position equations:
$$x_E = L_1 \cos\theta_1 + L_2 \cos(\theta_1 + \theta_2)$$
$$y_E = L_1 \sin\theta_1 + L_2 \sin(\theta_1 + \theta_2)$$
The final orientation of the end-effector is the sum of the joint angles, $\theta_1 + \theta_2$. This orientation can be represented in various ways, as detailed in my notes on `[[Mobile-Robotics/EULER ANGLES\|EULER ANGLES]]` and `[[Quaternions]]`.

### The Denavit-Hartenberg (DH) Convention
For more complex 3D manipulators, creating these transformations manually can be error-prone. The *Denavit-Hartenberg (DH) convention* provides a standardized, systematic method for assigning coordinate frames to each link and defining the transformation between them using just four parameters. This makes the FK problem much more manageable.

---

### Inverse Kinematics (IK)

Inverse kinematics is the inverse problem: given a desired pose for the end-effector, $T_{BE}$, we need to find the joint angles, $q$, that achieve it.
$$q = \mathcal{K}^{-1}(T_{BE})$$
IK is generally much harder than FK because:
* A solution may not exist if the target pose is outside the robot's workspace.
* Multiple solutions often exist. For a 2R arm, this manifests as "elbow up" vs. "elbow down" configurations.
* The governing equations are non-linear, making them difficult to solve.

### Analytical IK Solution for a 2R Planar Arm

For simple manipulators like a 2R arm, we can find a closed-form analytical solution.

### Geometric Approach
The most intuitive method is geometric. We can form a triangle between the robot's base, its elbow joint, and the target end-effector position $(x, y)$.

Using the Law of Cosines, we can solve for the angle at the elbow, which directly relates to $\theta_2$:
$$x^2 + y^2 = L_1^2 + L_2^2 - 2L_1L_2 \cos(\pi - \theta_2)$$
Since $\cos(\pi - \theta_2) = -\cos\theta_2$, we can rearrange to solve for $\cos\theta_2$:
$$\cos\theta_2 = \frac{x^2+y^2 - L_1^2 - L_2^2}{2L_1L_2}$$
Because $\cos(\theta_2) = \cos(-\theta_2)$, there are two possible solutions for $\theta_2$, corresponding to the elbow-up and elbow-down poses. Once we have a value for $\theta_2$, we can use trigonometry to solve for $\theta_1$.

### Algebraic Approach
Alternatively, we can start with the FK equations and solve them algebraically.
$$x = L_1 \cos\theta_1 + L_2 \cos(\theta_1 + \theta_2)$$
$$y = L_1 \sin\theta_1 + L_2 \sin(\theta_1 + \theta_2)$$
By squaring and adding these two equations and simplifying with trigonometric identities, we can isolate $\cos\theta_2$ and arrive at the exact same solution as the geometric method.

### Numerical IK Solutions
For manipulators with more joints, an analytical solution is often impossible. In these cases, we rely on iterative numerical methods to find an answer. These methods start with an initial guess for the joint angles and iteratively refine it to minimize the error between the current end-effector pose and the desired pose.

This turns the IK problem into a root-finding or optimization problem. The methods used are forms of non-linear optimization, a topic I cover in more detail in my `[[NLS]]` note.

---

### Velocities and the Jacobian

To understand the relationship between the speed of the joints and the speed of the end-effector, we use the *Jacobian matrix*. The Jacobian, $J(\theta)$, is the matrix that maps joint velocities ($\dot{\theta}$) to end-effector linear and angular velocities ($v$).
$$v = J(\theta)\dot{\theta}$$
The Jacobian is found by taking the partial derivatives of the forward kinematics equations with respect to each joint variable. For our 2R arm, differentiating the FK equations with respect to time yields:
$$
\begin{bmatrix} \dot{x} \\ \dot{y} \end{bmatrix} = \begin{bmatrix} -L_1s_1 - L_2s_{12} & -L_2s_{12} \\ L_1c_1 + L_2c_{12} & L_2c_{12} \end{bmatrix} \begin{bmatrix} \dot{\theta}_1 \\ \dot{\theta}_2 \end{bmatrix}
$$
The $2 \times 2$ matrix in this equation is the Jacobian for the 2R planar arm.

### Singularity
A manipulator is in a *singularity* when the Jacobian matrix loses rank and cannot be inverted. At a singularity, there are certain directions in the task space that the end-effector cannot move in, no matter how the joints move. This means the robot has momentarily lost a degree of freedom.

This happens when the determinant of the Jacobian is zero. For the 2R arm, the determinant is $det(J) = L_1 L_2 \sin\theta_2$. This becomes zero when $\sin\theta_2=0$, which occurs when $\theta_2 = 0$ or $\theta_2 = \pi$. Geometrically, this is when the arm is fully stretched out or folded back on itself.

This concept is directly analogous to the problem of Gimbal Lock in [[EULER ANGLES]] note, which is a singularity in an orientation representation.