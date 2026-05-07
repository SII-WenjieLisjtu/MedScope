<div align="center">

# MedScope

**Incentivizing "Think with Videos" for Clinical Reasoning via Coarse-to-Fine Tool Calling**

[![ICML](https://img.shields.io/badge/ICML-2026-blue)](https://icml.cc/Conferences/2026)
[![arXiv](https://img.shields.io/badge/arXiv-2602.13332-b31b1b.svg)](https://arxiv.org/abs/2602.13332)
[![License](https://img.shields.io/badge/License-Apache%202.0-green.svg)](LICENSE)

</div>

<div align="center">

<img src="assets/logo_sii.jpg" height="60" alt="Shanghai Innovation Institute"> &nbsp;&nbsp;&nbsp;&nbsp;
<img src="assets/logo_sjtu_medicine.png" height="60" alt="Shanghai Jiao Tong University"> &nbsp;&nbsp;&nbsp;&nbsp;
<img src="assets/logo_fudan.png" height="60" alt="Fudan University">

</div>

---

<div align="center">
  <a href="https://arxiv.org/abs/2602.13332">[arXiv Paper]</a> &nbsp;|&nbsp;
  <a href="README.zh.md">🇨🇳 中文</a> &nbsp;|&nbsp;
  <a href="#citation">[Citation]</a>
</div>

---

### Overview

**MedScope** is a tool-using clinical video reasoning model that enables doctors to **"think with videos"** through iterative, coarse-to-fine evidence seeking and verification over long-form medical procedures. Unlike conventional multimodal large language models that passively sample videos, MedScope interleaves intermediate reasoning with targeted tool calls to iteratively locate, verify, and justify predictions with temporally grounded visual evidence.

This repository contains the official implementation of our ICML 2026 paper:
> **MedScope: Incentivizing "Think with Videos" for Clinical Reasoning via Coarse-to-Fine Tool Calling**  
> Wenjie Li\*, Yujie Zhang\*, Haoran Sun, Xinqi He, Hongcheng Gao, Chenglong Ma, Ming Hu, Guankun Wang, Shiyi Yao, Renhao Yang, Hongliang Ren, Lei Wang, Junjun He, Yankai Jiang

---

### Framework

<div align="center">
  <img src="assets/figure_framework.png" width="90%" alt="MedScope Framework">
  <p><em>Figure 1. Overview of MedScope. (a) Coarse-to-fine clinical reasoning with explicit thought and tool actions. (b) Three-stage training pipeline: Clinical Reasoning Warm-up, Visual-CoT cold-start SFT, and GA-GRPO agentic RL.</em></p>
</div>

---

### Key Contributions

1. **Medical Visual Chain-of-Thought Paradigm**  
   We propose a "think with videos" paradigm and instantiate it with **MedScope**, a tool-using LMM trained via a three-stage pipeline (warm-up, visual-CoT cold-start, and grounding-aware GRPO) that elicits coarse-to-fine clinical reasoning.

2. **ClinVideoSuite: Evidence-Centric Data Suite**  
   We build a large-scale, evidence-centric clinical video suite for training and evaluation, including **ClinVideo-Eval**, a fine-grained long-form medical video reasoning benchmark with traceable evidence. The suite comprises:
   - **ClinVideo-Cap-635K**: 634.8K timestamped dense captions
   - **ClinVideo-QA-254K**: 253.8K evidence-grounded QA pairs with localized supervision windows
   - **ClinVideo-VCoT-34K** / **ClinVideo-CoT-84K**: Tool-augmented visual CoT trajectories

3. **GA-GRPO: Grounding-Aware RL**  
   We introduce **GA-GRPO** (Grounding-Aware Group Relative Policy Optimization), an agentic RL recipe with grounding-aligned rewards and adaptive advantage modulation to promote temporally aligned tool use and encourage diverse reasoning under varying video conditions.

---

### Main Results

MedScope achieves **state-of-the-art** performance among open-source models on both in-domain and out-of-domain evaluations.

**SVU-31K (Multi-grained Video Understanding)**
- Full Video Description: **4.76** DO (state-of-the-art)
- Fine-grained Temporal Reasoning: **11.30** BLEU-4, **4.56** CIDEr
- Fine-grained Perception: **18.90** CIDEr, **10.93** METEOR

**ClinVideo-Eval (Grounded Clinical Video Reasoning)**
- OphVL Temporal Grounding: **64.42** mIoU
- MedVideoCap Grounded VQA: **65.58** mIoU
- Strong generalization to out-of-domain **SurgVidLM**

<div align="center">
  <img src="assets/figure_results_svu.png" width="85%" alt="Results on SVU-31K and ClinVideo-Eval">
  <p><em>Figure 2. Performance comparison on full and fine-grained video understanding and VQA benchmarks.</em></p>
</div>

<div align="center">
  <img src="assets/figure_results_clinvideo.png" width="90%" alt="Results on ClinVideo-Eval">
  <p><em>Table 1. Performance comparison on ClinVideo-Eval (in-domain and out-of-domain).</em></p>
</div>

---

### Dataset Statistics

<div align="center">
  <img src="assets/figure_dataset_stats.png" width="90%" alt="ClinVideoSuite Statistics">
  <p><em>Figure 3. Overview of ClinVideoSuite data characteristics across MedVideoCap, SurgVidLM, and OphVL.</em></p>
</div>

---

### Release Status

- [x] Paper (arXiv)
- [ ] **Code & Models** — *Will be released soon. Stay tuned!*
- [ ] **ClinVideoSuite Dataset** — *Will be released soon.*

---

<h2 id="citation">Citation</h2>

If you find this work useful for your research, please consider citing our paper:

```bibtex
@inproceedings{li2026medscope,
  title={MedScope: Incentivizing "Think with Videos" for Clinical Reasoning via Coarse-to-Fine Tool Calling},
  author={Li, Wenjie and Zhang, Yujie and Sun, Haoran and He, Xinqi and Gao, Hongcheng and Ma, Chenglong and Hu, Ming and Wang, Guankun and Yao, Shiyi and Yang, Renhao and Ren, Hongliang and Wang, Lei and He, Junjun and Jiang, Yankai},
  booktitle={Proceedings of the 43rd International Conference on Machine Learning (ICML)},
  year={2026}
}
```

---

## Acknowledgements

This work is supported by Shanghai Innovation Institute, Shanghai Jiao Tong University School of Medicine, and Shanghai Artificial Intelligence Laboratory. We gratefully acknowledge the clinical data support from Ruijin Hospital.

---

## Contact

For questions or suggestions, please feel free to open an issue or contact the authors.

---

<div align="center">
  <img src="assets/logo_sii.jpg" height="40" alt="Shanghai Innovation Institute">
  <p><small>Shanghai Innovation Institute</small></p>
</div>
