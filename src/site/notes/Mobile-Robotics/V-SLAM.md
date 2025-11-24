---
{"dg-publish":true,"permalink":"/mobile-robotics/v-slam/"}
---

# V-SLAM: The Architecture

To build a SLAM system that doesn't drift into infinity after 10 meters, we must separate the concerns into a Frontend (Fast, Local) and a Backend (Slow, Global).

### 1. The Frontend: Visual Odometry
The goal here is *speed*. We need to track the robot's motion relative to the *immediate* previous state.

* **Scan-to-Scan:** Align Frame $t$ to Frame $t-1$.
    * *Pros:* Fast.
    * *Cons:* Drifts instantly. $O(N)$ error accumulation.
* **Scan-to-Local-Map (The Standard):** Align Frame $t$ to a local map created by frames $t-k \dots t-1$.
    * *Pros:* More stable constraints.
    * *Cons:* Requires managing a local point cloud (voxel grid or k-d tree).

The Heuristic:
Don't process every frame. If the robot hasn't moved, don't update the map.
Define **Keyframes**:
$$ || t_{curr} - t_{last} || > \delta_{dist} \quad \lor \quad || R_{curr} R_{last}^T || > \delta_{angle} $$

### 2. The Backend: Global Optimization

The goal here is *consistency*. We accept that the frontend drifts. The backend's job is to "snap" the trajectory back when we recognize a place we've been before.

This is modeled as a *Factor Graph* (or Pose Graph).
* **Nodes:** The Keyframe poses.
* **Edges:** The constraints.
    * *Odometry Edges:* "I moved $x$ meters forward from Node A to Node B" (From Frontend).
    * *Loop Closure Edges:* "Node A looks exactly like Node Z" (From Place Recognition).

### 3. Loop Closure Detection

How do we know Node 1000 is the same place as Node 50?
1.  Geometric check:Is $|| P_{1000} - P_{50} || < \text{radius}$? (Only works if drift is low).
2.  Appearance check:Use descriptors (FPFH, BoW, NetVLAD).
3.  Verification:Run ICP between Scan 1000 and Scan 50. If fitness > threshold, add a constraint edge.

### 4. The Algorithm Loop

1.  **Receive Scan** $S_t$.
2.  **Predict Pose** $T_{pred}$ using constant velocity model.
3.  **Frontend:** ICP align $S_t$ to $S_{local\_map}$ starting at $T_{pred}$. Get $T_{est}$.
4.  **Keyframe Check:** Did we move enough?
    * **No:** Update current pose, wait for next scan.
    * **Yes:**
        1.  Add Node $T_{est}$ to Pose Graph.
        2.  Add Odometry Edge from $T_{last\_key}$.
        3.  **Loop Detection:** Search for old nodes near $T_{est}$.
            * If match found: Add Loop Edge.
            * **Trigger Optimizer:** Solve [[Mobile-Robotics/Graph-SLAM ( Pose Graph Optimization )\|Graph-SLAM ( Pose Graph Optimization )]].
        4.  Update Global Map.

