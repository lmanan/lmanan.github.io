---
layout: post
title: High Resolution Image Synthesis with Latent Diffusion Models 
categories: Computer Vision
tags:
mathjax: true
comments: true
---

(From [Rombach et al, 2022](https://arxiv.org/abs/2112.10752))
>"However, since these models typically operate directly in pixel space, optimization of powerful diffusion models (DMs) often consumes hundreds of GPU days and inference is expensive due to sequential evaluations. To enable DM training on limited computational resources while retaining their quality and flexibility, we apply them in the latent space of powerful pretrained autoencoders. In contrast to previous work, training diffusion models on such a representation allows for the first time to reach a near-optimal point between complexity reduction and detail preservation, greatly boosting visual fidelity."

Rombach et al argue that one can train diffusion models in a latent space, instead of directly training in the pixel space. This would enable faster training. They show that this preserves the quality and flexibility of vanilla diffusion models.

>"Our perceptual compression model is based on previous work and consists of an autoencoder trained by combination of a perceptual loss and a patch based adversarial objective. This ensures that the reconstructions are confined to the image manifold by enforcing local realism and avoids blurriness introduced by relying solely on pixel-space losses such as L2 or L1 objectives."

Given an image $x \in \mathcal{R}^{H \times W \times 3}$, the encoder $\mathcal{E}$ encodes $x$ into a latent representation $z = \mathcal{E}(x)$ and the decoder $D$ reconstructs the image from the latent, giving $\bar{x} = \mathcal{D}(z) = \mathcal{D}(\mathcal{E}(x))$.

The authors say that to avoid a high variance in the latent representations, they test two different kinds of regularizations: they refer to the first as \textit{KL reg} which imposes a penalty for deviation from the standrad normal. 


These are the parameters of the loss:
```
    lossconfig:
      target: modules.losses.LPIPSWithDiscriminator
      params:
        disc_start: 50001
        kl_weight: 0.000001
        disc_weight: 0.5
```

The authors mention in the supplementary section that the autoencoder is trained in an adversarial manner, similar to their previous [work](https://arxiv.org/abs/2012.09841), Esser and Rombach, 2021.

The patch based discriminator is optimized to differentiate original images from the reconstructions $\mathcal{D}(\mathcal{E}(x))$. To avoid arbitrarily scaled latent spaces, they regularize the latent to zero mean centered and have a small variance by introducing a regularizing loss term $L_{\text{reg}}$.

This regularization typically requires a small weighting term, for example $1e^{-6}$.
The full autoencoder loss $L_{AE}$ is a linear sum of 4 different contributions and looks as follows:

$$
L_{AE} = \text{min}_{\mathcal{E} \mathcal{D}} \text{max}_{\psi} \left( L_{\text{rec}} (x, \mathcal{D}\mathcal{E}(x)) - L_{\text{adv}}(\mathcal{D}\mathcal{E}(x)) + \log D_{\psi} (x) + L_{\text{reg}} (x; \mathcal{E}, \mathcal{D}) \right)
$$
