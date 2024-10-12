---
{"dg-publish":true,"permalink":"/classical-mechanics/basic-concepts/conservation-of-energy-for-conservative-forces/"}
---



# Conservation of Energy for Conservative Forces

- **Total Energy**:
  The total mechanical energy $E$ of a system is given by the sum of its kinetic energy $T$ and potential energy $U$:
  $$
  E = T(\vec{v}(t)) + U(\vec{r}(t), t)
  $$

- **Time Derivative of Energy**:
  Taking the time derivative of the total energy:
  $$
  \frac{dE}{dt} = \frac{dT}{dt} + \frac{dU}{dt}
  $$

- **Kinetic Energy**:
  The kinetic energy $T$ is given by:
  $$
  T = \frac{1}{2} m v^2
  $$
  The time derivative of $T$ can be computed as follows:
  $$
  \frac{dT}{dt} = m \vec{v} \cdot \frac{d\vec{v}}{dt} = m \vec{v} \cdot \vec{a} = \vec{v} \cdot \vec{F}
  $$

- **Potential Energy**:
  The time derivative of the potential energy $U$ is given by:
  $$
  \frac{dU}{dt} = \sum_{\alpha} \frac{\partial U}{\partial x^\alpha} \frac{dx^\alpha}{dt} + \frac{\partial U}{\partial t}
  $$
  In vector notation, this can be expressed as:
  $$
  \frac{dU}{dt} = \vec{v} \cdot \nabla U + \frac{\partial U}{\partial t}
  $$

- **Total Energy Rate of Change**:
  Substituting the expressions for $\frac{dT}{dt}$ and $\frac{dU}{dt}$ into the equation for $\frac{dE}{dt}$:
  $$
  \frac{dE}{dt} = \vec{v} \cdot \vec{F} + \vec{v} \cdot \nabla U + \frac{\partial U}{\partial t}
  $$

- **For Conservative Forces**:
  For conservative forces, the force can be expressed as:
  $$
  \vec{F} = -\nabla U
  $$
  Substituting this into the energy rate of change gives:
  $$
  \frac{dE}{dt} = \vec{v} \cdot (-\nabla U) + \vec{v} \cdot \nabla U + \frac{\partial U}{\partial t}
  $$
  The first two terms cancel each other out, leading to:
  $$
  \frac{dE}{dt} = \frac{\partial U}{\partial t}
  $$

- **Constant Potential Energy**:
  If $U \equiv U(\vec{r})$ (i.e., potential energy does not explicitly depend on time), then:
  $$
  \frac{\partial U}{\partial t} = 0
  $$
  Therefore, we have:
  $$
  \frac{dE}{dt} = 0
  $$

- **Conclusion**:
  Since the time derivative of the total energy $E$ is zero, this means that $E$ is a constant:
  $$
  E = \text{constant}
  $$
  This shows the conservation of mechanical energy in a system where only conservative forces are acting.
