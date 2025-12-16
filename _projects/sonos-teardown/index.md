---
layout: post
title: Summer work at MIT CSAIL
description: Last summer at MIT CSAIL, I worked on using deep-learning to compress animal camera trap images. Camera traps are often deployed in remote areas where wireless transmission is extremely limited. My image compression algorithm can greatly compress the images and make them small enough to be easily transmitted with limited bandwidth. I presented my work this December at NeurIPS 2025 (First-author! That is me ^) in front of 200 AI researchers. I was selected as a spotlight (10% acceptance rate)!

skills: 
  - Python / PyTorch
  - Model fine-tuning
  - Computer Vision
  - Dataset curation & Validation
  - Long-range radio

main-image: /neurips.png
---

## Full talk here:
[Recording](https://www.climatechange.ai/papers/neurips2025/89)


## Embedded/System Engineering

{% include image-gallery.html images="rpi.png" height="300" %} 
For engineering, I ran my model on a Raspberry Pi integrated with an RPi camera and a long-range radio. I was able to:
1. Deploy my machine learning model on the Raspberry Pi
2. Take images using the camera
3. Compress images on-device and transmit them over long-range radio across 400 m
4. Recover the images on my laptop with minimal quality loss

I used Python, OpenCV, PyTorch, and embedded systems programming extensively in this project.

## Abstract
Automatically triggered cameras, also known as camera traps, are an indispensable tool for studying animal ecology. Retrieving data from remote camera traps is an ongoing challenge. Especially when deployed in remote areas, bandwidth of wireless transmission is extremely limited. We propose a method for efficient deep-learning based image compression to address this challenge. Our method utilizes a state-of-the-art autoencoder network to compress images into a compact latent representation. By simply transmitting these latents we demonstrate that in many cases we can reduce bandwidth compared to JPEG while achieving the same or better reconstruction quality. We also propose a method for deployment-specific fine-tuning of our autoencoder architecture to specialize models at the edge to specific environmental conditions. Specifically, we fine-tune the encoder with LoRA and saliency-guided compression. Our experiments demonstrate that this approach reduces bandwidth requirements and improves reconstruction error, particularly at locations with abundant imagery.


