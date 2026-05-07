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
  <a href="#english">🇺🇸 English</a> &nbsp;|&nbsp;
  <a href="#chinese">🇨🇳 中文</a> &nbsp;|&nbsp;
  <a href="#citation">[Citation]</a>
</div>

> **Click 🇺🇸 English / 🇨🇳 中文 above to switch language.**

---

<h2 id="english">English</h2>

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

<h2 id="chinese">中文介绍</h2>

### 概述

**MedScope** 是一个面向临床长视频推理的工具使用型多模态大语言模型。不同于传统模型对视频进行被动采样或弱监督检查，MedScope 通过**粗到细（coarse-to-fine）**的证据搜索与验证机制，使模型能够像临床医生一样"用视频思考"——在长篇手术及内镜视频中迭代定位、验证并归因时间敏感的关键视觉证据。

本仓库为 ICML 2026 录用论文的官方实现：
> **MedScope: Incentivizing "Think with Videos" for Clinical Reasoning via Coarse-to-Fine Tool Calling**  
> 李文杰\*、张玉洁\*、孙浩然、何新琪、高鸿成、马成龙、胡明、王冠坤、姚诗怡、杨仁豪、任洪亮、王磊、何俊军、蒋炎侃

---

### 核心方法

MedScope 的核心是一个**视觉思维链（Visual Chain-of-Thought）**框架，通过轻量级视觉工具箱实现粗到细的临床推理：

1. **工具定义**
   - `crop_video`：接收视频与时间窗口，返回片段级密集采样帧序列，用于局部证据收集
   - `get_frame`：接收视频与时间戳，返回最近的三帧用于精细验证

2. **三阶段训练**
   - **阶段一：Clinical Reasoning Warm-up** —— 融合多源医疗语义与长时程推理先验
   - **阶段二：Visual CoT Cold-Start SFT** —— 在 ClinVideo-VCoT 与 ClinVideo-CoT 上监督学习工具调用与证据检索
   - **阶段三：GA-GRPO** —— 基于 Grounding-Aware 奖励与自适应优势的强化学习，优化时间对齐的工具使用

3. **奖励设计**
   - 答案正确性奖励 + 格式奖励 + 工具定位奖励（IoU 与时戳对齐）
   - 轨迹级保真度加权优势，鼓励可靠推理、抑制无根据工具调用

---

### 主要贡献

1. **"用视频思考"的医学视觉思维链范式**  
   提出并实现了 MedScope，一个通过三阶段流程（热身、冷启动监督微调、Grounding-Aware GRPO）训练的工具使用型大模型，激发粗到细的临床推理能力。

2. **ClinVideoSuite：以证据为中心的大规模临床视频数据套件**  
   构建了面向训练和评测的大规模证据中心数据套件，核心包括：
   - **ClinVideo-Cap-635K**：63.48万带时间戳的密集字幕
   - **ClinVideo-QA-254K**：25.38万具有局部监督窗口的证据基础问答对
   - **ClinVideo-VCoT-34K / ClinVideo-CoT-84K**：工具增强的视觉思维链轨迹
   - **ClinVideo-Eval**：细粒度长视频医学视频推理基准

3. **GA-GRPO：Grounding-Aware 的强化学习算法**  
   提出了 GA-GRPO（Grounding-Aware Group Relative Policy Optimization），通过 grounding 对齐的复合奖励与自适应优势调制，促进时间对齐的工具使用，并在多样化视频条件下鼓励多样化推理。

---

### 实验结果

MedScope 在域内与域外评测中均达到了**开源模型的最先进性能**：

**SVU-31K（多粒度视频理解）**
- 完整视频描述：**4.76** DO（最优）
- 细粒度时序推理：**11.30** BLEU-4，**4.56** CIDEr
- 细粒度感知推理：**18.90** CIDEr，**10.93** METEOR

**ClinVideo-Eval（定位临床视频推理）**
- OphVL 时序定位：**64.42** mIoU
- MedVideoCap 定位 VQA：**65.58** mIoU
- 在域外 **SurgVidLM** 上保持强劲泛化能力

---

### 发布计划

- [x] 论文（arXiv）
- [ ] **代码与模型** —— *即将发布，敬请期待！*
- [ ] **ClinVideoSuite 数据集** —— *即将发布。*

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
