---
{"dg-publish":true,"permalink":"/inverse-graphics/image-formation/"}
---


### Image Projection Basics

The core equations show how 3D points collapse to 2D:
```
y = y/z  
x = f·(x/z)  
y = f·(y/z)
```

**What this really means**:  
When you take a photo, camera makes the 3D world into a 2D image ( pixels) (W2C) . The division by `z` is important ( this is basic stereo) - it's why objects farther away appear smaller. The focal length `f` acts like a "zoom factor". 

**Practical implications**:  
- A tree 10m away at (x=2, z=10) projects to the same position as a toy car at (x=0.2, z=1)  
- This non-linearity causes perspective distortion (think railroad tracks converging)

### Coordinate Systems

1. **Camera Frame**: Raw 3D coordinates relative to the lens  
2. **Normalized Image Frame**: "Pure" coordinates before pixel conversion  
3. **Pixel Coordinates**: Where it actually lands on your sensor (e.g., (255,255))

**The conversion magic happens via matrix `K`**:
```
[ f   0   p_x ]
[ 0   f   p_y ]
[ 0   0    1  ]
```
Where `p_x`, `p_y` is where the optical axis hits the sensor (usually near image center). The zeros assume perfect square pixels - real cameras often have slight skew. ( these px and py are offset that we require , it makes the 0,0 from bottom to where the offset is if that makes sense)

### Triangulation 

**The core idea**:  
If you see the same object in two photos taken from different positions, you can reconstruct its 3D location like your eyes do with stereo vision.

**Mathematically**:  
For each camera, we:
1. Backproject a ray using `K⁻¹` to undo the camera's distortion  
2. Convert to world coordinates using `[R|t]`  
3. Find where rays from all cameras intersect (in practice, their closest point due to noise)

**Why least squares?**  
Coz it might just not intersect, like lmao wtf.
Because real measurements are noisy. We minimize:
```
∑||πᵢ(X) - xᵢ||²
```
This averages out errors across all observations.

---

### Epipolar Geometry 

For any point in Image 1, its match in Image 2 must lie along a specific line (the epipolar line). This reduces stereo matching from a 2D search to 1D. 

**Key components**:  
- **Epipole**: Where the other camera's center appears in your image  
- **Fundamental Matrix (F)**: The algebraic embodiment of this relationship  

**Why F matters**:  
1. `Fx₁` directly gives you the search line in Image 2  
2. It encodes the cameras' relative pose compactly  
3. Enables depth estimation from parallax  

**Derivation intuition**:  
The coplanarity condition `x₂ᵀ(t × Rx₁) = 0` says: "The vector to X, the baseline between cameras, and the vector to x₂ must all lie in the same plane."

### Implementation Notes

**For triangulation**:  
- Wider camera baselines improve depth accuracy but make matching harder  
- Acute angles between rays reduce precision  

**For fundamental matrix**:  
- Normalize coordinates first (subtract mean, scale by 1/std dev)  
- Use RANSAC to handle mismatched points  
- Refine with Levenberg-Marquardt non-linear optimization  

**Common pitfalls**:  
- Forgetting lens distortion before computing F  
- Assuming perfect calibration (always estimate K if possible)  
- Ignoring degenerate configurations (e.g., all points on a plane)  


These foundations power:  
- **Structure from Motion (SfM)**: Your phone's 3D scanning  
- **Visual SLAM**: Autonomous car navigation  
- **NeRF**: Novel view synthesis via implicit neural representations  

The math is absurd bruhh, but it's literally how your phone creates portrait mode bokeh effects by estimating depth from multiple viewpoints, which is so insane man