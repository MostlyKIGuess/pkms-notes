---
{"dg-publish":true,"permalink":"/classical-mechanics/lagrangian-and-hamiltonian-dynamics/euler-lagrange-equation/"}
---

## Euler-Lagrange Equation

Good video to get intuition : 

<iframe width="560" height="315" src="https://www.youtube.com/embed/8UtnDaGHpq0?si=PFhOAJsOJedklmrL" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe> 




The **Euler-Lagrange equation** is a fundamental result of the calculus of variations and is derived from Hamilton’s principle of stationary action. It provides the equations of motion for a physical system once the **Lagrangian** is known.

### Euler-Lagrange Equation:

For each generalized coordinate $q_i$, the Euler-Lagrange equation is:

$$
\frac{d}{dt} \left( \frac{\partial L}{\partial \dot{q}_i} \right) - \frac{\partial L}{\partial q_i} = 0
$$


## Intuition:

Soo what I have come to realize is the way this equation works is magical, that somehow it follows the rules stated before and finds the path that does the least action which we already know from Hamiltonian Principle, also this video explains the math behind it, but I didn't really get it, maybe someday I will re-visit this and get it, here's the video I am talking about .

But yeah it makes sense why all of these work in the first place after understanding that essentially what we are gonna solve for is the path with least action and that is what happens in reality.


<iframe width="560" height="315" src="https://www.youtube.com/embed/V0wx0JBEgZc?si=yBOD7bqQIiQ34x5d" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>


### Derivation:

Starting from Hamilton’s principle, $\delta S = 0$, we have the action:

$$
S = \int_{t_1}^{t_2} L(q_i, \dot{q}_i, t) \, dt
$$

Taking the variation of $S$, we vary the path slightly and compute how the action changes. Setting the variation to zero yields the Euler-Lagrange equations, which are the equations of motion.

### Example:



For a simple harmonic oscillator, the Lagrangian is:

$$
L = \frac{1}{2} m \dot{x}^2 - \frac{1}{2} k x^2
$$

Applying the Euler-Lagrange equation:

$$
\frac{d}{dt} \left( \frac{\partial L}{\partial \dot{x}} \right) - \frac{\partial L}{\partial x} = 0
$$

yields the equation of motion:

$$
m \ddot{x} + k x = 0
$$


- Pendulum exampolo

![Pasted image 20241014200827.png](/img/user/Classical%20Mechanics/Lagrangian%20and%20Hamiltonian%20Dynamics/Pasted%20image%2020241014200827.png)




