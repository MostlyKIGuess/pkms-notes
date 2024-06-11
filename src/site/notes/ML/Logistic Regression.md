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
