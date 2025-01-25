# NeRF Implementation for 3D Scene Reconstruction

This repository contains an implementation of Neural Radiance Fields (NeRF) for 3D scene reconstruction using PyTorch. The project demonstrates how NeRF can synthesize novel views of a scene by training on a set of input images and their camera poses.

## Features

- **NeRF Architecture:**
  - Positional encoding for 3D points and viewing directions.
  - Multi-layer perceptron (MLP) with skip connections.
  - Separate branches for density prediction and color prediction.
- **Two-Stage Rendering:**
  - Coarse model for sampling 3D points in space.
  - Fine model for importance sampling based on coarse model weights.
- **Custom Training Pipeline:**
  - Loss calculation using Mean Squared Error (MSE).
  - Learning rate decay with configurable parameters.

## Model Architecture

The NeRF model consists of:
- **Positional Encoding:** Encodes spatial and directional inputs into higher-dimensional spaces to capture fine details.
- **Coarse and Fine Models:** Two MLPs (multi-layer perceptrons) are used, one for coarse sampling and one for finer details.
- **Output:** Predicts color values (RGB) and volume densities for each 3D point.

### Key Model Components
- **Input:**
  - 3D points (`xs`).
  - Viewing directions (`ds`).
- **Layers:**
  - Positional encoding for points and directions.
  - MLP layers for feature extraction.
  - Final layers for density and color prediction.

### Training Details
- **Optimizer:** Adam optimizer with an initial learning rate of 5e-4.
- **Loss Function:** MSE loss for image reconstruction.
- **Batching:**
  - Training is performed on pixel batches sampled from images.
  - Chunking is used to handle memory constraints during inference.
- **Learning Rate Decay:** Configurable exponential decay for dynamic adjustment.

## Dataset
The implementation uses the Tiny NeRF dataset (`tiny_nerf_data.npz`), which contains:
- Training and testing images.
- Camera poses and focal lengths.

## Training Pipeline
1. **Dataset Preparation:**
   - Images and poses are preprocessed, and a specific index is held out for testing.
   - Remaining images and poses are used for training.
2. **Coarse Sampling:**
   - Sample 3D points along rays cast through pixels.
   - Compute radiance and density using the coarse model.
3. **Fine Sampling:**
   - Use weights from the coarse model to resample points along rays.
   - Compute refined radiance and density using the fine model.
4. **Loss Computation:**
   - Compare reconstructed images with ground truth using MSE loss.

## Reference
Implementation is based off original NeRF paper:                                   
> [NeRF: Representing Scenes as Neural Radiance Fields for View Synthesis
](https://arxiv.org/abs/2003.08934) 

