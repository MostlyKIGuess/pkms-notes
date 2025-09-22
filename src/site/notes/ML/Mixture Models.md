---
{"dg-publish":true,"permalink":"/ml/mixture-models/"}
---


> In statistics, a mixture model is a probabilistic model for representing the presence of subpopulations within an overall population, without requiring that an observed data set should identify the sub-population to which an individual observation belongs. - _Wikipedia_
### Gaussian Mixture Models (GMMs)

GMM: Superposition of K Gaussian densities of the following form is mixture of Gaussians.

$$p(x) = \sum_{k=1}^{K} \pi_k \mathcal{N}(x | \mu_k, \Sigma_k)$$
Mixture coefficients: $\pi_k$ (prior probability of a point belonging to cluster $k$)
We get the bottom relation by integrating both sides on the above equation.
$$\sum_{k=1}^{K} \pi_k = 1$$

Likelihood Function:
$$L(\theta | x) = \prod_{i=1}^{n} p(x_i) = \prod_{i=1}^{n} \sum_{k=1}^{K} \pi_k \mathcal{N}(x_i | \mu_k, \Sigma_k)$$
where $\theta = \{\pi_1, ..., \pi_K, \mu_1, ..., \mu_K, \Sigma_1, ..., \Sigma_K \}$.

Log Likelihood:
$$\log L(\theta | x) = \sum_{i=1}^{n} \log \sum_{k=1}^{K} \pi_k \mathcal{N}(x_i | \mu_k, \Sigma_k)$$

This is a hard problem.

##### E-step 
- compute soft assignment (posterior probabilities)
$$\gamma_{ik} = p(k | x_i) = \frac{\pi_k \mathcal{N}(x_i | \mu_k, \Sigma_k)}{\sum_{j=1}^{K} \pi_j \mathcal{N}(x_i | \mu_j, \Sigma_j)}$$
#### M-step
- re-estimate parameters
$$N_k = \sum_{i=1}^{n} \gamma_{ik}$$
$$\mu_k^{\text{new}} = \frac{1}{N_k} \sum_{i=1}^{n} \gamma_{ik} x_i$$
$$\Sigma_k^{\text{new}} = \frac{1}{N_k} \sum_{i=1}^{n} \gamma_{ik} (x_i - \mu_k^{\text{new}})(x_i - \mu_k^{\text{new}})^T$$
$$\pi_k^{\text{new}} = \frac{N_k}{n}$$


#### Advantages
- Flexibility, K-Means just assumes that clusters are spherical.
- Uncertainty estimation is an added benefit of soft assignment.
- Density Estimate, can help in identify outliers, anomalies (low $p(x)$) 
- Useful as a Generative Model
- Less sensitive to initialization than K-Means

#### How to choose K?

- Pick the 'k' which generates maximum likelihood for a 'hold out' set.
- Cross-validation, Information Criteria (AIC, BIC)

