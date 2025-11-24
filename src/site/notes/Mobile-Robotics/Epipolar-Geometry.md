---
{"dg-publish":true,"permalink":"/mobile-robotics/epipolar-geometry/"}
---


#### What? Why?

Epipolar geometry describes the geometric relations in image pairs. It enables faster search for prediction of corresponding points between images by reducing the search space from 2D to 1D. 
Not only this but this 1D is special because in your image you might have multiple objects which are similar in nature and you can get a wrong correspondence by traversing the entire image. But with epipolar geometry you are reduced to searching in this one line which is somewhat geometrically consistent and your chances of getting wrong correspondences are reduced.


![Pasted image 20251122192049.png](/img/user/Mobile-Robotics/Pasted%20image%2020251122192049.png)

### Terms:
- **Epipolar Axis**: The line connecting the two projection centers. ( $\mathcal{B} = O'O''$).
- **Epipolar Plane**: The plane formed by a 3D point and the two projection centers. ($\epsilon = O'O''\mathcal{X}$)
- **Epipoles**: The projections of each camera's center onto the other's image plane. ($e' = P'\mathcal{B}$, $e'' = P''\mathcal{B}$)
- **Epipolar Lines**: The intersection of the epipolar plane with each image plane. ($l' = \epsilon P'$, $l'' = \epsilon P''$)


### In the Epipollar Plane..

Using a distortion-free lens,
- the projection centers $O'$ and $O''$ 
- the point $\mathcal{X}$ 
- the projections $x'$ and $x''$ are known. ( image points )
- the epipolar lines $l'$ and $l''$ 
- the epipoles $e'$ and $e''$
all lie in the same epipolar plane $\epsilon$.

### Epipolar Constraint

If the point lies on the image plane, then its corresponding point must lie on the corresponding epipolar line. This is called the epipolar constraint.

Also for any point x lying on a line l, the relation $x^Tl = 0$ holds true.
Therefore,
$$x'^Tl' = 0$$

and exploiting the coplanarity of the points X',X''.
$$x''^TF x' = 0$$

As $l'' = Fx'$, where F is the fundamental matrix.
The epipoles satisfy the relations:
$$e'^{T}Fx''= 0$$
$$e''^{T}F^Tx'= 0$$
Epipoles are the null space of the fundamental matrix.

$$null(F^{T)}= e'$$
$$null(F)= e''$$


They correspond to an eigenvalue of 0.
### Fundamental Matrix

- The fundamental matrix F is a 3x3 rank 2 matrix that relates corresponding points in stereo images. It encapsulates the epipolar geometry between two views. 
- F has 7 DOF (one DOF is lost due to the scale ambiguities of the homogeneous coordinate space and the other is lost due to the rank-deficiency constraints). Essential matrix E has 5 DOF.
- Defined upto any arbitrary scale factor.
- The fundamental matrix can be computed using a set of corresponding points between the two images. At least 8 point correspondences are needed to compute F using the 8-point algorithm.


$$x'Fx'' = 0$$
Where $x'$ and $x''$ are corresponding points in the two images.
So if we have n corresponding points, we can set up a system of linear equations to solve for the entries of F.
$$\begin{bmatrix} x_1'x_1'' & x_1'y_1'' & x_1' & y_1'x_1'' & y_1'y_1'' & y_1' & x_1'' & y_1'' & 1 \\ x_2'x_2'' & x_2'y_2'' & x_2' & y_2'x_2'' & y_2'y_2'' & y_2' & x_2'' & y_2'' & 1 \\  . &  .&  .&.  &.  &.  &.  &.  & 1 \\ x_n'x_n'' & x_n'y_n'' & x_n' & y_n'x_n'' & y_n'y_n'' & y_n' & x_n'' & y_n'' & 1 \end{bmatrix} \begin{bmatrix} f_{11} \\ f_{12} \\ f_{13} \\ f_{21} \\ f_{22} \\ f_{23} \\ f_{31} \\ f_{32} \\ f_{33} \end{bmatrix} = 0$$

$$Af = 0$$
Where A is the matrix of coefficients from the corresponding points, and f is the vector of entries of F. A is of size n x 9. and it's rank is 8.
Use normalization to improve numerical stability. Solve using SVD. Enforce rank-2 constraint by setting the smallest singular value to zero.

$$ F = UDV^T $$
if you want rank 3:
$$ F = U_{1}\sigma_{1}V_{1}^T + U_{2}\sigma_{2}V_{2}^T + U_{3}\sigma_{3}V_{3}^T $$

Enforce rank 2, or $min||F - F'||_F$ 
$$ F' = U_{1}\sigma_{1}V_{1}^T + U_{2}\sigma_{2}V_{2}^T $$

Where $||.||_F$ is the Frobenius norm.

Computation of F is done over n correspondence, to improve we can run RANSAC to filter out outliers.

Note: F is confuputed on the transformed image if $I' = TI$ then $F = T^{T}F'T$ 


other methods for finding  F: Normalized 8-point algorithm, 7-point algorithm, 5-point algorithm.

### Chirality and Pose Recovery

We get 4 motion pairs from $E = [t]_x R$
$$E = U\Sigma V^{T}, \Sigma = diag(1,1,0)$$

So 4 possible pairs would be:
$$R = U W V^{T}, t = +u_{3}$$
$$R = U W V^{T}, t = -u_{3}$$
$$R = U W^{T} V^{T}, t = +u_{3}$$
$$R = U W^{T} V^{T}, t = -u_{3}$$

Where $W = \begin{bmatrix} 0 & -1 & 0 \\ 1 & 0 & 0 \\ 0 & 0 & 1 \end{bmatrix}$
To determine the correct pair, we can use the chirality condition which states that the reconstructed 3D points must lie in front of both cameras. We can triangulate a point using each of the four motion pairs and check the depth values. The correct motion pair will yield positive depth values for the majority of points.



[[Mobile-Robotics/Triangulation\|Triangulation]]

