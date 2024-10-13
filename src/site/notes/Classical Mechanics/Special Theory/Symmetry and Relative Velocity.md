---
{"dg-publish":true,"permalink":"/classical-mechanics/special-theory/symmetry-and-relative-velocity/"}
---



### Time Dilation & Length Contraction (Symmetry)

- **Symmetry of Observers**:
  - A moving and stationary observer both perceive the other’s clock and rod differently.
  - For clocks:  
    - A stationary observer will measure a **moving clock** to have a longer time interval between consecutive ticks (this is **time dilation**).
    - A moving observer will measure a **stationary clock** to have longer time intervals between ticks.
  - For rods:  
    - A moving observer will measure a **stationary rod** to be **shorter** than its proper (rest) length (**length contraction**).
    - A stationary observer will measure a **moving rod** to be shorter than its rest length.


## Time Dilation and Length Contraction Derivation:

### Time Dilation Derivation

**Concept**: Time dilation describes how a moving clock is observed to tick more slowly compared to a stationary clock.

1. Consider a clock at rest in frame $K$ that ticks every $\Delta t$ seconds.
2. In the stationary frame $K'$, the clock moves with velocity $v$.
3. During one tick of the clock, light travels a distance $d$ to reflect off a mirror and return:
   - In the rest frame $K$, the light travels a total distance of $d + d = 2d$.
   - In frame $K'$, the time taken for the light to travel is $t' = \frac{2d}{c}$.

4. The distances are related by the Lorentz transformation:
   $$ d = vt $$

5. The time interval in the stationary frame $K$ is:
   $$ d = c \Delta t $$

6. Substituting these into the distance equation gives:
   $$ 2d = c \Delta t = v t + vt $$

7. Solving for $t$ yields:
   $$ t = \frac{2d}{c} = \frac{2v \Delta t}{c} $$

8. Now, substituting back:
   $$ \Delta t' = \frac{2d}{c} = \frac{2vt}{c} = \frac{\Delta t}{\sqrt{1 - \frac{v^2}{c^2}}} $$

9. Hence, the time dilation formula is:
   $$ \Delta t' = \frac{\Delta t}{\sqrt{1 - \frac{v^2}{c^2}}} $$

---

### Length Contraction Derivation

**Concept**: Length contraction states that the length of an object moving relative to an observer is measured to be shorter than its proper length.

1. Consider a rod of proper length $L_0$ at rest in frame $K$.
2. When the rod moves with velocity $v$ relative to an observer in frame $K'$, its endpoints are measured simultaneously in $K'$.

3. Let $x_1$ and $x_2$ be the positions of the endpoints of the rod in frame $K$:
   $$ L_0 = x_2 - x_1 $$

4. The Lorentz transformation gives the positions in the moving frame $K'$:
   $$ x_1' = \gamma (x_1 - vt_1) $$
   $$ x_2' = \gamma (x_2 - vt_2) $$

5. For simultaneous measurements in $K'$ ($t_1 = t_2$), we have:
   $$ L' = x_2' - x_1' = \gamma (x_2 - vt) - \gamma (x_1 - vt) $$

6. This simplifies to:
   $$ L' = \gamma (x_2 - x_1) = \gamma L_0 $$

7. Since $\gamma = \frac{1}{\sqrt{1 - \frac{v^2}{c^2}}}$, the length contraction formula becomes:
   $$ L' = L_0 \sqrt{1 - \frac{v^2}{c^2}} $$

---

### Derivation of Relative Velocity in the Lorentz Transformation

Let's consider a particle with position $ x(t) = u_x t $ moving with velocity $ u_x $ in the $ x $-direction, and apply the Lorentz transformation to calculate its velocity in the $ K' $ frame.

The Lorentz transformations are given by:
$$ x' = \gamma (x - v t) $$
$$ t' = \gamma \left( t - \frac{x v}{c^2} \right) $$

Now, differentiating both equations with respect to time:
$$ \frac{dx'}{dt} = \gamma \left( \frac{dx}{dt} - v \right) = \gamma ( u_x - v ) $$

For time:
$$ \frac{dt'}{dt} = \gamma \left( 1 - \frac{u_x v}{c^2} \right) $$

Thus, the relative velocity in the $ K' $ frame becomes:
$$ u'_x = \frac{u_x - v}{1 - \frac{u_x v}{c^2}} $$

Similarly, for the $ y $- and $ z $-components of velocity:
$$ u'_y = \frac{u_y}{\gamma \left( 1 - \frac{u_x v}{c^2} \right)} $$
$$ u'_z = \frac{u_z}{\gamma \left( 1 - \frac{u_x v}{c^2} \right)} $$

Where:
- $u_x, u_y, u_z$ are the velocity components of the particle in the stationary frame $K$.
- $u'_x, u'_y, u'_z$ are the velocity components in the moving frame $K'$.
- $v$ is the relative velocity between frames.
- $\gamma = \frac{1}{\sqrt{1 - \frac{v^2}{c^2}}}$ is the Lorentz factor.

---
