---
{"dg-publish":true,"permalink":"/mobile-robotics/co-ordinate-transforms/"}
---

# Coordinate Transformations


### Representing a Point in Space

A point $P$ in 3D space is represented using a vector (sometimes). Although, the coordinates of this vector depend entirely on the *coordinate frame* we are using as a reference.

Let's consider two different coordinate frames, {A} and {B}.

- A point $P$ described in frame {A} is denoted by the vector ${}^A P$.
- The same point $P$ described in frame {B} is denoted by the vector ${}^B P$.

${}^A P = \begin{bmatrix} p_x \\ p_y \\ p_z \end{bmatrix}$

Each vector is just a set of three coordinates that locate the point $P$ along the axes of that particular frame.

### Rigid Body Transformations

When we talk about transformations, we're often dealing with rigid bodies. This means the object itself doesn't bend or deform; it only changes its *position* (translation) and *orientation* (rotation) in space.

-   *Translation*: Describes the movement of a point from one location to another. We can represent this by adding a translation vector.
-   *Rotation*: Describes the change in orientation of a frame. This is more complex and requires a rotation matrix.

### Rotation Matrix

A rotation matrix describes the orientation of one coordinate frame with respect to another. ( This statement might not always hold btw)

The notation ${}_{B}^{A}R$ represents the rotation matrix that maps the coordinates of a vector from frame {B} to frame {A}.

$$ {}^A P = {}_{B}^{A}R \ {}^B P $$

This equation takes a point described in frame {B} (${}^B P$) and, by pre-multiplying it with the rotation matrix ${}_{B}^{A}R$, gives us the coordinates of that same point as described in frame {A} (${}^A P$).

#### Deriving the Rotation Matrix

How do we find the values inside ${}_{B}^{A}R$? The matrix is constructed from the *dot products* of the unit vectors of the two frames.

Let the unit vectors for frame {A} be $\hat{X}_A, \hat{Y}_A, \hat{Z}_A$ and for frame {B} be $\hat{X}_B, \hat{Y}_B, \hat{Z}_B$.

The matrix ${}_{B}^{A}R$ is a 3x3 matrix whose columns are the unit vectors of frame {B} expressed in frame {A}'s coordinates.

$$ {}_{B}^{A}R = \begin{bmatrix} {}^A\hat{X}_B & {}^A\hat{Y}_B & {}^A\hat{Z}_B \end{bmatrix} $$

Each of those column vectors can be found by projecting the {B} unit vectors onto the {A} axes:

# Coordinate Transformations

### Representing a Point in Space

A point $P$ in 3D space is represented using a vector. Although, the coordinates of this vector depend entirely on the *coordinate frame* we are using as a reference 

Let's consider two different coordinate frames, {A} and {B}.

- A point $P$ described in frame {A} is denoted by the vector ${}^A P$.
- The same point $P$ described in frame {B} is denoted by the vector ${}^B P$.

${}^A P = \begin{bmatrix} p_x \\ p_y \\ p_z \end{bmatrix}$

Each vector is just a set of three coordinates that locate the point $P$ along the axes of that particular frame.

### Rigid Body Transformations

When we talk about transformations, we're often dealing with rigid bodies. This means the object itself doesn't bend or deform; it only changes its *position* (translation) and *orientation* (rotation) in space. A transformation is a way of mapping points from one frame to another 

-   *Translation*: Describes the movement of a point from one location to another.
-   *Rotation*: Describes the change in orientation of a frame.

---

### Rotation Matrix

The primary tool for handling rotations is the *rotation matrix*. A rotation matrix describes the orientation of one coordinate frame with respect to another.

![Pasted image 20250919004914.png](/img/user/Mobile-Robotics/Pasted%20image%2020250919004914.png)


The notation ${}_{B}^{A}R$ represents the rotation matrix that maps the coordinates of a vector from frame {B} to frame {A}. This mapping is expressed as:

$$ {}^A P = {}_{B}^{A}R \ {}^B P $$
*(Craig, 2014, Eq. 2.13)*

This equation takes a point described in frame {B} (${}^B P$) and, by pre-multiplying it with the rotation matrix ${}_{B}^{A}R$, gives us the coordinates of that same point as described in frame {A} (${}^A P$).

#### Deriving the Rotation Matrix

The matrix ${}_{B}^{A}R$ is a 3x3 matrix whose columns are the unit vectors of frame {B} expressed in frame {A}'s coordinates 

Let the unit vectors for frame {A} be $\hat{X}_A, \hat{Y}_A, \hat{Z}_A$ and for frame {B} be $\hat{X}_B, \hat{Y}_B, \hat{Z}_B$.

$$ {}_{B}^{A}R = \begin{bmatrix} {}^A\hat{X}_B & {}^A\hat{Y}_B & {}^A\hat{Z}_B \end{bmatrix} $$

Each column vector is found by projecting the {B} unit vectors onto the {A} axes. This means the components of the rotation matrix are the dot products of the unit vectors of the two frames 

$$ {}_{B}^{A}R = \begin{bmatrix} \hat{X}_B \cdot \hat{X}_A & \hat{Y}_B \cdot \hat{X}_A & \hat{Z}_B \cdot \hat{X}_A \\ \hat{X}_B \cdot \hat{Y}_A & \hat{Y}_B \cdot \hat{Y}_A & \hat{Z}_B \cdot \hat{Y}_A \\ \hat{X}_B \cdot \hat{Z}_A & \hat{Y}_B \cdot \hat{Z}_A & \hat{Z}_B \cdot \hat{Z}_A \end{bmatrix} $$
*(Craig, 2014, Eq. 2.3)*

#### Properties of a Rotation Matrix

-   The matrix is *orthogonal*. Its columns (and rows) are mutually orthogonal unit vectors.
-   The *inverse* of a rotation matrix is equal to its *transpose*. 
    $$ ({}_{B}^{A}R)^{-1} = ({}_{B}^{A}R)^T = {}_{A}^{B}R $$
-   The *determinant* is always +1.


now go read chapter 2 JJ Craig