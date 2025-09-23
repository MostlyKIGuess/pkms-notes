---
{"dg-publish":true,"permalink":"/mobile-robotics/papa-where-do-i-multiply-left-or-right/"}
---

# Euler Angle Conventions 


While we all know that any 3D orientation can be achieved using 3 sequential rotations, order and _reference frame_ used for these are very important, different choices lead to different results and conventions. I will get to the bottom of XYZ ( fixed-frame ) and ZYX ( local-frame ) and explain how to use the "final"  resulting rotation matrix.
### Fixed-Frame vs. Local-Frame Rotations

* *Fixed-Frame (World) Rotations*: Each rotation is performed around an axis of the original, stationary world frame. To combine them, we **pre-multiply** (multiply from the left).
* *Local-Frame (Body/Euler) Rotations*: Each rotation is performed around an axis of the current, moving body frame. To combine them, we **post-multiply** (multiply from the right).

### XYZ Fixed-Frame Rotations

We perform three rotations in the order X, then Y, then Z, all with respect to the *fixed world frame*.
Let's derive the final rotation matrix, $R_{XYZ}$. We start with our body frame aligned with the world frame ($R = I$).

-  Rotation about World X-axis
The first rotation is by an angle $\alpha$ about the fixed world X-axis. Since this is the first rotation, the composition is simple:
$$R_1 = R_x(\alpha)$$

-  Rotation about World Y-axis
The second rotation is by an angle $\beta$ about the fixed world Y-axis. Because the reference frame is the fixed world frame, we must *pre-multiply* our current rotation matrix by the new one:
$$R_2 = R_y(\beta) \cdot R_1 = R_y(\beta)R_x(\alpha)$$

- Rotation about World Z-axis
The third rotation is by an angle $\gamma$ about the fixed world Z-axis. Again, we *pre-multiply*:
$$R_{final} = R_z(\gamma) \cdot R_2 = R_z(\gamma)R_y(\beta)R_x(\alpha)$$

So, the final rotation matrix for the XYZ fixed-frame convention is $R_{XYZ} = R_z(\gamma)R_y(\beta)R_x(\alpha)$. Notice that the matrices are applied in the reverse order of the operations.

###  ZYX Local-Frame Rotations (Roll-Pitch-Yaw)


This is the more common one, it's called the _yaw,pitch,roll_ , yes and not _roll-pitch-yaw_ as the order in which we multiply is _yaw,pitch_ and _roll_. That is why it's called ZYX.

final rotation matrix for $R_{ZYX}$.

 - Rotation about Local Z-axis (Yaw)
The first rotation is by an angle $\alpha$ about the local Z-axis.
$$R_1 = R_z(\alpha)$$

- Rotation about *new* Local Y-axis (Pitch)
The second rotation is by an angle $\beta$ about the *new* local Y-axis (the one that has already been rotated by the yaw). Because the reference frame is the moving local frame, we must *post-multiply*:
$$R_2 = R_1 \cdot R_y(\beta) = R_z(\alpha)R_y(\beta)$$

- Rotation about *newest* Local X-axis (Roll)
The third rotation is by an angle $\gamma$ about the *newest* local X-axis. Again, we *post-multiply*:
$$R_{final} = R_2 \cdot R_x(\gamma) = R_z(\alpha)R_y(\beta)R_x(\gamma)$$

So, the final rotation matrix for the ZYX local-frame convention is $R_{ZYX} = R_z(\alpha)R_y(\beta)R_x(\gamma)$. Notice the matrices are applied in the same order as the operations.


### The "Why" Behind World (Pre) vs. Local (Post) Multiplication

The entire confusion around pre- and post-multiplication boils down to one simple question: which ruler am I using for the measurement? Am I rotating based on the *fixed, unchanging room around me (the World Frame)*, or am I rotating based on *my own, moving perspective (the Local Frame)*?
### Funny Thingy

We saw how to make from world XYZ and local ZYX, good news is they are actually the same!

#### Equivalence Relation:

<iframe src="/img/user/Mobile-Robotics/new.pdf" width="100%" height="600px" title="new.pdf" style="border:1px solid #ccc;"></iframe>
### Why the Final Application is *Always* Pre-Multiplication

This section addresses core question: once we have the final rotation matrix $R$, how do we use it?

No matter how you derive the final rotation matrix , whether it's from an XYZ fixed-frame sequence, a ZYX local-frame sequence, Rodrigues' formula, or a quaternion .Its purpose is always the same: to transform the coordinates of a point from one frame to another.

The application of this final matrix to a point vector is *always* a pre-multiplication.

This can feel strange: why are there two ways to *build* the matrix, but only one way to *use* it?

The reason is that *building the matrix* and *using the matrix* are two fundamentally different jobs.

* *Building R*: The entire pre- vs. post-multiplication logic is part of the *construction process*. It's how we carefully combine a sequence of individual rotations to create a *single* final rotation matrix, $R$, that represents the total change in orientation from start to finish.

* *Using R*: Once I have that final matrix $R$, its job is to be a *translator*. It takes a point's coordinates that are written in the body's language (frame $\{B\}$) and translates them into the world's language (frame $\{W\}$).

If you have a point $p_B$ defined in a body frame $\{B\}$, and you have derived the rotation matrix $R_{WB}$ that describes the orientation of frame $\{B\}$ relative to the world frame $\{W\}$, the coordinates of that same point in the world frame, $p_W$, are found by:

The mathematical transformation of a point is:
$$p_W = R_{WB} \cdot p_B$$

By convention in linear algebra, when a matrix operates on a vector, the matrix is written on the left. 
So, once my final $R$ is built, I *always* pre-multiply the point vector by it. 
