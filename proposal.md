---
layout: default
title: Proposal
section_nav: true
---

<section class="content-grid">
  <aside class="page-sidebar">
    <nav class="section-nav" aria-label="Proposal sections">
      <h2>Sections</h2>
      <a class="section-link" href="#video">Proposal Video</a>
      <a class="section-link" href="#section-1">1. Introduction</a>
      <a class="section-link" href="#section-2">2. Problem Definition</a>
      <a class="section-link" href="#section-3">3. Methods</a>
      <a class="section-link" href="#section-4">4. Results &amp; Discussion</a>
      <a class="section-link" href="#references">References</a>
      <a class="section-link" href="#gantt">Gantt Chart</a>
      <a class="section-link" href="#contribution">Contribution Table</a>
    </nav>
  </aside>
  <article class="page-body" markdown="1">

<h2 id="video">Proposal Video</h2>
<div class="video-embed">
  <iframe src="https://www.youtube.com/embed/vj-4mJqYQno" title="Proposal video" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

<h2 id="section-1">1. Introduction / Background</h2>

We propose a multimodal ML pipeline to process de-identified urology visit artifacts (scanned reports and images) and train supervised models that generate reports similar in style and content to the originals. The task involves aligning textual reports with corresponding images so that, when given an unseen study, the system can output a coherent, clinically styled draft.

Multimodal medical models are advancing rapidly: **Google's MedGemma** is designed for medical text-image comprehension and shows strong benchmark performance [<a href="#ref-1">1</a>], [<a href="#ref-7">7</a>]. **LLaVA-Med** [<a href="#ref-2">2</a>] and **BiomedCLIP** [<a href="#ref-3">3</a>] demonstrate effective biomedical vision-language alignment, providing open-source starting points. For text-only baselines, **ClinicalBERT** remains a strong encoder for clinical notes [<a href="#ref-4">4</a>].

Some scans contain text burned into images (faxed forms, typed overlays). To make these usable, we apply OCR preprocessing [<a href="#ref-5">5</a>], [<a href="#ref-6">6</a>].

---

<h3 id="dataset">Dataset</h3>

We're starting with 198 studies, each structured as:

- `/scans/` - PNG images (some with annotations)
- `/ocr/` - OCR outputs from Advanced Urology's script
- `report.txt` - Corresponding urology report

This gives us paired multimodal data: images (plus OCR text) aligned with reports. More studies will be added over time.

<h2 id="section-2">2. Problem Definition</h2>

**Problem:** Given urology studies (scans plus optional OCR), we aim to train supervised models that generate clinical reports similar to the references.

**Motivation:** Radiologists interpret scans and draft detailed reports that urologists rely on for patient care. This process is very time consuming at scale. An assistive system that generates a first pass draft could streamline workflow [<a href="#ref-8">8</a>], [<a href="#ref-9">9</a>]. Radiologists would only verify and edit, reducing reporting time, improving consistency, and giving urologists faster access to structured reports. This system is assistive only, it's not a replacement for actual clinicians.

<h2 id="section-3">3. Methods</h2>

<h3 id="methods-preprocessing">Preprocessing</h3>

- **OCR integration** - use Advanced Urology's outputs; optionally re-run with other engines
- **Text normalization** - clean OCR text, tokenize, chunk long documents
- **Image prep** - resize and normalize scans, optionally extract ROIs
- **Dataset splits** - patient-level train/val/test partitions to avoid leakage

---

<h3 id="methods-models">Models</h3>

- **Text baselines** - n-gram or TF-IDF retrieval templates
- **Clinical PLMs** - fine-tune ClinicalBERT [<a href="#ref-4">4</a>] or similar encoders
- **Multimodal V+L**:
  - BiomedCLIP embeddings for image-conditioned generation [<a href="#ref-3">3</a>]
  - LLaVA-Med for multimodal captioning [<a href="#ref-2">2</a>]
  - MedGemma for long-context multimodal comprehension [<a href="#ref-1">1</a>], [<a href="#ref-7">7</a>]

---

<h3 id="methods-supervised">Supervised Learning</h3>

- Retrieval baselines (TF-IDF, nearest-neighbor)
- Transformer encoder-decoder models (BERT2BERT, T5, MedGemma fine-tuning)
- Evaluation via **BLEU, ROUGE, BERTScore** against references

---

<h3 id="methods-unsupervised">Where Unsupervised Helps</h3>

- **Quality control** - UMAP or k-means on embeddings to detect outliers
- **Exploration** - topic modeling (BERTopic) for grouping and curriculum training

---

<h3 id="methods-impl">Implementations</h3>

Hugging Face Transformers, OpenCLIP/BiomedCLIP, LLaVA-Med, MedGemma, long-context retrieval

