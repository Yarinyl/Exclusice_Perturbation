
# 🛡️ Universal Exclusive Adversarial Perturbation (UEAP)

This repository presents **Universal Exclusive Adversarial Perturbation (UEAP)** — a novel adversarial attack approach designed to affect only a specific subset of images within the same class, leaving others unharmed. Developed by **Idan Yankelev, Yarin Yerushalmi Levi, Amit Baras, and Yonatan Amaru** as part of a research project in 2024.

---

## 📌 Project Overview

In contrast to traditional Universal Adversarial Perturbations (UAPs) which affect all images, **UEAPs** are crafted to:
- **Degrade classifier performance on a victim subset** (e.g., black dogs),
- **Maintain correct classification on a keep subset** (e.g., non-black dogs),
- All within the same semantic class.

## 🧠 Methodology

Given a class `c` and its image set `S`, we split it into:
- `sj` (victim images) ⟶ to be misclassified
- `S \ sj` (keep images) ⟶ to retain original classification

### Objective:
- For `xi ∈ sj`: `f(xi + δ) = y_target ≠ c`
- For `xj ∈ S \ sj`: `f(xj + δ) = c`

### Exclusive Loss Function:
```
loss_exclusive = α * Σ f(x_keep + δ, y_gt) ± β * Σ f(x_victim + δ, y_target)
```
Where:
- `α`, `β` control trade-off
- `±` depends on whether attack is targeted or untargeted

## ⚙️ Implementation Details

- **Model**: ResNet50 fine-tuned on 4 classes: dogs, cats, birds, fish.
- **Dataset**: Custom subset of ImageNet + Kaggle datasets with ~1150 images/class.
- **Attack Type**: Targeted & Untargeted UEAP and UAP
- **Optimization**: Based on PGD (Projected Gradient Descent)
  - Step size: 9/255
  - Epsilon budget: 16/255 to 50/255
  - Iterations: 50

## 📊 Results Summary

| Class | Targeted UAP | Targeted UEAP | Untargeted UAP | Untargeted UEAP |
|-------|--------------|----------------|----------------|------------------|
| Dogs  | 68.75%       | ✅ Less harmful | 81.25%         | ✅ More selective |
| Cats  | 81.25%       | ✅ Better       | 78.12%         | ✅ Preserved keep |
| Fish  | 53.12%       | ✅ Sharper      | 21.88%         | ✅ Controlled     |
| Birds | 31.25%       | ✅ Effective    | 34.38%         | ✅ Focused        |

UEAP outperformed UAP in keeping non-target images unaffected while still degrading performance on the victim subset.

## 📈 Key Findings

- **Selective Vulnerability**: UEAP attacks are harder to detect due to their focused impact.
- **Trade-off Observed**: Stronger victim attack often compromises keep set accuracy.
- **Overfitting Issue**: Overfitting to a small dataset suggests need for more training data.

## 🔮 Future Work

- Expand dataset to reduce overfitting and increase robustness.
- Explore applications on object detection and real-world vision systems.
- Enhance optimization for better victim-targeting with minimal collateral.
