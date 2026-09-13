# MOSAIC-T: Generative Mosaic Single-Cell Multi-Omics and Immune Repertoire Engine
> **🚧 Note: This project is currently UNDER CONSTRUCTION. Core modules are being actively developed and tested.**

<div align="center">
  <h3>Decoding T Cell Senescence Trajectories and Virtual Target Interventions</h3>
</div>

---

## 📖 Project Overview

**MOSAIC-T** is a deep generative learning framework designed to solve the "mosaic data missingness" problem in single-cell multi-omics. It is specifically tailored to analyze the immunosenescence and terminal exhaustion of T cells during aging.

While traditional single-omics studies are limited to observational correlations, MOSAIC-T aims to establish the causal chain: **Clonal Expansion $\\rightarrow$ Gene Regulation $\\rightarrow$ Phenotypic Exhaustion**. Our ultimate goal is not just to generate aesthetically pleasing clustering plots, but to build an *in-silico* digital twin simulator capable of modeling "cellular rejuvenation."

---

## 🧬 Biological Background & The "Mosaic Dilemma"

To fully understand why T cells become exhausted, we need three pieces of information simultaneously:
1.  **RNA**: Current physical state and transcriptional activity.
2.  **ATAC**: Chromatin accessibility and upstream epigenetic regulatory potential.
3.  **TCR**: Absolute clonal identity and antigen-driven history.

**The Computational Challenge:**
Real-world datasets exist as "data silos". Existing top-tier models (like totalVI, scVI) strongly rely on "fully paired" multi-omics data. Forcing intersections on partially overlapping datasets leads to massive feature loss or complete failure. 

**MOSAIC-T** asks: *How can we infer the non-linear joint distribution of RNA, ATAC, and TCR in a model that has never "seen" all three simultaneously?*

---

## ⚙️ Core Architecture & Algorithms

MOSAIC-T is built upon an **Anchor-Satellite Asynchronous VAE-GAN** architecture, featuring three groundbreaking computational pillars:

### 1. Mosaic Adversarial Alignment (马赛克对抗对齐)
*   **Encoders**: RNA acts as the absolute global anchor (`RnaEncoder`) to establish the baseline topological manifold. ATAC (`AtacEncoder`) and TCR (`TcrEncoder`) act as "satellites," dynamically awakened only when their respective data is input.
*   **Latent Game**: A Generator (G) maps diverse modality combinations to a unified latent space ($Z$), while a Discriminator (D) attempts to identify the original dataset source. Nash Equilibrium is reached when batch/modality effects are seamlessly erased.

### 2. Physical GRN Masking (GRN 物理级掩码)
*   Breaking the "black box" of deep learning. We introduce a `MaskedLinear` mechanism in the first layer of the RNA encoder.
*   Using prior Gene Regulatory Networks (Regulons) from SCENIC+, we perform a Hadamard product (hard pruning) to sever all biologically implausible TF-Gene weights. 
*   **Result**: Latent dimensions represent explicit Transcription Factor (TF) activity states, not just statistical principal components.

---

## 💊 In-silico Interventions (Digital Twin Application)

Our ultimate application is a virtual drug screening pipeline:
1.  **Age Manifold**: Apply age regularization to stretch a directional absolute time axis in the latent space.
2.  **Reverse Time**: Extract the latent vector of a terminally exhausted old cell ($Z_{old}$), apply a negative perturbation along the manifold gradient to generate $Z_{young}$.
3.  **Generative Decoding**: The multi-head decoder outputs the virtual "rejuvenated" RNA/ATAC feature matrices.
4.  **Target Screening**: Compare the generated differential expression profiles against databases like CMap to screen for anti-exhaustion small molecule drugs *in-silico*.

---

## 📂 Repository Structure (Planned)

```text
MOSAIC-T/
├── data/                  # Placeholder for data loaders and pre-processed mosaic datasets
├── models/
│   ├── __init__.py
│   ├── encoder.py         # RnaEncoder, AtacEncoder, TcrEncoder
│   ├── decoder.py         # Multi-head decoders (ZINB, BCE)
│   ├── layers.py          # Custom MaskedLinear layers for GRN priors
│   └── mosaic_t.py        # Main VAE-GAN architecture
├── utils/
│   ├── loss.py            # InfoNCE, ZINB, Adversarial, and Age Regularization losses
│   └── data_utils.py      # Asynchronous batching and routing for mosaic data
├── scripts/
│   ├── train.py           # Training loop with adversarial min-max game
│   └── infer.py           # In-silico perturbation and generation scripts
├── notebooks/             # Tutorials and exploratory data analysis
├── requirements.txt
└── README.md
