---
{"dg-publish":true,"permalink":"/robotics-dynamics-control/configurations-and-robot-transformations/"}
---


### Configuration, State, and Degrees of Freedom

### Configuration and C-Space

The first question to answer in robotics is, simply, *where is the robot?*. 
A proper complete answer would require knowing position of every single point in the robot with respect to some global frame. This complete specification is called the robot's *configuration*.
> Configuration of a robot specifies the position of every point of the robot

A configuration, denoted by $q$, is a set of parameters that uniquely defines the robot's state. The set of all possible configurations a robot can achieve is its *configuration space*, or *C-space*. 
The C-space is a crazy concept because it transforms the complex geometric problem of a robot moving among obstacles in the physical *workspace* into a simpler problem of a single point moving in this higher-dimensional C-space. Configurations where the robot does not collide with obstacles are part of the *collision-free C-space*.

### Degrees of Freedom (DOF)

The *number of degrees of freedom (DOF)* is the minimum number of independent parameters needed to completely specify the robot's configuration. 
This means the DOF is also the dimension of the robot's C-space.

* A point in a 2D plane has 2 DOF, described by $(x, y)$.
* A rigid body in a 2D plane requires three parameters: its position $(x, y)$ and its orientation $\theta$. It therefore has 3 DOF.
* A rigid body in 3D space requires six parameters: its position $(x, y, z)$ and its orientation, which can be described by three angles (e.g., roll, pitch, yaw). It has 6 DOF.

We can develop an intuition for this by considering the constraints on a rigid body. In 2D, once we fix the position of a point *A* (2 DOF), a second point *B* is constrained to lie on a circle around *A* (1 more DOF). Any other point *C* is then fully determined by its distances to *A* and *B*. The total is $2+1=3$ DOF. In 3D, the circles become spheres, leading to $3+2+1=6$ DOF.

### Robot Joints

Robots are typically composed of rigid bodies called *links* connected by *joints*. Joints introduce constraints and reduce the overall DOF of the system. 
![Pasted image 20250921022014.png](/img/user/Robotics-Dynamics-Control/Pasted%20image%2020250921022014.png)

![Pasted image 20250921021902.png](/img/user/Robotics-Dynamics-Control/Pasted%20image%2020250921021902.png)

_Image taken from Modern Robotics: Mechanics, Planning, and Control_
### Grübler's Formula
To calculate the DOF of a mechanism, we can use *Grübler's Formula*. This formula is based on the principle of subtracting the constraints imposed by joints from the total possible freedoms of the links.

$$DOF = m(N-1-J) + \sum_{i=1}^{J}f_{i}$$
where:
* $m$ is the DOF of a single rigid body in the workspace ($m=3$ for planar mechanisms, $m=6$ for spatial mechanisms).
* $N$ is the total number of links, including the fixed base (ground).
* $J$ is the total number of joints.
* $f_{i}$ is the number of freedoms provided by joint $i$.

*Example: A 3R Planar Arm*
For a planar arm with three revolute joints connected in a series to a base:
* It's a planar mechanism, so $m=3$.
* There are 3 moving links and 1 ground link, so $N=4$.
* There are 3 revolute joints, so $J=3$.
* Each planar revolute joint has 1 DOF, so $f_i = 1$ for all three joints.

Plugging this into the formula:
$$DOF = 3(4-1-3) + (1+1+1) = 3(0) + 3 = 3$$
This confirms our intuition that the arm has 3 degrees of freedom.

*Important Note:* Grübler's formula is only valid if all joint constraints are independent. For some mechanisms with redundant constraints, it may yield an incorrect result.

### Rigid Body Transformations in 2D

To describe a robot's configuration mathematically, we move from the notions of Euclidean geometry to the algebraic framework of *Cartesian geometry*. These are there in [[Mobile-Robotics/Co-ordinate-Transforms\|Co-ordinate-Transforms]] some what.

### Coordinate Frames and Rotation

We attach coordinate frames to objects of interest. 
A point $P$ can be represented as a vector from the origin of its frame, for example, $p = p_x\hat{x} + p_y\hat{y}$. By convention, we use *right-handed coordinate frames*.

To describe a robot's motion, we need to relate points described in a local *body frame* $\{B\}$ to a global *world frame* $\{W\}$. If frame $\{B\}$ is rotated by an angle $\theta$ with respect to $\{W\}$, we can find the coordinates of a point $p$ in the world frame, $p_W$, from its coordinates in the body frame, $p_B$.

