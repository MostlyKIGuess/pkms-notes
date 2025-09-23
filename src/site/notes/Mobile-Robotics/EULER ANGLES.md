---
{"dg-publish":true,"permalink":"/mobile-robotics/euler-angles/"}
---



To my friends in awe why the title is in all caps, I love Euler numbers.

### Rotation Representations: Euler Angles

While a full 3x3 rotation matrix is a complete way to describe an orientation, it's often more intuitive to think about rotation in a simpler way. This is where *Euler Angles* come in.

The core idea is to break down any complex 3D rotation into a sequence of three simpler rotations around the principal axes of a coordinate frame. We often call these rotations by more familiar names:

-   **Roll**: Rotation around the body's forward-backward axis (often the X-axis).
-   **Pitch**: Rotation around the body's side-to-side axis (often the Y-axis).
-   **Yaw**: Rotation around the body's vertical axis (often the Z-axis).

YESS, after _months_ of not being able to remember this, 
I have found a very trivial way to remember this, imagine this airplane and stand on it thinking you are an army man , what I want you to do now is make a gun with your left hand, now your index finger is pointing outwards -- great,  this is step 1.
I would assume you are a normal person and point your thumb to the top, now your middle finger can go 90 degree from the index finger.

What you have now is exactly the Roll, Pitch, Yaw you wanted from the below image, just exactly in the opposite direction. 

Index -> X
Middle -> Y
Thumb -> Z


![Pasted image 20250919005915.png](/img/user/Mobile-Robotics/Pasted%20image%2020250919005915.png)

To get the final orientation, we simply multiply the three corresponding rotation matrices. For example, for a "ZYX" convention, the final rotation matrix would be:

$$ R_{ZYX} = R_Z(\psi) R_Y(\theta) R_X(\phi) $$

Where $\psi$ (psi) is the yaw, $\theta$ (theta) is the pitch, and $\phi$ (phi) is the roll angle.


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/mobile-robotics/papa-where-do-i-multiply-left-or-right/" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">




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
$R_1 = R_x(\alpha)$

-  Rotation about World Y-axis
The second rotation is by an angle $\beta$ about the fixed world Y-axis. Because the reference frame is the fixed world frame, we must *pre-multiply* our current rotation matrix by the new one:
$R_2 = R_y(\beta) \cdot R_1 = R_y(\beta)R_x(\alpha)$

- Rotation about World Z-axis
The third rotation is by an angle $\gamma$ about the fixed world Z-axis. Again, we *pre-multiply*:
$R_{final} = R_z(\gamma) \cdot R_2 = R_z(\gamma)R_y(\beta)R_x(\alpha)$

So, the final rotation matrix for the XYZ fixed-frame convention is $R_{XYZ} = R_z(\gamma)R_y(\beta)R_x(\alpha)$. Notice that the matrices are applied in the reverse order of the operations.

###  ZYX Local-Frame Rotations (Roll-Pitch-Yaw)


This is the more common one, it's called the _yaw,pitch,roll_ , yes and not _roll-pitch-yaw_ as the order in which we multiply is _yaw,pitch_ and _roll_. That is why it's called ZYX.

final rotation matrix for $R_{ZYX}$.

 - Rotation about Local Z-axis (Yaw)
The first rotation is by an angle $\alpha$ about the local Z-axis.
$R_1 = R_z(\alpha)$

- Rotation about *new* Local Y-axis (Pitch)
The second rotation is by an angle $\beta$ about the *new* local Y-axis (the one that has already been rotated by the yaw). Because the reference frame is the moving local frame, we must *post-multiply*:
$R_2 = R_1 \cdot R_y(\beta) = R_z(\alpha)R_y(\beta)$

- Rotation about *newest* Local X-axis (Roll)
The third rotation is by an angle $\gamma$ about the *newest* local X-axis. Again, we *post-multiply*:
$R_{final} = R_2 \cdot R_x(\gamma) = R_z(\alpha)R_y(\beta)R_x(\gamma)$

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
$p_W = R_{WB} \cdot p_B$

By convention in linear algebra, when a matrix operates on a vector, the matrix is written on the left. 
So, once my final $R$ is built, I *always* pre-multiply the point vector by it. 


</div></div>

#### Cayley's formula and constraints

I wasn't sure where to plug this in, so it might look a weird here, but so far we have been talking about Rotations, but this is just another form of representation.

$$R = (I_{3}- S )^{-1}(I_{3}+S)$$
Where S is a _skew-symmetric_ matrix ( $\therefore S=-S^{T}$). Along with this it's a good idea to remember the 6 constrains on the Rotation Matrix, norm of columns are 1, dot product between any two pairs of column is bound to be 0.
#### The Order of Rotations Matters

A critical detail is that the *order* of these rotations is extremely important. Matrix multiplication is *not commutative* (unless it's in 2D, this can be trivially understood) , which means changing the order of the rotations will result in a completely different final orientation (Craig, 2014, p. 23).

$$ R_Z R_Y R_X \neq R_X R_Y R_Z $$

Because of this, there are many different Euler angle conventions (e.g., ZYX, ZYZ, XYX, etc.), and it's crucial to know which one is being used.


#### Problem of Rotation around an Arbitrary axis


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/mobile-robotics/rotation-about-an-axis/" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">







<iframe src="/img/user/Mobile-Robotics/RotationAboutArbAxis.pdf" width="100%" height="600px" title="RotationAboutArbAxis.pdf" style="border:1px solid #ccc;"></iframe>


</div></div>


### The Problem with Euler Angles: Gimbal Lock

The biggest skill issue of  Euler angles is a phenomenon called *Gimbal Lock*. 
This is a critical failure case where you effectively lose one of your three degrees of rotational freedom.

<iframe src="/img/user/Mobile-Robotics/ch_gimballock.pdf" width="100%" height="600px" title="ch_gimballock.pdf" style="border:1px solid #ccc;"></iframe>

Gimbal lock occurs when the axes of two of the three gimbals align, this typically happens when the pitch angle is $\pm90^{\circ}$.

When the middle gimbal aligns with the outer gimbal, a rotation that should be performed by the outer gimbal can now be performed by the innermost gimbal instead. The two gimbals now rotate together, and you can no longer rotate around one of the original axes. An attempt to rotate around the yaw axis and the roll axis result in the same motion.

Because of this skill issue,  Daddy [[Mobile-Robotics/Quaternions\|Quaternions]] is here to  save you .
Videos by Grant Sanderson and Ben Eater are amazing including his [blog](https://eater.net/quaternions).
[_"Well, Papa, can you multiply triplets?"_ ](https://arxiv.org/abs/0810.5562)