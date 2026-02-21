# Unofficial Replication of deepflash2

This repository contains an unofficial replication of the core methodology from the paper:  
**"Deep learning-enabled segmentation of ambiguous bioimages with deepflash2"** (Griebel et al., 2023).

This implementation focuses on the robust training and evaluation of model ensembles to handle bioimages with low signal-to-noise ratios and high inter-expert ambiguity.

---

## Key Implementation Features

### 1. Model Architecture & Ensemble
* **Backbone:** U-Net with a **ConvNext-Tiny** encoder, utilizing ImageNet pre-trained weights.
* **Initialization:** Decoder weights initialized using a **truncated normal distribution** ($\sigma=0.02$) to promote ensemble diversity.
* **Ensemble Strategy:** Training of **$M=5$ models** via K-Fold cross-validation to capture inter-model variance and enable uncertainty estimation.

### 2. Deepflash2 Training Policy
* **Two-Phase Fine-Tuning:** * **Phase 1:** One epoch with a frozen encoder to stabilize the decoder.
    * **Phase 2:** 25 epochs with all layers unfrozen to adapt to the specific bioimage domain.
* **Loss Function:** A hybrid objective combining **Cross-Entropy Loss** and **Dice Loss** to handle class imbalance.

### 3. Two-Step Evaluation Pipeline
* **Step 1: Absolute Performance:** Calculation of **Dice Score** (Semantic Segmentation) and **mAP** (Instance Segmentation) against the **STAPLE-estimated ground truth**.
* **Step 2: Relative Performance:** Comparison of model scores against the **Expert Ambiguity Range** (the inter-expert performance spread). This validates if the AI performs within the bounds of human variability.

---

## Visualization
The pipeline includes a visualization tool that randomly samples the test set to compare:
1. **Original Bioimage** (Input)
2. **STAPLE Ground Truth** (Expert Consensus)
3. **Deepflash2 Prediction** (Ensemble Output)



---

## Requirements
* `torch`
* `segmentation-models-pytorch`
* `opencv-python`
* `scikit-learn`
* `matplotlib`

---

## Citation
If you use this methodology, please cite the original paper:
> Griebel, M., Segebarth, D., Stein, N. et al. Deep learning-enabled segmentation of ambiguous bioimages with deepflash2. *Nat Commun* **14**, 1679 (2023). https://doi.org/10.1038/s41467-023-36960-9