The derivation proceeds as follows:
1.  The basis vectors of $\{B\}$ in terms of $\{W\}$ using trigonometry:
   $$\hat{x}_B = \cos\theta \hat{x}_W + \sin\theta \hat{y}_W$$
   $$\hat{y}_B = -\sin\theta \hat{x}_W + \cos\theta \hat{y}_W$$
2.  A point vector in the body frame is $p_B = p_{xB}\hat{x}_B + p_{yB}\hat{y}_B$. Substitute the expressions from step 1 to find the same point vector relative to the world frame, $p_W$:
   $$p_W = p_{xB}(\cos\theta \hat{x}_W + \sin\theta \hat{y}_W) + p_{yB}(-\sin\theta \hat{x}_W + \cos\theta \hat{y}_W)$$
3.  Group the $\hat{x}_W$ and $\hat{y}_W$ terms:
   $$p_W = (p_{xB}\cos\theta - p_{yB}\sin\theta)\hat{x}_W + (p_{xB}\sin\theta + p_{yB}\cos\theta)\hat{y}_W$$
4.  This can be written in matrix form, which gives us the 2D *rotation matrix*:
   $$\begin{bmatrix} p_{xW} \\ p_{yW} \end{bmatrix} = \begin{bmatrix} \cos\theta & -\sin\theta \\ \sin\theta & \cos\theta \end{bmatrix} \begin{bmatrix} p_{xB} \\ p_{yB} \end{bmatrix}$$
This relationship is compactly written as $p_W = R_{WB}p_B$. The rotation matrix $R_{WB}$ transforms the coordinates of a point from frame $\{B\}$ to frame $\{W\}$. This is very important distinction to make, the rotation matrix transforms the coordinates of a point from one frame to another, but if you apply the same rotation matrix to the basis vectors of the frame, it will give you the basis vectors of the other frame in the original frame's coordinates.

### The Special Orthogonal Group $SO(2)$
Rotation matrices have several important properties:
* They are *orthogonal*, meaning $R^T R = I$.  Therefore $R^{-1}=R^T$
* Their columns are mutually orthogonal unit vectors.
* Their determinant is always $+1$. A rotation preserves the length of vectors and the "handedness" of the coordinate system. (_Note: Not related to SO(2) but when we have determinant -1, it represents a reflection.)

The set of all $2 \times 2$ matrices that satisfy these properties forms the *Special Orthogonal Group of dimension 2*, denoted $SO(2)$.

### Homogeneous Transformations and $SE(2)$
When a frame is both rotated by $R$ and translated by a vector $t$, the transformation is $p_W = R_{WB}p_B + t_{WB}$. This mixes matrix multiplication and vector addition, which is annoying. We can combine them into a single matrix multiplication using *homogeneous coordinates*. We augment our vectors and matrices:
* A 2D point $p=(p_x, p_y)$ becomes a 3D vector $\tilde{p} = (p_x, p_y, 1)^T$.
* The transformation becomes a $3 \times 3$ matrix $T = \begin{bmatrix} R & t \\ 0 & 1 \end{bmatrix}$.

Now, the transformation is a single, clean multiplication: $\tilde{p}_W = T_{WB}\tilde{p}_B$.
$$
\begin{bmatrix} p_{xW} \\ p_{yW} \\ 1 \end{bmatrix} = \begin{bmatrix} \cos\theta & -\sin\theta & t_x \\ \sin\theta & \cos\theta & t_y \\ 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} p_{xB} \\ p_{yB} \\ 1 \end{bmatrix}
$$
These $3 \times 3$ transformation matrices form the *Special Euclidean Group of dimension 2*, or $SE(2)$. An advantage is that composing transformations is as simple as matrix multiplication: $T_{WA} = T_{WB}T_{BA}$.
(_That's because it's a property of  SE(2) that the composition of two transformations is another transformation in SE(2)._)




The fundamental insight is that the columns of a rotation matrix $R_{WB}$ are the basis vectors of the "new" frame $\{B\}$ expressed in the coordinates of the "old" frame $\{W\}$. This allows us to transform the coordinates of any point from frame $\{B\}$ to frame $\{W\}$ via matrix multiplication:
$$p_W = R_{WB} p_B$$

When we need to account for both rotation and translation, we use *homogeneous coordinates*. This combines the rotation matrix $R$ and a translation vector $t$ into a single transformation matrix $T$, allowing us to represent a full rigid body motion with a single matrix multiplication:
$$
\begin{bmatrix} p_W \\ 1 \end{bmatrix} = \begin{bmatrix} R & t \\ 0 & 1 \end{bmatrix} \begin{bmatrix} p_B \\ 1 \end{bmatrix}
$$


