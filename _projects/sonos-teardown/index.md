---
layout: post
title: Summer work at MIT CSAIL
description: Last summer at MIT CSAIL, I worked on using deep-learning to compress animal camera trap images. I fine-tuned models in PyTorch and built image processing pipelines in Python. I presented my work this December at NeurIPS 2025 (First-author! that is me ^) in front of 200 AI researchers. 
skills: 
  - Python / PyTorch
  - Model fine-tuning
  - Computer Vision
  - Dataset curation & Validation
  - Long-range radio

main-image: /neurips.png
---

## Abstract
Automatically triggered cameras, also known as camera traps, are an indispensable tool for studying animal ecology. Retrieving data from remote camera traps is an ongoing challenge. Especially when deployed in remote areas, bandwidth of wireless transmission is extremely limited. We propose a method for efficient deep-learning based image compression to address this challenge. Our method utilizes a state-of-the-art autoencoder network to compress images into a compact latent representation. By simply transmitting these latents we demonstrate that in many cases we can reduce bandwidth compared to JPEG while achieving the same or better reconstruction quality. We also propose a method for deployment-specific fine-tuning of our autoencoder architecture to specialize models at the edge to specific environmental conditions. Specifically, we fine-tune the encoder with LoRA and saliency-guided compression. Our experiments demonstrate that this approach reduces bandwidth requirements and improves reconstruction error, particularly at locations with abundant imagery.

## Full talk here:
[Wikipedia](https://www.climatechange.ai/papers/neurips2025/89)