---

<h3 id="methods-ethics">Safety &amp; Ethics</h3>

- All reports de-identified by Advanced Urology
- Outputs must be verified by radiologists
- We are adding guardrails against hallucination with clinician-in-the-loop validation

<h2 id="section-4">4. (Potential) Results &amp; Discussion</h2>

<h3 id="results-metrics">Metrics</h3>

- **Text similarity** - BLEU, ROUGE-L, METEOR
- **Embedding-based** - BERTScore, CLIPScore
- **Clinical correctness** - rule-based key-finding checks plus expert review
- **Efficiency** - throughput (reports per second)

---

<h3 id="results-goals">Goals</h3>

- Achieve **ROUGE-L >= 0.7** (competitive similarity)
- Capture **clinical style and structure**
- Ensure safe deployment - outputs as drafts, not diagnoses

---

<h3 id="results-expected">Expected Results</h3>

- Transformer models (ClinicalBERT-conditioned seq2seq) outperform retrieval baselines
- Multimodal fusion (BiomedCLIP, LLaVA-Med, MedGemma) improves scan-specific detail inclusion
- Long-context handling reduces truncation issues and improves coherence

<h2 id="references">References</h2>

[1] <span id="ref-1"></span> D. Golden and R. Pilgrim, "MedGemma: Our most capable open models for health AI development," *Google Research*, Jul. 09, 2025. <https://research.google/blog/medgemma-our-most-capable-open-models-for-health-ai-development/> (accessed Oct. 02, 2025).

[2] <span id="ref-2"></span> C. Li *et al.*, "LLaVA-Med: Training a Large Language-and-Vision Assistant for Biomedicine in One Day," Jun. 2023. doi: <https://doi.org/10.48550/arXiv.2306.00890>.

[3] <span id="ref-3"></span> S. Zhang *et al.*, "BiomedCLIP: a multimodal biomedical foundation model pretrained from fifteen million scientific image-text pairs," Jan. 2025. doi: <https://doi.org/10.48550/arXiv.2303.00915>.

[4] <span id="ref-4"></span> K. Huang, J. Altosaar, and R. Ranganath, "ClinicalBERT: Modeling Clinical Notes and Predicting Hospital Readmission," Nov. 2020. doi: <https://doi.org/10.48550/arxiv.1904.05342>.

[5] <span id="ref-5"></span> E. Hsu, I. Malagaris, Y.-F. Kuo, R. Sultana, and K. Roberts, "Deep learning-based NLP data pipeline for EHR-scanned document information extraction," *JAMIA Open*, vol. 5, no. 2, Jun. 2022, doi: <https://doi.org/10.1093/jamiaopen/ooac045>.

[6] <span id="ref-6"></span> J. K. James *et al.*, "Experience With an Optical Character Recognition Search Application for Review of Outside Medical Records," *Mayo Clinic Proceedings: Digital Health*, vol. 2, no. 4, pp. 511â€“514, Aug. 2024, doi: <https://doi.org/10.1016/j.mcpdig.2024.08.001>.

[7] <span id="ref-7"></span> G. Preda, "From X-rays to Insights: Exploring MedGemma 4B's Medical Multimodality," *Medium.com*, May 25, 2025. <https://medium.com/@gabi.preda/from-x-rays-to-insights-exploring-medgemma-4bs-medical-multimodality-ef620a4d8264> (accessed Oct. 03, 2025).

[8] <span id="ref-8"></span> L. Yang *et al.*, "Advancing Multimodal Medical Capabilities of Gemini," May 2024. doi: <https://doi.org/10.48550/arXiv.2405.03162>.

[9] <span id="ref-9"></span> K. Saab *et al.*, "Capabilities of Gemini Models in Medicine," *arXiv.org*, May 01, 2024. <https://arxiv.org/abs/2404.18416> (accessed Oct. 02, 2025).

<h2 id="gantt">Gantt Chart</h2>
<p><a href="https://gtvault-my.sharepoint.com/:x:/g/personal/jkaur46_gatech_edu/EeK2_WYbi4dOmPsW9AqqfBcBWANPqZlzL_4FAyF8L6i5Tw?e=Dj48YB&nav=MTVfezAwMDAwMDAwLTAwMDEtMDAwMC0wMTAwLTAwMDAwMDAwMDAwMH0" target="_blank" rel="noopener">Here is the Gantt chart</a></p>

<h2 id="contribution">Contribution Table</h2>
<p align="center">
    <img src="{{ '/assets/images/contribution.png' | relative_url }}" alt="Contribution Table" width="600">
</p>
> We will opt in to the award consideration.

  </article>
</section>
