# AI-Based Restoration of Degraded Images

## Degradation-Aware Residual U-Net v2

A lightweight deep learning framework for restoring degraded grayscale
images using a degradation-aware residual encoder-decoder architecture.

------------------------------------------------------------------------

# 1. Problem Overview

Real-world images suffer from noise, blur, resolution degradation, and
loss of structural details. The objective is to recover high-quality
images while preserving edges, texture, and structural information.

------------------------------------------------------------------------

# 2. Proposed Solution

## Degradation-Aware Residual U-Net v2

The proposed model combines:

-   Degradation estimation
-   Residual learning
-   Encoder-decoder restoration
-   Skip connections
-   PixelShuffle based super-resolution

------------------------------------------------------------------------

# 3. Overall Workflow

    Degraded Input Image
              |
              v
    Degradation Estimator
              |
              v
    Degradation Embedding
              |
              v
    Residual U-Net Backbone
              |
              v
    Residual Refinement
              |
              v
    PixelShuffle Upscaling
              |
              v
    Restored High Resolution Image

------------------------------------------------------------------------

# 4. Architecture

The network contains:

## Degradation Estimator

Learns degradation-aware features from corrupted input images.

## Encoder

Extracts image structures and feature representations.

## Bottleneck

Captures deep restoration information.

## Decoder

Reconstructs fine details using skip connections.

## Residual Learning

The model predicts restoration corrections instead of directly
predicting the final image.

------------------------------------------------------------------------

# 5. Model Configuration

  Parameter             Value
  --------------------- ----------------------
  Architecture          DA Residual U-Net v2
  Base Channels         48
  Scale Factor          2x
  Embedding Dimension   16
  Input                 Grayscale LR image
  Output                Restored HR image

------------------------------------------------------------------------

# 6. Inference Pipeline

    Input Folder
          |
          v
    .npy Files
          |
          v
    run.py
          |
          v
    DA Residual U-Net v2
          |
          v
    Restored .npy Files
          |
          v
    Output Folder

------------------------------------------------------------------------

# 7. Input and Output Format

## Input

-   Format: `.npy`
-   Shape: `(H,W)`
-   Example: `128 x 128`

## Output

-   Format: `.npy`
-   Same filename as input
-   Restored resolution: `256 x 256`
-   Values range: `[0,1]`

------------------------------------------------------------------------

# 8. Deployment Structure

    submission/

    ├── run.py
    ├── requirements.txt
    ├── README.md
    └── models/
        └── da_resunet_sr_v2_best.pth

------------------------------------------------------------------------

# 9. Execution

Install dependencies:

``` bash
pip install -r requirements.txt
```

Run:

``` bash
python run.py <input-directory> <output-directory>
```

Example:

``` bash
python run.py data/Test_NoisyLR/NoisyLR output
```

------------------------------------------------------------------------

# 10. Validation

The pipeline guarantees:

-   One output for every input
-   Same filenames
-   Correct resolution
-   Grayscale output
-   Values clipped between 0 and 1
-   No NaN or Inf values
-   GPU compatible inference

------------------------------------------------------------------------

# 11. Performance

Validation on 400 images:

  Metric               Result
  -------------------- -------------------
  Images processed     400
  Resolution           128x128 → 256x256
  Inference time       \~22 seconds
  Average time/image   \~55 ms

------------------------------------------------------------------------

# Summary

Degradation-Aware Residual U-Net v2 provides an efficient restoration
system by combining degradation awareness and residual super-resolution
learning for robust image recovery.
