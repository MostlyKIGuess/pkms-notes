---
{"dg-publish":true,"permalink":"/mobile-robotics/nls/"}
---


### Non-Linear Least Squares (NLS)

While linear least squares provides a closed-form solution for fitting linear models, many real-world problems involve non-linear models. A common example is fitting a Gaussian function to a set of observations.

Given a set of $m$ observations $(x_i, y_i)$, we want to find the parameters $\beta = [a, \mu, \sigma]^T$ that best fit the non-linear model:

$$ y_i = f(x_i, \beta) = a \cdot \exp\left(-\frac{(x_i - \mu)^2}{2\sigma^2}\right) $$

Since this function is non-linear in its parameters, we cannot solve it directly. Instead, we must use an iterative optimization algorithm like the *Gauss-Newton* method.

#### The Gauss-Newton Algorithm

The core idea is to start with an initial guess for the parameters and iteratively refine it by solving a sequence of *linear* least-squares problems.

1.   Start with an initial estimate for the parameters, $\beta_0 = [a_0, \mu_0, \sigma_0]^T$.

2.   At each iteration, we linearize the non-linear function $f(x, \beta)$ around the current estimate $\beta_t$ using a first-order Taylor series expansion:

   $$ f(x, \beta) \approx f(x, \beta_t) + J(x, \beta_t) \delta\beta $$

   where $\delta\beta = \beta - \beta_t$ is the update step we want to find, and $J$ is the *Jacobian matrix* of $f$ with respect to the parameters $\beta$:

   $$ J = \begin{bmatrix} \frac{\partial f}{\partial a} & \frac{\partial f}{\partial \mu} & \frac{\partial f}{\partial \sigma} \end{bmatrix} $$

3.   We can now write the error (or residual) for each observation $i$ as:

   $$ e_i = y_i - f(x_i, \beta) \approx y_i - (f(x_i, \beta_t) + J_i \delta\beta) $$
   $$ (y_i - f(x_i, \beta_t)) \approx J_i \delta\beta $$

   This is a linear system of the form $\mathbf{y}_{new} = J \delta\beta$, where $\mathbf{y}_{new}$ is the vector of residuals. We can stack the equations for all $m$ observations:

   $$ \begin{bmatrix} y_1 - f(x_1, \beta_t) \\ \vdots \\ y_m - f(x_m, \beta_t) \end{bmatrix}_{m \times 1} = \begin{bmatrix} J_1 \\ \vdots \\ J_m \end{bmatrix}_{m \times 3} \begin{bmatrix} \delta a \\ \delta \mu \\ \delta \sigma \end{bmatrix}_{3 \times 1} $$

4.   We solve this linear least-squares problem for $\delta\beta$ using the normal equations:

   $$ \delta\beta = (J^T J)^{-1} J^T \mathbf{y}_{new} $$

5.   Update the parameter estimate for the next iteration:

   $$ \beta_{t+1} = \beta_t + \delta\beta $$

6.   Repeat steps 2-5, linearizing around the new estimate $\beta_{t+1}$, until the change in parameters $||\delta\beta||$ is below a small threshold $\epsilon$, or a maximum number of iterations is reached.

#### Levenberg-Marquardt (LM) Algorithm

The Gauss-Newton algorithm can be unstable if the Jacobian $J$ is ill-conditioned. The *Levenberg-Marquardt (LM)* algorithm is a more robust alternative that adds a damping factor $\lambda$ to the normal equations. This helps to control the step size and direction, making convergence more reliable.

$$ (J^T J + \lambda I) \delta\beta = J^T \mathbf{y}_{new} $$