---
{"dg-publish":true,"permalink":"/ml/gda/"}
---

**Gaussian Discriminant Analysis**


I am still deriving the parameters post applying MLE.


$$

\begin{align}
\ell(\phi,\mu_0,\mu_1,\Sigma) &= \log \prod_{i=1}^m p(x^{(i)}, y^{(i)};\phi,\mu_0,\mu_1,\Sigma) \\
&= \log \prod_{i=1}^m p(x^{(i)}\lvert y^{(i)};\mu_0,\mu_1,\Sigma) p(y^{(i)};\phi)\\
&= \sum\limits_{i=1}^m \log p(x^{(i)}\lvert y^{(i)};\mu_0,\mu_1,\Sigma) p(y^{(i)};\phi)
\end{align}
$$

abstracting it we get:


$$
\begin{align}
\ell(\phi,\mu_k,\Sigma) &= \sum\limits_{i=1}^m \log p(x^{(i)}\lvert y^{(i)};\mu_k,\Sigma) p(y^{(i)};\phi)\\
&= \sum\limits_{i=1}^m \bigg[-\frac{n}{2}\log 2\pi-\frac{1}{2}\log\lvert\Sigma\rvert -\frac{1}{2}(x^i-\mu_k)^T\Sigma^{-1}(x^i-\mu_k) + y^i\log\phi+(1-y^i)\log(1-\phi)\bigg]\\
\end{align}
$$


Taking Derivative on that equation we get the following values:


$$
\phi = \frac{1}{m}\sum\limits_{i=1}^m \mathbb{1}\{y^{(i)}=1\}

$$

$$
\mu_k = \frac{\sum_{i=1}^m\mathbb{1}\{y^{(i)}=k\}x^{(i)}}{\sum_{i=1}^m\mathbb{1}\{y^{(i)}=k\}}
$$
$$
\Sigma = \frac{1}{m}\sum\limits_{i=1}^m (x^{(i)} - \mu_{y^{(i)}})(x^{(i)} - \mu_{y^{(i)}})^T

$$



### Posterior Probability in Gaussian Discriminant Analysis

We have the posterior probability of class 1 given the input $x$ as:

$$
p(y=1|x; \phi) = \frac{p(y=1|x; \phi) p(x|\mu_1, \Sigma)}{p(y=1|x; \phi) p(x|\mu_1, \Sigma) + p(y=0|x; \phi) p(x|\mu_0, \Sigma)}

$$
Given the class priors and the likelihoods, we can write this as:

$$
p(y=1|x; \phi) = \frac{\phi N(x|\mu_1, \Sigma)}{\phi N(x|\mu_1, \Sigma) + (1 - \phi) N(x|\mu_0, \Sigma)}
$$

Rewriting the above equation, we have:

$$
p(y=1|x; \phi) = \frac{1}{1 + \frac{(1 - \phi) N(x|\mu_0, \Sigma)}{\phi N(x|\mu_1, \Sigma)}}
$$

Since the Gaussian distribution is a member of the exponential family, we can eventually express the ratio in the denominator as $\exp(\theta^T x)$, where $\theta$ is a function of $\phi, \mu_0, \mu_1,$ and $\Sigma$.
