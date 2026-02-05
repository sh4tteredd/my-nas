# Immich on TrueNAS SCALE

This page describes how I set up **Immich** on TrueNAS SCALE using the official Docker app container.

Immich is a self-hosted photo and video backup solution with AI-powered image recognition and search. The setup leverages the **OpenVINO Machine Learning image** and GPU passthrough for improved performance.

---

## Setup Overview

The stack consists of the official Immich Docker container deployed through TrueNAS SCALE Apps.  

Key features enabled:

- **OpenVINO Machine Learning Image**  
  Selected in the **TrueNAS app container settings** under "Machine Learning Image Type".
- **GPU passthrough**  
  Enabled in the container settings to allow GPU acceleration for AI tasks.
- **CLIP model**  
  The model `ViT-L-16-SigLIP2-256__webli` is set as the CLIP model for image embeddings and similarity search.

---