# Video Generation Models

## Overview

Video generation models extend image generation by creating a sequence of coherent frames that form a realistic video. Unlike image generation, where only spatial consistency is required, video generation must maintain both **spatial** and **temporal consistency**. This means that objects, lighting, camera motion, and scene dynamics should remain realistic throughout the entire video.

Most modern video generation models, such as **Sora**, **Veo**, **Kling**, **HunyuanVideo**, and **Movie Gen**, are based on **latent diffusion models** combined with **Transformer architectures**.

---

# Why Video Generation is Difficult

Generating a single image requires producing one realistic frame.

Generating a video requires producing **hundreds or even thousands of correlated frames**.

For example, a 10-second video at 24 FPS contains:

```text
10 × 24 = 240 frames
```

Each frame must satisfy:

* Match the text prompt
* Preserve object identity
* Maintain consistent lighting
* Follow realistic physics
* Produce smooth motion
* Keep camera movement coherent

Humans are extremely sensitive to inconsistencies between consecutive frames. Even a single incorrect frame can break the illusion of realism.

---

# From Image Diffusion to Video Diffusion

Image diffusion starts with random noise and gradually removes noise until a realistic image appears.

```text
Random Noise
      │
      ▼
Progressive Denoising
      │
      ▼
Generated Image
```

Video diffusion follows the same principle but operates on an entire **video latent representation** instead of a single image.

```text
Random Video Noise
        │
        ▼
Progressive Denoising
        │
        ▼
Generated Video
```

Instead of representing data as:

```text
Height × Width × Channels
```

video generation represents it as:

```text
Time × Height × Width × Channels
```

The additional **Time** dimension allows the model to learn motion and temporal relationships.

---

# Video Latent Representation

Modern models do **not** generate videos directly in pixel space because doing so would require enormous computational resources.

Instead, videos are compressed into a lower-dimensional latent space using a Video VAE (Variational Autoencoder).

```text
Input Video
      │
      ▼
Video Encoder (VAE)
      │
      ▼
Compressed Video Latent
      │
      ▼
Diffusion Transformer
      │
      ▼
Video Decoder (VAE)
      │
      ▼
Generated Video
```

This significantly reduces computational cost while preserving important visual information.

---

# Space-Time Attention

Image generation only needs to understand spatial relationships between image patches.

Video generation must understand both:

* Spatial relationships (within a frame)
* Temporal relationships (across frames)

Instead of attending only to neighboring image patches, the Transformer can attend across both **space and time**.

For example:

```text
Frame 1     Frame 2     Frame 3

   Ball  →    Ball  →    Ball
```

The model learns that the object appearing in multiple frames is the same object moving through time.

This mechanism is commonly referred to as **Space-Time Attention**.

---

# Video Generation Pipeline

A simplified pipeline of modern video generation models is shown below.

```text
Text Prompt
      │
      ▼
Text Encoder
      │
      ▼
Prompt Embedding
      │
      ▼
Random Video Latent
      │
      ▼
Diffusion Transformer
      │
      ▼
Denoised Video Latent
      │
      ▼
Video Decoder
      │
      ▼
Generated Video
```

---

# Is the Video Generated Frame by Frame?

A common misconception is that diffusion models generate videos one frame after another.

Most modern diffusion-based video models **do not** generate videos autoregressively.

Instead, they start with an entire noisy video latent.

For example, a 16-frame video begins as:

```text
Frame 1   Noise
Frame 2   Noise
Frame 3   Noise
...
Frame16   Noise
```

During every diffusion step, **all frames are refined simultaneously**.

The Transformer exchanges information across all frames using attention, allowing motion and object identity to remain consistent.

This differs from Large Language Models (LLMs), which generate one token at a time.

---

# Why Long Videos Are Challenging

A one-minute video at 24 FPS contains:

```text
60 × 24 = 1440 frames
```

Processing all frames simultaneously would require an enormous amount of GPU memory because Transformer attention grows rapidly with the number of tokens.

Therefore, current video generation models usually generate videos in **short clips**, such as:

* 16 frames
* 32 frames
* 64 frames

Longer videos are created by:

* Generating overlapping clips
* Extending previously generated clips
* Stitching clips together smoothly

This makes long-duration generation computationally feasible.

---

# How Does the Model Know the Video Length?

The model does **not** decide the duration itself.

The desired duration is specified before generation.

For example,

```text
8 seconds × 24 FPS = 192 frames
```

The system creates an initial random latent tensor with exactly 192 frames.

The diffusion model then denoises this tensor until a realistic video emerges.

The duration is therefore determined by the input latent tensor's shape rather than being predicted during generation.

---

# Diffusion vs Autoregressive Video Generation

Diffusion-based models:

* Start with an entire noisy video
* Denoise all frames simultaneously
* Produce high visual quality
* Excellent temporal consistency

Autoregressive video models:

* Generate one frame (or latent token) at a time
* Predict future frames from previous frames
* Similar to how LLMs predict the next token

Both approaches are active areas of research.

---

# Connection to World Models

Modern video generation is gradually evolving toward **World Models**.

Instead of only generating pixels, future models aim to understand:

* Physical interactions
* Object permanence
* Cause and effect
* Long-term motion
* Environment dynamics

This allows AI systems not only to generate realistic videos but also to simulate how the world changes over time.

---

# Key Takeaways

* Video generation extends image diffusion by adding a temporal dimension.
* Modern systems operate in compressed latent space rather than pixel space.
* Space-Time Attention enables consistent motion and object identity.
* Diffusion models refine all frames simultaneously instead of generating one frame at a time.
* Long videos are typically produced as multiple shorter clips because of computational limitations.
* The desired video duration is specified before generation by defining the size of the initial latent tensor.
* Future research is moving toward world models that learn the dynamics of the physical world instead of only generating realistic pixels.
