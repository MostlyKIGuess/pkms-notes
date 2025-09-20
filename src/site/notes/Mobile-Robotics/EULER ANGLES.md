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





![[RotationAboutArbAxis.pdf]]

====

</div></div>


### The Problem with Euler Angles: Gimbal Lock

The biggest skill issue of  Euler angles is a phenomenon called *Gimbal Lock*. 
This is a critical failure case where you effectively lose one of your three degrees of rotational freedom.

![[ch_gimballock.pdf]]

Gimbal lock occurs when the axes of two of the three gimbals align, this typically happens when the pitch angle is $\pm90^{\circ}$.

When the middle gimbal aligns with the outer gimbal, a rotation that should be performed by the outer gimbal can now be performed by the innermost gimbal instead. The two gimbals now rotate together, and you can no longer rotate around one of the original axes. An attempt to rotate around the yaw axis and the roll axis result in the same motion.

Because of this skill issue,  Daddy [[Mobile-Robotics/Quaternions\|Quaternions]] is here to  save you .
Videos by Grant Sanderson and Ben Eater are amazing including his [blog](https://eater.net/quaternions).
[_"Well, Papa, can you multiply triplets?"_ ](https://arxiv.org/abs/0810.5562)