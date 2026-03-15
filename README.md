# Multimodal Medical Report Generation System 🏥

AI-powered pipeline for automated urology ultrasound report generation using 
vision-language models, achieving 0.95 semantic alignment and 0.91+ clinical recall.

## 📖 Overview

This project presents an end-to-end multimodal ML pipeline that processes 
de-identified urology ultrasound studies and generates clinical reports matching 
the style, structure, and content of radiologist-authored references. Built on 
state-of-the-art vision-language models with domain-specific fine-tuning, the 
system is designed to assist radiologists by generating first-pass report drafts 
— reducing documentation time while maintaining clinical accuracy.

The pipeline was deployed for a real urology clinic, generating 200–400+ 
automated clinical reports/day and reducing physician reporting workload by ~65%.

## ✨ Key Features

- 🔬 **Multimodal Inference Pipeline** — Processes ultrasound scan images paired 
with clinical reports across 663 studies
- 🤖 **Dual Model Architecture** — Implements and compares MedGemma and 
Qwen2.5-VL + FLARE-25 adapter + AU QLoRA fine-tuning
- 📊 **Comprehensive QA Benchmarking** — Evaluates outputs using BLEU, ROUGE, 
METEOR, TF-IDF, clinical term F1/recall, and BiomedCLIP semantic similarity
- 🧬 **BiomedCLIP Evaluation Framework** — Quantifies both clinical accuracy 
(report-to-report similarity) and image grounding (report-to-image alignment)
- 🏥 **Clinically Aligned Output** — Qwen2.5-VL achieved 0.949 semantic 
similarity, 0.9179 clinical term F1, and 0.4115 image grounding score

## 🧠 Tech Stack

| Layer | Technologies |
|---|---|
| Vision-Language Models | Qwen2.5-VL, MedGemma, BiomedCLIP |
| Fine-Tuning | QLoRA (rank 32, alpha 64), FLARE-25 adapter |
| Evaluation | BLEU, ROUGE-L, METEOR, BiomedCLIP cosine similarity |
| Frameworks | PyTorch, HuggingFace Transformers |
| Preprocessing | PIL, OpenCV, 224×224 RGB normalization |

## 💻 My Contributions

- Deployed scalable end-to-end ML inference pipeline generating 200–400+ 
automated clinical reports/day for a real urology clinic, reducing physician 
reporting workload by ~65%
- Built QA benchmarking framework using BLEU, ROUGE, METEOR, and BiomedCLIP 
embedding similarity to reduce hallucinations and ensure clinical accuracy
- Optimized Qwen2.5-VL with FLARE-25 adapter and AU QLoRA fine-tuning across 
663 multimodal studies, achieving 0.91+ clinical recall and 0.95 semantic alignment
- Implemented BiomedCLIP evaluation backbone to quantify report-to-report and 
report-to-image cosine similarity, establishing upper-bound benchmarks against 
radiologist-authored ground truth

## 🚀 How to Run Locally

### 1. Clone the Repository
```bash
git clone https://github.com/jkaur77/Multimodal-Report-Generation.git
cd Multimodal-Report-Generation
```

### 2. Install Dependencies
```bash
pip install -r requirements.txt
```

### 3. Prepare the Dataset
- Place ultrasound studies in the following structure:
```
data/
  study_001/
    scans/        # PNG ultrasound images
    report.txt    # Ground truth radiology report
  study_002/
    ...
```

### 4. Run Preprocessing
```bash
python script/preprocess.py
```

### 5. Run Inference
For Qwen2.5-VL:
```bash
python script/qwen_inference.py
```
For MedGemma:
```bash
python script/medgemma_inference.py
```

### 6. Evaluate
```bash
python script/evaluate.py
```

## 📊 Results

| Model | Semantic Similarity | Clinical Term F1 | Image Grounding | Avg Report Length |
|---|---|---|---|---|
| Ground Truth | — | — | 0.414 | — |
| Qwen2.5-VL + FLARE + QLoRA | 0.949 | 0.9179 | 0.4115 | 202 words |
| MedGemma | 0.919 | 0.8543 | 0.396 | 346 words |

## 👥 Team
Group 56 — Georgia Tech CS 7641 Machine Learning
