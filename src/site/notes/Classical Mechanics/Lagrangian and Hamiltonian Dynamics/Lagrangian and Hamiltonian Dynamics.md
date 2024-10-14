---
{"dg-publish":true,"permalink":"/classical-mechanics/lagrangian-and-hamiltonian-dynamics/lagrangian-and-hamiltonian-dynamics/"}
---


# Hamilton's Principle

 The path of a physical system between two states is the one that minimizes the difference between kinetic and potential energies.

![Pasted image 20241014115408.png](/img/user/Classical%20Mechanics/Lagrangian%20and%20Hamiltonian%20Dynamics/Pasted%20image%2020241014115408.png)


### Action:
The action is defined as the integral of the **Lagrangian** $L$, which is a function of the generalized coordinates $q_i$, their time derivatives $\dot{q}_i$, and time $t$:

$$
S = \int_{t_1}^{t_2} L(q_i, \dot{q}_i, t) \, dt
$$

Here, $L(q_i, \dot{q}_i, t)$ is the **Lagrangian**, typically given by the difference between the kinetic energy $T$ and potential energy $U$:

$$
L = T - U
$$

Hamilton’s principle can be expressed as:

$$
\delta S = 0
$$

This means that the actual path a system takes is the one that makes the **variation of the action zero.**

To derive the equations of motion from this, we compute the variation $\delta S$ and set it to zero:

$$
\delta S = \delta \int_{t_1}^{t_2} L(q_i, \dot{q}_i, t) \, dt = 0
$$

This leads to the **Euler-Lagrange equations**:

$$
\frac{d}{dt} \left( \frac{\partial L}{\partial \dot{q}_i} \right) - \frac{\partial L}{\partial q_i} = 0
$$

The Euler-Lagrange equations are the equations of motion that follow from Hamilton's principle.

- lookout [[Classical Mechanics/Lagrangian and Hamiltonian Dynamics/Euler-Lagrange Equation\|Euler-Lagrange Equation]] .
- and [[Classical Mechanics/Lagrangian and Hamiltonian Dynamics/Generalized Coordinates\|Generalized Coordinates]].




