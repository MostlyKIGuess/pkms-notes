---
{"dg-publish":true,"permalink":"/ml/logistic-regression/"}
---

### Classification and Logistic Regression

We can imagine classification as a special regression problem where we only regress to a set of binary values, 0 and 1. Sometimes, we use -1 and 1 notation as well. We call these the negative class and positive class, respectively.

However, if we apply a regression model here, it does not make sense that we predict any values other than 0 and 1. Therefore, we modify the hypothesis function to be:

$$ h_\theta(x) = g(\theta^T x) = \frac{1}{1 + \exp(-\theta^T x)} $$

It ranges from 0 to 1 as output. This intuitively explains why we call it regression since it outputs in a continuous space. However, the value indicates the probability of belonging to a certain class. So essentially, it is a classifier.

Let’s look at what happens when we take the derivative of the logistic function:

$$
\begin{aligned}
\frac{d}{dz}g(z) &= \frac{d}{dz} \left( \frac{1}{1 + \exp(-z)} \right) \\
&= \frac{\exp(-z)}{(1 + \exp(-z))^2} \\
&= \frac{1 + \exp(-z) - 1}{(1 + \exp(-z))^2} \\
&= \frac{1}{1 + \exp(-z)} \left( 1 - \frac{1}{1 + \exp(-z)} \right) \\
&= g(z) (1 - g(z))
\end{aligned}
$$


With this prior knowledge, the question is how are we supposed to find $\theta$. We know that least squares regression can be derived from the maximum likelihood algorithm, which is where we should start.

So I always had this doubt on why we even do maximum likelihood in the first place?
It involves thinking of the problem as a optimization / a search problem where we seek a set of parameters which fits the PDF from the data sample.
We define the parameter theta $\theta$ as everything will be parameterized by it.


We assume:

$$ P(y|x;\theta) = (h_\theta(x))^y (1 - h_\theta(x))^{1-y} $$

where $y$ should be either one or zero. Assuming that samples are i.i.d. (Independent and Identically distributed), one may ask is that a good assumption? Well it's decent enough and we can get away with it. [Good discussion ](https://stats.stackexchange.com/questions/82096/is-independent-and-identically-distributed-an-assumption-or-a-fact) to read if you wanna go in depth about it. 

we have the likelihood as:

$$ L(\theta) = \prod_{i=1}^m P(y^{(i)}|x^{(i)};\theta) = \prod_{i=1}^m (h_\theta(x^{(i)}))^{y^{(i)}} (1 - h_\theta(x^{(i)}))^{1-y^{(i)}} $$

Applying the log, we have:

$$ \log L(\theta) = \sum_{i=1}^m y^{(i)} \log h_\theta(x^{(i)}) + (1 - y^{(i)}) \log (1 - h_\theta(x^{(i)})) $$

Then, we can use gradient descent to optimize the likelihood. In updating, we should have:

$$ \theta = \theta + \alpha \nabla_\theta L(\theta) $$

Note we have a plus sign instead of a minus sign since we are finding the **maximum**, not the **minimum**. To find the derivative,

$$
\begin{aligned}
\frac{\partial}{\partial \theta_j} L(\theta) &= \left( \frac{y}{g(\theta^T x)} - \frac{1 - y}{1 - g(\theta^T x)} \right) \frac{\partial}{\partial \theta_j} g(\theta^T x) \\
&= \left( \frac{y}{g(\theta^T x)} - \frac{1 - y}{1 - g(\theta^T x)} \right) g(\theta^T x) (1 - g(\theta^T x)) \frac{\partial}{\partial \theta_j} (\theta^T x) \\
&= (y - h_\theta(x)) x_j
\end{aligned}
$$

From the first line to the second line, we use the derivative of the logistic function derived above. This gives us the update rule for each dimension of the feature vector. Although we have the same algorithm as LMS in this case, the hypothesis in this case is different. It is not surprising to have the same equation when we talk about the Generalized Linear Model.


## Newton's Method 

So if we think of the maximizing the likelihood function we can do that in a faster way that is if we take it's derivative then it shall be zero at the maxima and we can prove it's a concave function hence giving us the global maxima.

Here's a good visualization of what it does:


<iframe width="560" height="315" src="https://www.youtube.com/embed/x2KbdoxrQ6o?si=APFrgrVXc9zjiDRo" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>



Basically on each iteration after starting from a random point we take the derivative and land where the tangent meets the graph again on the x axis, and this happens in quadratic speed so we reach to our root faster.

Mathematically it's:

$$ f'(x_0) = f(x_0) \Delta \Rightarrow \Delta = \frac{f(x_0)}{f'(x_0)} $$

Derived from this idea, we can let $f(x) = L'(\theta)$. Following this approach, we can find the maximum of the objective function faster. For finding the minimum, the process is similar.

### Newton's Method for Vector-Valued $\theta$

If $\theta$ is vector-valued, we need to use the Hessian matrix in the updating. The Hessian matrix is the square matrix of second-order partial derivatives of the function. More details about the Hessian can be found in other posts. In short, to update, we have:

$$ \theta = \theta - H^{-1} \nabla_\theta L(\theta) $$

Although Newton's method converges quadratic-ally, each update is more costly than gradient descent . Read about it [here](https://www.baeldung.com/cs/gradient-descent-vs-newtons-gradient-descent) .

## Implementation:

### Gradient Descent:

```python
def fit(self, x, y, alpha=0.01, num_iters=1000):
        """Run gradient descent to minimize J(theta) for logistic regression.

        Args:
            x: Training example inputs. Shape (m, n).
            y: Training example labels. Shape (m,).
            alpha: The learning rate.
            num_iters: The number of iterations for gradient descent. 
        """
        m, n = x.shape
        self.theta = np.zeros(n)

        for _ in range(num_iters):
            h = 1 / (1 + np.exp(-x.dot(self.theta)))
            gradient = x.T.dot(h - y) / m
            self.theta -= alpha * gradient
```

### Newton's Method:

```python
def fit(self, x, y):
        """Run Newton's Method to minimize J(theta) for logistic regression.

        Args:
            x: Training example inputs. Shape (m, n).
            y: Training example labels. Shape (m,).
        """
        m,n = x.shape
        self.theta = np.zeros(n)
        e = 1e-5
        newtheta = np.zeros(n)
        while np.linalg.norm(newtheta - self.theta, ord=1) > e:
            self.theta = newtheta
            h = 1/(1+np.exp(-x.dot(self.theta)))
            gradient = x.T.dot(h-y)/m
            H = x.T.dot(np.diag(h*(1-h))).dot(x)/m
            newtheta = self.theta - np.linalg.inv(H).dot(gradient)
        self.theta = newtheta
```
