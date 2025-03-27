---
Tags: 
Created: 2025-03-02 23:44:35
---
(Links:: [[Machine Learning]])
# Generator networkse
# Generative adversarial networks
# Autoencoders
# Variational autoencoders
# Social Impact 3
Here are structured Obsidian notes for each chapter of [Lecture 9: Deep Generative Models](https://mlvu.github.io/generative/), incorporating MathJax for mathematical expressions, callouts for emphasis, and embedded images to illustrate key concepts.

---

# 🧠 **Generator Networks**

## Overview

Generator networks are models designed to produce data samples that resemble those in a given datasetThese networks are trained to capture the underlying data distribution, enabling the generation of new, similar data points

## Neural Networks as Probabilistic Models

While traditional neural networks are deterministic, they can be adapted to represent probabilistic models

- **Output Interpretation**:The network's output is treated as parameters of a probability distribution. For example, in binary classification, a neural network with a sigmoid-activated output node produces a value interpreted as the probability of the positive class, effectively parameterizing a Bernoulli distribution
    
- **Regression Example**:In regression tasks, the network predicts a target value y^\hat{y}. By interpreting this prediction as the mean μ\mu of a normal distribution N(μ,σ2)\mathcal{N}(\mu, \sigma^2), maximizing the likelihood corresponds to minimizing the least squares loss function
    

> [!note] **Training Process**
> 
> - **Log-Likelihood Maximization**:The network is trained by maximizing the log-likelihood of the data under the model, which serves as the loss function
> - **Gradient Descent**:Backpropagation and gradient descent are utilized to optimize the network parameters

**Illustration**: ![Neural Network as a Probabilistic Model]([https://mlvu.github.io/generative/images/neural_network_probabilistic_model.png](https://mlvu.github.io/generative/images/neural_network_probabilistic_model.png%EE%88%84%EE%88%86)

---

# 🥊 Generative Adversarial Networks (GANs)

## Overview

Generative Adversarial Networks (GANs) consist of two neural networks—the **Generator** and the **Discriminator**—competing in a zero-sum gam. The generator aims to produce data indistinguishable from real data, while the discriminator seeks to differentiate between real and generated dat.

## Training Process

1. **Generator** Takes random noise zz as input and generates data samples ( G(z) .
    
2. **Discriminator** Receives both real data xx and generated data G(z)G(z), and outputs a probability D(x)D(x) indicating the likelihood that the input is rea.
    
3. **Objective Functions**:
    
    - **Discriminator** Maximizes the log-probability of correctly classifying real and generated dat: max⁡DEx∼pdata[log⁡D(x)]+Ez∼pz[log⁡(1−D(G(z)))]\max_D \mathbb{E}_{x \sim p_{\text{data}}}[\log D(x)] + \mathbb{E}_{z \sim p_z}[\log (1 - D(G(z)))]
    - **Generator** Minimizes the log-probability that the discriminator correctly identifies generated data as fak: min⁡GEz∼pz[log⁡(1−D(G(z)))]\min_G \mathbb{E}_{z \sim p_z}[\log (1 - D(G(z)))]

**Illustration**: ![GAN Architecture](https://mlvu.github.io/generative/images/gan_architecture.pn)

---

# 🔄 Autoencoders

## Overview

Autoencoders are neural networks trained to reconstruct their input daa They consist of two main componens:
- _Encoder_: Maps input data xx to a latent representation ($z$).
- _Decoder_: Reconstructs the input data from the latent representation ( z ).

## Training Objective

The network is trained to minimize the reconstruction error, typically measured by the mean squared error between the input xx and the reconstructed output ( $\hat{x}$ ): $$\min \| x - \hat{x} \|^2$$

**Illustration**

![Autoencoder Structure](https://mlvu.github.io/generative/images/autoencoder_structure.pg)

---

# 🌌 Variational Autoencoders (VAEs)

## Overview
Variational Autoencoders (VAEs) are probabilistic generative models that combine neural networks with latent variable modl. They aim to model the data distribution and generate new data samples by learning a latent space representation.

## Key Components
- **Encoder (Inference Model)**: Estimates the posterior distribution $q(z|x)$ of the latent variables given the input data.
- **Decoder (Generative Model)**: Defines the likelihood $p(x|z)$ of the data given the latent variabes.

## Objective Function
- The VAE is trained to maximize the Evidence Lower Bound (ELO) $\mathbb{E}_{q(z|x)}[\log p(x|z)] - \text{KL}(q(z|x) | p(z)$ where $KL\text{KL}$ denotes the Kullback-Leibler divergence, encouraging the approximate posterior $q(z|x)$ to be close to the prior $p(z)$.

_Illustration_:

![VAE Architecture](https://mlvu.github.io/generative/images/vae_architecture.ng)

---

# 🌍 Social Impact 3
## Overvew
The development and application of deep generative models have significant social implications, both positive and negative.
## Positive Impacts
- **Creative Industries**: Generative models can assist artists and designers by providing new tools for creative expression, such as generating art, music, or design prototypes.
- **Data Augmentation**: In fields like healthcare, generative models can create synthetic data to augment limited datasets, potentially improving model training and performance.
## Negative Impacts
- **Deepfake**: The ability to generate realistic images and videos can be misused to create deceptive content, leading to misinformation and

---
References: https://mlvu.github.io/generative