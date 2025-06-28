---
{"dg-publish":true,"permalink":"/research/optical-flow-model/"}
---

### Depth and Camera Pose
- Depth (d): The distance of a 3D point from the camera.
- Camera Pose (T): The position and orientation of the camera in the world (usually represented as a 4×4 transformation matrix, combining rotation and translation).

### Projection (π) and Backprojection (π⁻¹)
- Projection (π):Maps a 3D point (in camera coordinates) to 2D pixel coordinates.
- Backprojection (π⁻¹): Given a pixel (u, v) and its depth d, reconstructs the 3D point in camera coordinates.

### Step 1: Backprojection (π⁻¹)
Given a pixel (u, v) in image Iᵢ and its depth d, we compute its 3D coordinates in the camera frame of Iᵢ:
$$
P_i = \pi^{-1}(u, v, d)
$$
- For a pinhole camera:
  $$
  P_i = d \cdot \begin{bmatrix} (u - c_x)/f_x \\ (v - c_y)/f_y \\ 1 \end{bmatrix}
  $$
  where $(f_{x}, f_{\gamma})$ are focal lengths and $(c_{x},c_{\gamma})$ is the principal point.

### Step 2: Transform from Frame i to Frame j (Tᵢⱼ)
The 3D point $P_{i}$is transformed to the camera frame of $I_{j}$ using:
$$
T_{ij} = (T_{jw})^{-1} \cdot T_{iw}
$$
where:
- $T_{iW}$= camera-to-world transform for frame i,
- $T_{jW}$= camera-to-world transform for frame j,
- $T_{ij}$= relative transform from frame i to frame j.

The transformed point is:
$$
P_j = T_{ij} \cdot P_i
$$

### Step 3: Projection (π)
Project $P_j$ onto the image plane of $I_j$:
$$
(u', v') = \pi(P_j)
$$
For a pinhole camera:
$$
u' = f_x \cdot (P_{j,x} / P_{j,z}) + c_x, \quad v' = f_y \cdot (P_{j,y} / P_{j,z}) + c_y
$$

### Step 4: Compute Optical Flow
The flow vector for pixel (u, v) is:
$$
\hat{f}(u, v) = (u' - u, v' - v)
$$


# Intuition


- Static Scene Assumption: Only the camera moves; the world is static.
- Flow Depends on Depth: Closer points (small d) induce larger flow (more motion in the image), while distant points (large d) induce smaller flow.
- Flow Depends on Camera Motion: The direction and magnitude of flow depend on how the camera moves (rotation vs. translation).

## Example
### **Given:**
- Two images 
- Camera intrinsics: fx = fy = 500, cx = cy = 320.
- Camera poses:
  - Tᵢw = identity (camera i is at world origin).
  - Tⱼw = translation of (0.1, 0, 0) (camera j moves right by 0.1m).
- Pixel (u, v) = (320, 320) ( Assume center of the image).
- Depth d = 1m.

### **Step 1: Backprojection**
$$
P_i = \pi^{-1}(320, 320, 1) = \begin{bmatrix} 0 \\ 0 \\ 1 \end{bmatrix}
$$

### **Step 2: Transform to Frame j**
$$
T_{ij} = (T_{jw})^{-1} \cdot T_{iw} = \begin{bmatrix} 1 & 0 & 0 & -0.1 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix}
$$
$$
P_j = T_{ij} \cdot P_i = \begin{bmatrix} -0.1 \\ 0 \\ 1 \end{bmatrix}
$$

### **Step 3: Projection**
$$
u' = 500 \cdot (-0.1 / 1) + 320 = 270
$$
$$
v' = 500 \cdot (0 / 1) + 320 = 320
$$

### **Step 4: Flow Vector**
$$
\hat{f}(320, 320) = (270 - 320, 320 - 320) = (-50, 0)
$$

#### **Interpretation:**
- The pixel moved left by 50 pixels because the camera moved right.
- If depth d were larger (e.g., 10m), the flow would be smaller (-5, 0).

### Dense Flow Field 
Instead of computing flow for one pixel, we compute it for all pixels (u, v) in a grid (h × w), given a dense depth map d. This gives a flow field:
$$
h_{ij}(T_i^w, T_j^w, d_i) = \pi(T_{ij} \cdot \pi^{-1}(u, v, d_i)) - \begin{bmatrix} u \\ v \end{bmatrix}
$$

#### Why is this useful?
- For visual odometry: Estimate camera motion by comparing predicted flow (from depth and pose) with observed flow.
- For depth estimation: If camera motion is known, we can estimate depth from flow.
