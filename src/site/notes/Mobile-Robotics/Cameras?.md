---
{"dg-publish":true,"permalink":"/mobile-robotics/cameras/"}
---

# Perspective Projection

![Pasted image 20251121180231.png](/img/user/Mobile-Robotics/Pasted%20image%2020251121180231.png)
![Pasted image 20251121180243.png](/img/user/Mobile-Robotics/Pasted%20image%2020251121180243.png)
![Pasted image 20251121180255.png](/img/user/Mobile-Robotics/Pasted%20image%2020251121180255.png)



Q) Determine `K` for  a digital camera with Image Size 640 x 480 and horizontal field of view equal to $90^{\circ}$ , assume principal point in the center of image and squared pixels. What if fov?
- So because principal point is in center $c_{x}= \frac{640}{2} = 320$ and and $c_{y} = \frac{480}{2} = 240$ . 
- And because of:![Pasted image 20251121180719.png](/img/user/Mobile-Robotics/Pasted%20image%2020251121180719.png)
- We also have that $f_{x} = f_{y} = f$ because of squared pixels.
- $tan\left(\frac{90}{2}\right)= \frac{W}{2f}$ , so $f = \frac{W}{2tan(45)} = \frac{640}{2} = 320$ 
- Vertical fov can be calculated as: ${fov}_{v} = 2tan^{-1}\left(\frac{H}{2f}\right) = 2tan^{-1}\left(\frac{480}{640}\right) \approx 73.74^{\circ}$

#### The Proof: Why Parallel Lines Meet

**To Prove:** Show that two parallel lines in the 3D world project to lines in the image that intersect at a single specific pixel (the Vanishing Point

1. Define Parallel Lines in 3D
Two lines are parallel if they have the **same direction vector** but different starting points.
Let's define the direction vector as $\vec{V} = [l, m, n]$.

* **Line A:** Starts at point $A_0 = (X_{a}, Y_{a}, Z_{a})$.
Equation: $\begin{bmatrix} X(s) \\ Y(s) \\ Z(s) \end{bmatrix} = \begin{bmatrix} X_{a} + s \cdot l \\ Y_{a} + s \cdot m \\ Z_{a} + s \cdot n \end{bmatrix}$ 

* **Line B:** Starts at a different point $B_0 = (X_{b}, Y_{b}, Z_{b})$.
Equation: $\begin{bmatrix} X(s) \\ Y(s) \\ Z(s) \end{bmatrix} = \begin{bmatrix} X_{b} + s \cdot l \\ Y_{b} + s \cdot m \\ Z_{b} + s \cdot n \end{bmatrix}$

Here, $s$ is the distance parameter. As $s \to \infty$, we move infinitely far away along the line.

2. Apply the Camera Projection

Recall the basic pinhole projection equations (assuming standard camera frame where focal length is $f$):
$$u = f \frac{X}{Z}, \quad v = f \frac{Y}{Z}$$

Let's substitute our line equations into this projection.

For Line A:
$$u_A(s) = f \frac{X_{a} + s \cdot l}{Z_{a} + s \cdot n}$$
$$v_A(s) = f \frac{Y_{a} + s \cdot m}{Z_{a} + s \cdot n}$$

For Line B:
$$u_B(s) = f \frac{X_{b} + s \cdot l}{Z_{b} + s \cdot n}$$
$$v_B(s) = f \frac{Y_{b} + s \cdot m}{Z_{b} + s \cdot n}$$

3. Take the Limit (The "Vanishing" part)

For $u_A$:
$$\lim_{s \to \infty} u_A(s) = \lim_{s \to \infty} f \frac{X_{a}/s + l}{Z_{a}/s + n} = f \frac{0 + l}{0 + n} = f \frac{l}{n}$$

For $v_A$:
$$\lim_{s \to \infty} v_A(s) = \lim_{s \to \infty} f \frac{Y_{a}/s + m}{Z_{a}/s + n} = f \frac{0 + m}{0 + n} = f \frac{m}{n}$$

same for Line B:
$$\lim_{s \to \infty} u_B(s) = f \frac{l}{n}$$
$$\lim_{s \to \infty} v_B(s) = f \frac{m}{n}$$


Both lines converge to the exact same pixel coordinate:
$$\mathbf{p}_{\infty} = \left( f \frac{l}{n}, f \frac{m}{n} \right)$$

This point $\mathbf{p}_{\infty}$ depends **only** on the direction vector $(l, m, n)$ and the focal length $f$. It does **not** depend on the starting points $A_0$ or $B_0$.

Therefore, all lines with direction $(l, m, n)$ will intersect at this specific point on the image plane. This point is Vanishing Point.


# THE Projection Equation

![Pasted image 20251121184552.png](/img/user/Mobile-Robotics/Pasted%20image%2020251121184552.png)

$$\lambda  \begin{bmatrix} u \\ v \\ 1 \end{bmatrix} = K [R|t] \begin{bmatrix} X_w \\ Y_w \\ Z_w \\ 1 \end{bmatrix} $$

or in Camera Coordinates:
$$\lambda  \begin{bmatrix} u \\ v \\ 1 \end{bmatrix} = K \begin{bmatrix} X_c \\ Y_c \\ Z_c  \end{bmatrix} $$

Henceforth $[R|t]$ are the Extrinsic Parameters and $K$ is the Intrinsic Matrix.

### Radial Distortion 

For most lens it would just be quadratic but higher order terms can be introduced:
$$\begin{bmatrix} u_d \\ v_d \end{bmatrix} = (1 + k_{1}r^{2} + k_{2}r^{4}+ k_{3}r^{6}) \begin{bmatrix} u-u_{0} \\ v -v_{0} \end{bmatrix} + \begin{bmatrix} u_{0} \\ v_{0} \end{bmatrix}   $$


# Camera Caliberation

[[Mobile-Robotics/Camera Caliberation\|Camera Caliberation]]


# Camera Resection

[[Mobile-Robotics/Pose Problem -- Camera Resection\|Pose Problem -- Camera Resection]]


# Epipolar Geometry

[[Mobile-Robotics/Epipolar-Geometry\|Epipolar-Geometry]]

