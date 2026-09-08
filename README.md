# 🛡️ Adversarial Attacks in NLP Deep Learning Models

[![Python](https://img.shields.io/badge/Python-3.9%2B-blue?logo=python&logoColor=white)](https://www.python.org/)
[![PyTorch](https://img.shields.io/badge/PyTorch-2.x-EE4C2C?logo=pytorch&logoColor=white)](https://pytorch.org/)
[![Transformers](https://img.shields.io/badge/🤗_Transformers-Hugging_Face-FFD21E)](https://huggingface.co/docs/transformers)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

> **The text analogue of [adversarial_attacks_vision](https://github.com/fragompul/adversarial_attacks_vision): unveiling how Transformer-based NLP models can be fooled by imperceptible or fluent-sounding changes to text.**

## 📖 Project Overview

Transformer-based language models achieve near-human performance on sentiment analysis, natural
language inference, and text classification. They share a critical vulnerability with their vision
counterparts: small, often human-imperceptible or fluency-preserving changes to the input text can
flip a prediction with high confidence. Unlike images, text is **discrete**: there is no continuous
gradient you can walk along pixel-by-pixel, so every attack here has to solve a fundamentally
different search problem than `FGSM`/`PGD` do in the vision repo, word substitution, character
flips, and gradient-guided discrete search, rather than a continuous perturbation.

**Origin & Scope:**
This is the sibling repository to my Bachelor's Thesis project on adversarial vision, extending the
same rigor (hand-derived math, from-scratch implementations, quantitative robustness evaluation) to
the text domain. The two repos share a research philosophy but not a codebase: NLP attacks need a
different toolchain (PyTorch, Hugging Face Transformers, discrete combinatorial search) from the
TensorFlow/Keras stack used for vision.

---

## ✨ Key Features & Research Areas

1. **White-Box Attack:** Gradient-guided word substitution (**HotFlip**, Ebrahimi et al., 2018),
   implemented from scratch against the embedding layer's gradient, the discrete-input analogue of
   `attacks/whitebox/01_FGSM.ipynb` in the vision repo.
2. **Black-Box Attack:** Word-importance-ranked WordNet synonym substitution in the style of
   TextFooler/PWWS, a query-only attack that never touches a gradient, mirroring
   `attacks/blackbox/01_SquareAttack.ipynb`'s threat model.
3. **Quantitative Robustness Analytics:** Attack Success Rate, percentage of words changed (the text
   analogue of an L2 distortion), and query cost, measured on a real sample of SST-2 validation
   sentences rather than a hand-picked few.
4. **Defenses:** Adversarial data augmentation, fine-tuning on HotFlip-generated examples labeled
   with their true class, the text analogue of the vision repo's PGD-AT/TRADES notebook.
5. **Embedding Space Topology:** PCA of DistilBERT sentence embeddings, tracking how HotFlip's word
   substitutions move a sentence across the classifier's decision boundary, the text analogue of
   `latent_space/` in the vision repo.

---

## 📂 Repository Guide

Notebooks are numbered in the order they are meant to be read, and every folder has its own README.

1. **[`attacks/`](attacks/)**, split into `whitebox/` (HotFlip) and `blackbox/` (WordNet synonym substitution).
2. **[`robustness_evaluation/`](robustness_evaluation/)**: both attacks compared on a real sample of SST-2 sentences, and a perturbation-budget sweep showing HotFlip saturates almost immediately while Synonym Substitution keeps climbing.
3. **[`defenses/`](defenses/)**: adversarial data augmentation with HotFlip-generated examples.
4. **[`latent_space/`](latent_space/)**: PCA trajectory of a sentence's embedding as HotFlip attacks it.

Datasets are pulled directly through Hugging Face's `datasets` library (SST-2) rather than kept as
local copies, so there is no `data/` folder to track in git.

---

## 🚀 Getting Started

```bash
git clone https://github.com/fragompul/adversarial_attacks_nlp.git
cd adversarial_attacks_nlp
python -m venv .venv
.venv\Scripts\activate   # or: source .venv/bin/activate on Linux/macOS
pip install -r requirements-dev.txt
jupyter lab
```

---

## 🛠️ Technology Stack

* **Deep Learning Framework:** PyTorch
* **NLP Models:** Hugging Face Transformers (DistilBERT)
* **NLP Utilities:** NLTK (WordNet synonym lookup, POS tagging), Hugging Face `datasets` (SST-2)
* **Data Science & Visualization:** NumPy, Pandas, Scikit-Learn, Matplotlib

---

## 🌐 Part of a Research Family

Adversarial vulnerability is not specific to any one input type; the same core idea, small deliberate input changes causing disproportionate output changes, shows up wherever a model makes a decision. This repo has sibling projects applying the same rigor elsewhere:

* **[adversarial_attacks_vision](https://github.com/fragompul/adversarial_attacks_vision)** — the flagship of the family: FGSM/PGD/C&W and more on image classifiers, plus defenses, explainability, and a live dashboard.
* **[adversarial_attacks_audio](https://github.com/fragompul/adversarial_attacks_audio)** — fooling a keyword-spotting model with imperceptible waveform perturbations.
* **[adversarial_attacks_tabular](https://github.com/fragompul/adversarial_attacks_tabular)** — small, deliberate changes to a handful of numeric features swinging a regression model's prediction (house price appraisal), closer to fraud than to a picture that looks wrong.

---

## Author

**Francisco Javier Gómez Pulido**

*AI Lead @ AAPEX | Double Major in Mathematics & Computer Science* | Master's in Artificial Intelligence

📫 **Let's connect:**
* **LinkedIn:** [linkedin.com/in/frangomezpulido](https://www.linkedin.com/in/frangomezpulido)
* **GitHub:** [github.com/fragompul](https://github.com/fragompul)
* **Email:** [frangomezpulido2002@gmail.com](mailto:frangomezpulido2002@gmail.com)

---
*Sibling project to [adversarial_attacks_vision](https://github.com/fragompul/adversarial_attacks_vision). If you find this interesting, feel free to ⭐ star it!*
