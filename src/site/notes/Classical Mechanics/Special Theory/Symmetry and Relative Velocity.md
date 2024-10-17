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

## Time Dilation Derivation

Consider a clock at rest in the $K$ system that produces signals at regular intervals $\Delta t = t(2) - t(1)$.

An observer in a moving system $K'$ measures a time interval $\Delta t'$ on the same clock using the Lorentz transformation:

$$
t' = \gamma \left( t - \frac{vx_1}{c^2} \right)
$$

For two events happening at the same position $x_1$ in the $K$ system, $x_1(2) = x_1(1)$. Therefore, the time interval measured in $K'$ is given by:

$$
\Delta t' = \gamma \Delta t = \frac{\Delta t}{\sqrt{1 - \frac{v^2}{c^2}}}
$$

Thus, the time interval appears longer in the moving frame $K'$, which explains why moving clocks run more slowly compared to clocks at rest in the observer's frame.

## Length Contraction Derivation

Consider a rod of length $L_0$ lying along the $x$-axis of an inertial frame $K$. The observer in system $K'$ moving with uniform speed $v$ along the $x$-axis measures the length of the rod as $L'$ in the moving system $K'$.

Using the Lorentz transformation:

$$
x'(2) - x'(1) = \frac{x_2(2) - x_1(1)}{\sqrt{1 - \frac{v^2}{c^2}}}
$$

where $x_2 - x_1 = L_0$. The length measured in the moving frame is given by:

$$
L' = L_0 \sqrt{1 - \frac{v^2}{c^2}}
$$

Thus, the moving rod appears shorter in the frame $K'$ compared to its proper length $L_0$ in the rest frame $K$.

---

### Relative Velocity in Lorentz Transformation

**Concept**: When a particle moves with velocity $u_x$ in the $K$ frame, we want to find its velocity $u'_x$ in the $K'$ frame, which moves with velocity $v$ relative to $K$.

#### Steps:
1. Consider a particle moving in the $x$-direction with velocity $u_x$ in frame $K$.
2. Using the Lorentz transformation for position and time:

$$ x' = \gamma (x - vt) $$
$$ t' = \gamma \left( t - \frac{x v}{c^2} \right) $$

3. To find the velocity in the $K'$ frame, differentiate $x'$ and $t'$ with respect to $t$:

$$ \frac{dx'}{dt} = \gamma \left( \frac{dx}{dt} - v \right) = \gamma ( u_x - v ) $$

$$ \frac{dt'}{dt} = \gamma \left( 1 - \frac{u_x v}{c^2} \right) $$

4. Therefore, the velocity $u'_x$ in the $K'$ frame is:

$$ u'_x = \frac{dx'}{dt'} = \frac{u_x - v}{1 - \frac{u_x v}{c^2}} $$

#### For $y$- and $z$-components:
- The transverse components of velocity transform differently:

$$ u'_y = \frac{u_y}{\gamma \left( 1 - \frac{u_x v}{c^2} \right)} $$
$$ u'_z = \frac{u_z}{\gamma \left( 1 - \frac{u_x v}{c^2} \right)} $$

#### Conclusion:
The velocity of the particle in the moving frame $K'$ is related to its velocity in $K$ by:

$$ u'_x = \frac{u_x - v}{1 - \frac{u_x v}{c^2}} $$

---


