---
{"dg-publish":true,"permalink":"/ml/linear-regression/"}
---

note:-well so this implementation is linearly weighted regression.

A linear regression problem can be modeled as:

$$ h(x) = \sum_{i=0}^n \theta_i x_i = \theta^T x $$

We have $\theta_0$ for the bias, and sometimes it is called the intercept term. Imagine that you try to regress for a line in the 2D domain; the intercept term basically determines where the line crosses the y-axis. $\theta$ is called the parameter which we want to learn from the training data.

To learn it, we also define the cost function which we are trying to minimize:

$$ J(\theta) = \frac{1}{2} \sum_{i=1}^m (h_\theta(x^{(i)}) - y^{(i)})^2 $$

### Locally Weighted Linear Regression

In the regression method discussed above, we treat the cost resulting from training samples equally in the process. However, this might not be proper since some outliers should be assigned less weight. We implement this idea by assigning weights to each sample with respect to the querying point. For example, such a weight can be:

$$ w^{(i)} = \exp\left( -\frac{(x^{(i)} - x)^2}{2r^2} \right) $$

Although this is similar to a Gaussian function, it has nothing to do with it. Here, $x$ is the querying point. We need to keep all the training data for new predictions.


### Deriving the Normal Equation for Linear Regression 

The cost function for linear regression is given by: 

$$ J(\theta) = \frac{1}{2} (X\theta - \vec{y})^T (X\theta - \vec{y}) = \frac{1}{2} \sum_{i=1}^m (h_\theta(x^{(i)}) - y^{(i)})^2 $$

At this point, we need to find the derivative of $J$ with respect to $\theta$. From the properties of the trace, we know that: $$ \nabla_A \text{tr}(A^T B A C) = B^T A^T C^T + B A C $$ We also know the trace of a scalar is itself. Then: 

$$ \begin{aligned} \nabla_\theta J(\theta) &= \nabla_\theta \frac{1}{2} (X\theta - \vec{y})^T (X\theta - \vec{y}) \\ &= \frac{1}{2} \nabla_\theta \text{tr}(\theta^T X^T X \theta - \theta^T X^T \vec{y} - \vec{y}^T X \theta + \vec{y}^T \vec{y}) \\ &= \frac{1}{2} \nabla_\theta (\text{tr}(\theta^T X^T X \theta) - 2 \text{tr}(\vec{y}^T X \theta)) \\ &= \frac{1}{2} \nabla_\theta ( \theta^T X^T X \theta + \theta^T X^T X \theta - 2 X^T \vec{y}) \\ &= X^T X \theta - X^T \vec{y} \end{aligned} $$
Here, the second line results from applying $a = \text{tr}(a)$ where $a$ is a scalar. The third line comes from the fact that
(1) the derivative with respect to $\theta$ of $\vec{y}^T \vec{y}$ is zero; 
(2) $\text{tr}(A + B) = \text{tr}(A) + \text{tr}(B)$; and 
(3) $- \theta^T X^T \vec{y} - \vec{y}^T X \theta = -2 \vec{y}^T X \theta$. 
The fourth line uses (1) the property right above where $A^T = \theta, B = B^T = X^T X, C = I$; and (2) $\nabla_A \text{tr}(A B) = B^T$. We set the derivative to zero to obtain the normal equation: $$ X^T X \theta = X^T \vec{y} $$ Then, we update the parameter as: $$ \theta = (X^T X)^{-1} X^T \vec{y} $$

### Locally Weighted Linear Regression

In the regression method discussed above, we treat the cost resulting from training samples equally in the process. However, this might not be proper since some outliers should be assigned less weight. We implement this idea by assigning weights to each sample with respect to the querying point. For example, such a weight can be:

$$ w^{(i)} = \exp\left( -\frac{(x^{(i)} - x)^2}{2r^2} \right) $$

Although this is similar to a Gaussian function, it has nothing to do with it. Here, $x$ is the querying point. We need to keep all the training data for new predictions.

### Deriving the Normal Equation for Weighted Linear Regression

