<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0f2027,50:203a43,100:2c5364&height=180&section=header&text=Brain%20Tumour%20MRI%20Classification&fontSize=36&fontColor=ffffff&fontAlignY=38&desc=VGG16%20vs%20ViT-B%2F16%20vs%20Hybrid%20CNN%E2%80%93Transformer%20%C2%B7%20Grad-CAM&descAlignY=60&descSize=16" width="100%"/>

<div align="center">

<img src="https://img.shields.io/badge/Test%20Accuracy-99.16%25-00E5FF?style=flat-square" />
<img src="https://img.shields.io/badge/Dataset-7%2C153%20MRI%20scans-blue?style=flat-square" />
<img src="https://img.shields.io/badge/Presented-BITS%20Pilani%2C%20Goa-orange?style=flat-square" />
<br/>
<img src="https://skillicons.dev/icons?i=py,pytorch,sklearn&theme=dark" />

</div>

---

## 🧠 ᴏᴠᴇʀᴠɪᴇᴡ

This project classifies brain MRI scans into **4 classes**: glioma, meningioma, pituitary tumour and no tumour. It compares three deep learning models under identical conditions (same split, image size, optimizer and learning rate) and adds **Grad-CAM** heatmaps to every model, so each prediction can be checked visually.

It was my research internship work at **RAPID Center, PES University**, and I presented it at the **8th conference at BITS Pilani, Goa**. The full paper is [`BRAIN_TUMOR_CLASSIFICATION_FINAL.pdf`](BRAIN_TUMOR_CLASSIFICATION_FINAL.pdf).

---

## 📊 ʀᴇꜱᴜʟᴛꜱ

| Model | Test accuracy | Epochs | Notebook |
|:--|:--:|:--:|:--|
| VGG16 (fine-tuned) | 98.74% | 20 | [`vgg16_(7153).ipynb`](vgg16_(7153).ipynb) |
| ViT-B/16 (pretrained) | **99.16%** | 20 | [`vit_7153.ipynb`](vit_7153.ipynb) |
| **Hybrid VGG16 + ViT (proposed)** | **99.16%** | **15** | [`VGG16_ViT_Hybrid_BrainTumor(7153).ipynb`](VGG16_ViT_Hybrid_BrainTumor(7153).ipynb) |

```diff
+ The hybrid model matches ViT accuracy in 15 epochs instead of 20, which is 25% less training.
```

**Per-class results for the hybrid model** (test set, 716 images):

| Class | Precision | Recall | F1 | Support |
|:--|:--:|:--:|:--:|:--:|
| Glioma | 0.99 | 0.99 | 0.99 | 162 |
| Meningioma | 0.98 | 0.98 | 0.98 | 178 |
| No tumour | 1.00 | 1.00 | 1.00 | 200 |
| Pituitary | 0.99 | 0.99 | 0.99 | 176 |

The hybrid model misclassified 6 of 716 test images: 2 glioma ↔ meningioma swaps in each direction, and 1 meningioma ↔ pituitary swap in each direction. All 200 no-tumour scans were classified correctly.

---

## 🏗️ ᴍᴇᴛʜᴏᴅ

```text
                 ┌──────────────────────┐
  MRI scan ─────►│ VGG16 branch         │──► 4096 → 256 ─┐
  (224×224)  │   └──────────────────────┘                │
             │   ┌──────────────────────┐                ├─► concat (512-d) ─► classifier ─► 4 classes
             └──►│ ViT-B/16 branch      │──►  768 → 256 ─┘
                 └──────────────────────┘
```

- **Dataset:** Brain Tumor MRI Dataset, 7,153 images in 4 classes, stratified 80 / 10 / 10 train / validation / test split.
- **VGG16:** ImageNet-pretrained, fine-tuned for 4 classes. The convolutions capture local texture and edges.
- **ViT-B/16:** `vit_base_patch16_224` from timm, pretrained and fine-tuned. Self-attention captures global context across the whole scan.
- **Hybrid:** both branches run in parallel; their 256-d features are concatenated into a 512-d joint embedding.
- **Training:** 224×224 inputs, batch size 16, AdamW, learning rate 1e-4. The best checkpoint was selected on validation accuracy.
- **Explainability:** Grad-CAM heatmaps for all three models, plus an ablation study measuring each hybrid branch's contribution.

---

## 🚀 ʜᴏᴡ ᴛᴏ ʀᴜɴ

```bash
# 1. Open any notebook in Google Colab and select a GPU runtime
# 2. Download the Brain Tumor MRI Dataset (7,153 images, 4 classes) from Kaggle
# 3. Upload the dataset zip when the notebook asks for it
# 4. Run all cells: training, test evaluation, confusion matrix and Grad-CAM figures
```

**Stack:** Python · PyTorch · torchvision · timm · scikit-learn · OpenCV · Matplotlib · Seaborn · Google Colab

---

## 📁 ʀᴇᴘᴏꜱɪᴛᴏʀʏ

```text
├── VGG16_ViT_Hybrid_BrainTumor(7153).ipynb   # proposed hybrid model
├── vit_7153.ipynb                            # ViT-B/16 baseline
├── vgg16_(7153).ipynb                        # VGG16 baseline
└── BRAIN_TUMOR_CLASSIFICATION_FINAL.pdf      # full paper
```

---

<div align="center">

**Navyashree J** · BTech CSE (AI/ML), PES University

<a href="https://github.com/Navyashree04"><img src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white" /></a>
<a href="https://linkedin.com/in/navyashreej1"><img src="https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white" /></a>

</div>

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:2c5364,50:203a43,100:0f2027&height=100&section=footer" width="100%"/>