### Example 
A point $p_B = (2, 3, 5)$ is defined on a body $\{B\}$, which is initially coincident with the world frame $\{W\}$. The body is first rotated about its own Z-axis by $\pi/2$, then rotated about its new, local X-axis by $\pi/2$. Finally, it is translated by $t = (8, 1, 0)$ with respect to the world frame. Find the final coordinates of the point in the world frame, $p_W$.

The first rotation is by $\theta_1 = \pi/2$ about the Z-axis.
$$
R_1 = R_z(\pi/2) = \begin{bmatrix} \cos(\pi/2) & -\sin(\pi/2) & 0 \\ \sin(\pi/2) & \cos(\pi/2) & 0 \\ 0 & 0 & 1 \end{bmatrix} = \begin{bmatrix} 0 & -1 & 0 \\ 1 & 0 & 0 \\ 0 & 0 & 1 \end{bmatrix}
$$
The second rotation is by $\theta_2 = \pi/2$ about the *new* local X-axis.
$$
R_2 = R_x(\pi/2) = \begin{bmatrix} 1 & 0 & 0 \\ 0 & \cos(\pi/2) & -\sin(\pi/2) \\ 0 & \sin(\pi/2) & \cos(\pi/2) \end{bmatrix} = \begin{bmatrix} 1 & 0 & 0 \\ 0 & 0 & -1 \\ 0 & 1 & 0 \end{bmatrix}
$$

To find the final orientation of the body, we post-multiply the first rotation by the second.
$$
R_{WB} = R_1 R_2 = R_z(\pi/2) R_x(\pi/2) = \begin{bmatrix} 0 & -1 & 0 \\ 1 & 0 & 0 \\ 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} 1 & 0 & 0 \\ 0 & 0 & -1 \\ 0 & 1 & 0 \end{bmatrix} = \begin{bmatrix} 0 & 0 & 1 \\ 1 & 0 & 0 \\ 0 & 1 & 0 \end{bmatrix}
$$

Now we can find the final coordinates $p_W$ by applying the combined rotation and the final translation to the original point $p_B$.
$$p_W = R_{WB} p_B + t$$
$$
p_W = \begin{bmatrix} 0 & 0 & 1 \\ 1 & 0 & 0 \\ 0 & 1 & 0 \end{bmatrix} \begin{bmatrix} 2 \\ 3 \\ 5 \end{bmatrix} + \begin{bmatrix} 8 \\ 1 \\ 0 \end{bmatrix} = \begin{bmatrix} (0\cdot2 + 0\cdot3 + 1\cdot5) \\ (1\cdot2 + 0\cdot3 + 0\cdot5) \\ (0\cdot2 + 1\cdot3 + 0\cdot5) \end{bmatrix} + \begin{bmatrix} 8 \\ 1 \\ 0 \end{bmatrix}
$$
$$
p_W = \begin{bmatrix} 5 \\ 2 \\ 3 \end{bmatrix} + \begin{bmatrix} 8 \\ 1 \\ 0 \end{bmatrix} = \begin{bmatrix} 13 \\ 3 \\ 3 \end{bmatrix}
$$
The final coordinates of the point are $(13, 3, 3)$.


### Singularities in Orientation: Gimbal Lock

While representing orientation with a sequence of three rotations (*Euler angles*) is intuitive, it suffers from a critical problem known as *gimbal lock*. This is a *singularity* where the alignment of two rotation axes causes the loss of one degree of rotational freedom. I cover this in more detail in my [[Mobile-Robotics/EULER ANGLES\|EULER ANGLES]] note.

Let's examine the mathematical reason for this. Consider a ZYX roll-pitch-yaw convention. The final rotation is $R = R_z(y) R_y(p) R_x(r)$. If the pitch angle $p$ is set to $\pi/2$:
$$R_y(\pi/2) = \begin{bmatrix} 0 & 0 & 1 \\ 0 & 1 & 0 \\ -1 & 0 & 0 \end{bmatrix}$$
The full rotation becomes:
$$R = R_z(y) R_y(\pi/2) R_x(r)$$
A useful property of rotation matrices states that $R_y(\pi/2)R_x(r) = R_z(r)R_y(\pi/2)$. Substituting this in:
$$R = R_z(y) R_z(r) R_y(\pi/2) = R_z(y+r) R_y(\pi/2)$$
The final orientation now depends only on the *sum* of the yaw and roll angles, not their individual values. We can no longer distinguish between a yaw and a roll motion; the system has become degenerate and lost a degree of freedom.