The weighted cost function for linear regression is given by:

$$ J(\theta) = \frac{1}{2} \sum_{i=1}^m w^{(i)} (h_\theta(x^{(i)}) - y^{(i)})^2 $$

In matrix form, this becomes:

$$ J(\theta) = \frac{1}{2} (X\theta - \vec{y})^T W (X\theta - \vec{y}) $$

where $W$ is a diagonal matrix with the weights $w^{(i)}$ on the diagonal.

To find the gradient of $J$ with respect to $\theta$, we proceed as follows:

$$
\begin{aligned}
\nabla_\theta J(\theta) &= \nabla_\theta \left( \frac{1}{2} (X\theta - \vec{y})^T W (X\theta - \vec{y}) \right) \\
&= \nabla_\theta \left( \frac{1}{2} \left( \theta^T X^T W X \theta - \theta^T X^T W \vec{y} - \vec{y}^T W X \theta + \vec{y}^T W \vec{y} \right) \right) \\
&= \frac{1}{2} \nabla_\theta \left( \theta^T X^T W X \theta - 2 \theta^T X^T W \vec{y} + \vec{y}^T W \vec{y} \right) \\
&= \frac{1}{2} \left( 2 X^T W X \theta - 2 X^T W \vec{y} \right) \\
&= X^T W X \theta - X^T W \vec{y}
\end{aligned}
$$

We set the gradient to zero to find the optimal $\theta$:

$$ \nabla_\theta J(\theta) = 0 \implies X^T W X \theta = X^T W \vec{y} $$

Solving for $\theta$:

$$ \theta = (X^T W X)^{-1} X^T W \vec{y} $$

In summary, by adding the weights $w_i$ into the cost function, we modify the normal equation to:

$$ \theta = (X^T W X)^{-1} X^T W \vec{y} $$

This takes into account the weights for each training example in the calculation of the regression parameters.




## Implementation:


```python
import numpy as np
import util

from linear_model import LinearModel


def main(train_path, eval_path, pred_path):
    """Problem 1(b): Logistic regression with Newton's Method.

    Args:
        train_path: Path to CSV file containing dataset for training.
        eval_path: Path to CSV file containing dataset for evaluation.
        pred_path: Path to save predictions.
    """
    x_train, y_train = util.load_dataset(train_path, add_intercept=True)

    

    # Train logistic regression
    model = LogisticRegression(eps=1e-5)
    model.fit(x_train, y_train)

    # Plot data and decision boundary
    util.plot(x_train, y_train, model.theta, 'output/p01b_{}.png'.format(pred_path[-5]))

    # Save predictions
    x_eval, y_eval = util.load_dataset(eval_path, add_intercept=True)
    y_pred = model.predict(x_eval)
    np.savetxt(pred_path, y_pred > 0.5, fmt='%d')

    


class LogisticRegression(LinearModel):
    """Logistic regression with Newton's Method as the solver."""

    def fit(self, x, y):
        """Run Newton's Method to minimize J(theta) for logistic regression.

        Args:
            x: Training example inputs. Shape (m, n).
            y: Training example labels. Shape (m,).
        """
        
        # Init theta
        m, n = x.shape
        self.theta = np.zeros(n)

        # Newton's method
        while True:
            # Save old theta
            theta_old = np.copy(self.theta)
            
            # Compute Hessian Matrix
            h_x = 1 / (1 + np.exp(-x.dot(self.theta)))
            H = (x.T * h_x * (1 - h_x)).dot(x) / m
            gradient_J_theta = x.T.dot(h_x - y) / m

            # Updata theta
            self.theta -= np.linalg.inv(H).dot(gradient_J_theta)

            # End training
            if np.linalg.norm(self.theta-theta_old, ord=1) < self.eps:
                break

        

    def predict(self, x):
        """Make a prediction given new inputs x.

        Args:
            x: Inputs of shape (m, n).

        Returns:
            Outputs of shape (m,).
        """
        
        
        return 1 / (1 + np.exp(-x.dot(self.theta)))

    

```
****
