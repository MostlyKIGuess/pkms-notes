---
{"dg-publish":true,"permalink":"/ml/generalized-linear-models/"}
---

GLMs are here babby!!!.

We need to understand more about exponential family distributions before we get into GLMs.

$$
p(y;\eta) = b(y)\exp(\eta^T T(y) - a(\eta))
$$

Where all of the terms are:

- $y$ is data.
- $\eta$ is natural parameter.
- $T(y)$ is called sufficient statistics. 
- $b(y)$ is base measure.
- $a(\eta)$ is log-partition.

Quick observation tells us that these functions are only of certain parameters, so T(y) is a function only and only of y and not $\eta$. 

Some of the common exponential as these functions are:

##### Bernoulli:
$$
\begin{align}
p(y;\phi) &= \phi^y(1-\phi)^{1-y}\\
&= \exp(y\log\phi + (1-y)\log(1-\phi))\\
&= \exp\bigg(\bigg(\log\bigg(\frac{\phi}{1-\phi}\bigg)\bigg)y+\log(1-\phi)\bigg)
\end{align}
$$

$$\eta = \log(\phi/(1-\phi))$$
$$T(y) =y$$
$$a(\eta) = -\log(1-\phi) = \log(1+e^{\eta})$$
$$b(y)=1$$
##### Gaussian with variance = 1:
$$\sigma^{2}=1$$
$$p(y;\mu) = \frac{1}{\sqrt{2\pi}}\exp\bigg(-\frac{1}{2}y^2\bigg)\exp\bigg(\mu y - \frac{1}{2}\mu^2\bigg)$$

$$\eta = \mu$$
$$T(y)=y$$
$$a(\eta) = \mu^2/2 = \eta^2/2$$
$$
b(y) = (1/\sqrt{2\pi})\exp(-y^2/2)
$$

#### Overview of GLMs:
![Pasted image 20240612115714.png](/img/user/ML/Pasted%20image%2020240612115714.png)
![Pasted image 20240612115721.png](/img/user/ML/Pasted%20image%2020240612115721.png)

## Softmax Regression:


In a broader case, we can have multiple classes instead of the binary classes above. It is natural to model it as a Multinomial distribution, which also belongs to the exponential family that can be derived from the Generalized Linear Model (GLM).

In multinomial, we can define $\phi_1, \phi_2, \ldots, \phi_{k-1}$ to be the corresponding probabilities of $k-1$ classes. We do not need all $k$ classes since the last one is determined once the previous $k-1$ are set. So we can write $\phi_k = 1 - \sum_{i=1}^{k-1} \phi_i$.

We first define $T(y) \in \mathbb{R}^{k-1}$ and :

$$
T(1) = \begin{bmatrix} 1 \\ 0 \\ \vdots \\ 0 \end{bmatrix}, \quad T(2) = \begin{bmatrix} 0 \\ 1 \\ \vdots \\ 0 \end{bmatrix}, \ldots, \quad T(k) = \begin{bmatrix} 0 \\ 0 \\ \vdots \\ 0 \end{bmatrix}
$$

Note that for $T(k)$, we just have all zeros in the vector since the length of the vector is $k-1$. We let $T(y)_i$ define the $i$-th element in the vector. 

Now, we show the steps to derive the Multinomial distribution as an exponential family:

$$
\begin{aligned}
p(y; \phi) &= \phi_1^{[y=1]} \phi_2^{[y=2]} \ldots \phi_k^{[y=k]} \\
&= \phi_1^{[y=1]} \phi_2^{[y=2]} \ldots \phi_k^{1 - \sum_{i=1}^{k-1} [y=i]} \\
&= \phi_1^{T(y)_1} \phi_2^{T(y)_2} \ldots \phi_k^{1 - \sum_{i=1}^{k-1} T(y)_i} \\
&= \exp\left( T(y)_1 \log\left(\frac{\phi_1}{\phi_k}\right) + T(y)_2 \log\left(\frac{\phi_2}{\phi_k}\right) + \ldots + T(y)_{k-1} \log\left(\frac{\phi_{k-1}}{\phi_k}\right) + \log(\phi_k) \right) \\
&= b(y) \exp\left( \eta^T T(y) - a(\eta) \right)
\end{aligned}
$$

where

$$
\eta = \begin{bmatrix} \log\left(\frac{\phi_1}{\phi_k}\right) \\ \log\left(\frac{\phi_2}{\phi_k}\right) \\ \vdots \\ \log\left(\frac{\phi_{k-1}}{\phi_k}\right) \end{bmatrix}
$$

and

$$

a(\eta) = -\log(\phi_k), \quad b(y) = 1.

$$

This formulates the multinomial distribution as an exponential family. We can now have the link function as:

$$
\eta_i = \log\left(\frac{\phi_i}{\phi_k}\right)
$$

To get the response function, we need to invert the link function:

$$
\begin{aligned}
e^{\eta_i} &= \frac{\phi_i}{\phi_k} \\
\phi_k e^{\eta_i} &= \phi_i \\
\phi_k \sum_{i=1}^k e^{\eta_i} &= \sum_{i=1}^k \phi_i
\end{aligned}
$$

Then, we have the response function:

$$
\phi_i = \frac{e^{\eta_i}}{\sum_{j=1}^k e^{\eta_j}}
$$

This response function is called the softmax function.

From the assumption (3) in GLM, we know that $\eta_i = \theta_i^T x$ for $i = 1, 2, \ldots, k-1$ and $\theta_i \in \mathbb{R}^{n+1}$ is the parameter of our GLM model and $\theta_k$ is just 0 so that $\eta_k = 0$. Now, we have the model based on $x$:

$$
p(y=i|x; \theta) = \phi_i = \frac{e^{\theta_i^T x}}{\sum_{j=1}^k e^{\theta_j^T x}}

$$
This model is called softmax regression, which is a generalization of logistic regression. Thus, the hypothesis will be:

$$
h_\theta(x) = \mathbb{E}[T(y)|x; \theta] = \begin{bmatrix} \phi_1 \\ \phi_2 \\ \vdots \\ \phi_{k-1} \end{bmatrix} = \begin{bmatrix} \frac{e^{\theta_1^T x}}{\sum_{j=1}^k e^{\theta_j^T x}} \\ \frac{e^{\theta_2^T x}}{\sum_{j=1}^k e^{\theta_j^T x}} \\ \vdots \\ \frac{e^{\theta_{k-1}^T x}}{\sum_{j=1}^k e^{\theta_j^T x}} \end{bmatrix}
$$

Now, we need to fit $\theta$ such that we can maximize the log-likelihood. By definition, we can write it out:

$$
L(\theta) = \sum_{i=1}^m \log(p(y^{(i)}|x^{(i)}; \theta)) = \sum_{i=1}^m \log \left( \prod_{l=1}^k \left( \frac{e^{\theta_l^T x}}{\sum_{j=1}^k e^{\theta_j^T x}} \right)^{1\{y^{(i)}=l\}} \right)
$$

We can use gradient descent or Newton’s method to find the maximum of it.

**Note:** Logistic regression is a binary case of softmax regression. The sigmoid function is a binary case of the softmax function.

#### Overview simplified:

![Pasted image 20240612121923.png](/img/user/ML/Pasted%20image%2020240612121923.png)



[[ML/GDA\|GDA]]