---

### Axis-Angle Representation

An alternative, non-singular way to represent orientation is the *axis-angle* form. Euler's rotation theorem states that any orientation in 3D space can be described as a single rotation by an angle $\theta$ about a single unit axis $\hat{w}$. This is often more intuitive than a sequence of three separate rotations.

### Rodrigues' Formula Derivation
Rodrigues' formula provides a direct mapping from an axis $\hat{w}$ and angle $\theta$ to the equivalent $3 \times 3$ rotation matrix $R(\hat{w}, \theta)$. The slides note this was a derivation on the board, so here are the steps.

### Step 1: Decompose the Vector
We want to rotate a vector $v$ around the unit axis $\hat{w}$. We can decompose $v$ into two components: one parallel to $\hat{w}$ and one perpendicular to $\hat{w}$.
$$v = v_{||} + v_{\perp}$$
The parallel component is the projection of $v$ onto $\hat{w}$:
$$v_{||} = (\hat{w} \cdot v)\hat{w}$$
The perpendicular component is what remains:
$$v_{\perp} = v - v_{||} = v - (\hat{w} \cdot v)\hat{w}$$

### Step 2: Rotate the Components
The rotation only affects the perpendicular component. The parallel component lies on the axis of rotation and is therefore unchanged.
$$v_{||, rot} = v_{||}$$
The component $v_{\perp}$ rotates by angle $\theta$ in the plane whose normal is $\hat{w}$. The rotated vector, $v_{\perp, rot}$, can be described as a sum of a component along the original $v_{\perp}$ and a component along the vector orthogonal to both $\hat{w}$ and $v_{\perp}$ (which is given by $\hat{w} \times v$).
$$v_{\perp, rot} = (\cos\theta) v_{\perp} + (\sin\theta) (\hat{w} \times v_{\perp})$$
Since $\hat{w}$ and $v_{||}$ are parallel, their cross product is zero, meaning $\hat{w} \times v = \hat{w} \times (v_{||} + v_{\perp}) = \hat{w} \times v_{\perp}$. So we can write:
$$v_{\perp, rot} = (\cos\theta) v_{\perp} + (\sin\theta) (\hat{w} \times v)$$

### Step 3: Recombine and Simplify
The final rotated vector, $v_{rot}$, is the sum of the rotated components.
$$v_{rot} = v_{||, rot} + v_{\perp, rot} = v_{||} + (\cos\theta) v_{\perp} + (\sin\theta) (\hat{w} \times v)$$
Substitute the expressions for $v_{||}$ and $v_{\perp}$:
$$v_{rot} = (\hat{w} \cdot v)\hat{w} + \cos\theta (v - (\hat{w} \cdot v)\hat{w}) + \sin\theta (\hat{w} \times v)$$
Grouping terms for $v$ and $(\hat{w} \cdot v)\hat{w}$:
$$v_{rot} = v\cos\theta + (\hat{w} \cdot v)\hat{w}(1-\cos\theta) + (\hat{w} \times v)\sin\theta$$
This is the vector form of Rodrigues' formula.

### Step 4: Convert to Matrix Form
To get the matrix $R$ such that $v_{rot} = Rv$, we can represent the cross product as a matrix multiplication using the *skew-symmetric matrix* of $\hat{w}$, denoted $[\hat{w}]$:
$$[\hat{w}]v = \hat{w} \times v \quad \implies \quad [\hat{w}] = \begin{bmatrix} 0 & -w_z & w_y \\ w_z & 0 & -w_x \\ -w_y & w_x & 0 \end{bmatrix}$$
The term $(\hat{w} \cdot v)\hat{w}$ can be written as $(\hat{w}\hat{w}^T)v$. Also, a useful identity is $[\hat{w}]^2 = \hat{w}\hat{w}^T - I$.
Substituting these into the vector formula gives the final matrix form of Rodrigues' formula:
$$R(\hat{w}, \theta) = I\cos\theta + (\hat{w}\hat{w}^T)(1-\cos\theta) + [\hat{w}]\sin\theta$$
Using the identity for $[\hat{w}]^2$, this is more commonly written as:
$$R(\hat{w}, \theta) = I + \sin\theta[\hat{w}] + (1-\cos\theta)[\hat{w}]^2$$



