
# 🛡️ Universal Exclusive Adversarial Perturbation (UEAP)

This repository showcases our novel adversarial attack method — **Universal Exclusive Adversarial Perturbation (UEAP)** — developed as a research project by **Idan Yankelev, Yarin Yerushalmi Levi, Amit Baras, and Yonatan Amaru** in March 2024. The method targets specific subsets of images within the same class in a deep learning image classifier without affecting the rest, offering a more stealthy and focused adversarial strategy.

---

## 📌 Overview

Traditional adversarial perturbations affect all samples, making them easier to detect and mitigate. UEAP innovates by designing **class-specific** attacks that only degrade classifier performance on a chosen **subset** (e.g., black cats), leaving others (e.g., non-black cats) in the same class unaffected.

This offers:
- Higher stealth and specificity in attacks
- Improved understanding of model weaknesses
- Foundations for robust defenses and ethical red teaming

---

## 🔍 Research Goals

- Design perturbations that:
  - Misclassify only specific visual variants within a class (victims)
  - Preserve performance on the remaining samples in the same class (keep)
- Maintain universal applicability (same perturbation across multiple images)
- Evaluate both targeted and untargeted attack settings

---

## 🧪 Methodology

### Formal Setup
Let:
- `S` be the set of all images in class `c`
- `sj ⊂ S` be the victim subset (e.g., black dogs)
- `S \ sj` be the keep subset (e.g., non-black dogs)

### Optimization Objective
- For `x ∈ sj`: `f(x + δ) ≠ c` or `= y_target` (victim misclassification)
- For `x ∈ S \ sj`: `f(x + δ) = c` (keep correctness)

### Loss Function
```
loss_exclusive = α * Σ Loss(keep outputs, keep labels)
               ± β * Σ Loss(victim outputs, victim labels)
```
- `α`, `β`: control balance between precision and damage
- `±`: plus for targeted attack, minus for untargeted

### Algorithms
- Built upon PGD (Projected Gradient Descent)
- Compared to baseline Universal Adversarial Perturbation (UAP)

---

## 🖼️ Dataset & Model

- **Base Model**: ResNet50 fine-tuned on 4 animal classes: `dog`, `cat`, `bird`, `fish`
- **Source**: Curated datasets from Kaggle:
  - [Cats vs Dogs](https://www.kaggle.com/datasets/shaunthesheep/microsoft-catsvsdogs-dataset)
  - [Colored Dogs & Cats](https://www.kaggle.com/datasets/ahmedmostafa11111/cd-deep)
  - [Fish Dataset](https://www.kaggle.com/datasets/markdaniellampa/fish-dataset)
  - [Bird Species](https://www.kaggle.com/datasets/gpiosenka/100-bird-species)

- **Image Count**: ~1150 per class
- **Data Split**:
  - Training
  - Validation
  - Test (manually partitioned into "keep" and "victim")

---

## 📊 Experiment Details

- **Attack Budget (ε)**: 16/255 to 50/255
- **Step Size**: 9/255
- **Iterations**: 50
- **UEAP vs UAP Comparison**
  - Performed both targeted and untargeted attacks
  - Evaluated on test subsets

### Metrics
- Accuracy degradation on **victim** images (higher = more successful attack)
- Accuracy preservation on **keep** images (higher = more stealth)

### Results Summary

| Class | Keep Accuracy (Original/UAP/UEAP) | Victim Accuracy (Original/UAP/UEAP) |
|-------|------------------------------------|--------------------------------------|
| Dogs  | 100% / 68.75% / ✅81.25%           | 96.88% / 37.50% / ✅75.00%            |
| Cats  | 100% / 78.12% / ✅84.38%           | 96.80% / 43.75% / ✅46.88%            |
| Fish  | 100% / 21.88% / ✅90.62%           | 100.0% / 21.88% / ✅90.62%            |
| Birds | 96.88% / 34.38% / ✅40.62%         | 100.0% / 34.38% / ✅56.25%            |

🟢 UEAP showed strong ability to selectively degrade victim predictions while maintaining performance on keep images.

---

## 🔎 Insights

- **Stealth attacks** are possible and practical.
- **Targeted attacks** with UEAP can outperform untargeted ones under some settings.
- **Trade-off exists** between effectiveness on victim images and preserving keep accuracy.
- Larger datasets could reduce overfitting and improve generalization.


## 🔮 Future Work

- Train on a larger, more diverse dataset
- Explore real-world use cases: face recognition, object detection
- Investigate black-box settings for transferability of UEAP
- Develop defenses against exclusive perturbations

---
