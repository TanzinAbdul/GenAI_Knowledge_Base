# Diffusion Models (Score-Based Generative Models)

A comprehensive deep dive into the philosophy, thermodynamic analogies, and mathematical mechanics behind Diffusion Models—the state-of-the-art framework for high-fidelity continuous data generation.

---

## 🧭 Theoretical Foundations & History

### Shifting Away from Adversarial Games
While GANs produced sharp images, their adversarial nature made them notoriously unstable and prone to mode collapse. VAEs were stable but generated blurry results due to mathematical approximations in the ELBO loss. 

### The Thermodynamic Analogy (2015 / 2020)
Diffusion models approached generation from a completely different angle: **Non-equilibrium Thermodynamics**. Inspired by how gas molecules diffuse through space, Sohl-Dickstein et al. (2015) proposed destroying data structure iteratively and learning to reverse the process. 

However, the paradigm truly took off with Ho et al.’s 2020 paper, *["Denoising Diffusion Probabilistic Models" (DDPM)](https://arxiv.org/abs/2006.11239)*. They showed that by constraining the noise addition to a discrete Markov chain, a neural network could be trained exclusively to predict noise, yielding unprecedented generation quality that rapidly outpaced GANs.

```text
Forward Process (q):   Real Image ($x_0$) ───> Add Noise ───> Gaussian Noise ($x_T$)
Reverse Process ($p_\theta$):  Gaussian Noise ($x_T$) ───> Remove Noise ───> Synthetic Image ($x_0$)
