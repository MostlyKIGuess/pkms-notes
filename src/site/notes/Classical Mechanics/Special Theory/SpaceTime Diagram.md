---
{"dg-publish":true,"permalink":"/classical-mechanics/special-theory/space-time-diagram/"}
---



- Position is Realtive.
![Pasted image 20241013123734.png](/img/user/Classical%20Mechanics/Special%20Theory/Pasted%20image%2020241013123734.png)

# Rotation of Axes and Coordinate Transformation

- When the orientation of the axis is changed by an angle $\theta$, the new coordinates $(x', y')$ are related to the old coordinates $(x, y)$ using trigonometric functions of $\theta$ (cosine and sine).

- The transformation equations for the new coordinates $(x', y')$ are given by:
  $$
  x' = x \cos \theta + y \sin \theta
  $$
  $$
  y' = -x \sin \theta + y \cos \theta
  $$

## Matrix Form of the Transformation

- This transformation can be written more compactly using a matrix representation:
  
  $$
  \begin{pmatrix}
  x' \\
  y'
  \end{pmatrix}
  =
  \begin{pmatrix}
  \cos \theta & \sin \theta \\
  -\sin \theta & \cos \theta
  \end{pmatrix}
  \begin{pmatrix}
  x \\
  y
  \end{pmatrix}
  $$

---

### SpaceTime Diagram

![Pasted image 20241013124822.png](/img/user/Classical%20Mechanics/Special%20Theory/Pasted%20image%2020241013124822.png)


- Here comes the most important part, if speed of light can not change and something else has to change, then we need to do some kind of transform to understand relative measure. That is [[Classical Mechanics/Special Theory/Lorentz Transform\|Lorentz Transform]].