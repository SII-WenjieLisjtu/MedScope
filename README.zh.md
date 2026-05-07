<div align="center">

# MedScope

**Incentivizing "Think with Videos" for Clinical Reasoning via Coarse-to-Fine Tool Calling**

[![ICML](https://img.shields.io/badge/ICML-2026-blue)](https://icml.cc/Conferences/2026)
[![arXiv](https://img.shields.io/badge/arXiv-2602.13332-b31b1b.svg)](https://arxiv.org/abs/2602.13332)
[![GitHub Stars](https://img.shields.io/github/stars/SII-WenjieLisjtu/MedScope?style=social)](https://github.com/SII-WenjieLisjtu/MedScope)
[![License](https://img.shields.io/badge/License-Apache%202.0-green.svg)](LICENSE)
[![Python](https://img.shields.io/badge/Python-3.10%2B-blue)](https://www.python.org/)
[![PyTorch](https://img.shields.io/badge/PyTorch-2.3%2B-ee4c2c.svg)](https://pytorch.org/)

</div>

<div align="center">

<img src="assets/logo_sii.jpg" height="60" alt="Shanghai Innovation Institute"> &nbsp;&nbsp;&nbsp;&nbsp;
<img src="assets/logo_sjtu_medicine.png" height="60" alt="Shanghai Jiao Tong University"> &nbsp;&nbsp;&nbsp;&nbsp;
<img src="assets/logo_fudan.png" height="60" alt="Fudan University"> &nbsp;&nbsp;&nbsp;&nbsp;
<img src="assets/leapquest.png" height="60" alt="LeapQuest">

</div>

---

<div align="center">
  <a href="https://arxiv.org/abs/2602.13332">[📄 arXiv Paper]</a> &nbsp;|&nbsp;
  <a href="README.md">🇺🇸 English</a> &nbsp;|&nbsp;
  <a href="#citation">[📚 Citation]</a>
</div>

---

## 📰 动态

- 🎉 **2026.05** — 我们的论文被 **ICML 2026** 录用！我们将在第43届国际机器学习大会上展示 MedScope。🎊🥳✨

---

## 🧠 概述

**MedScope** 是一个面向临床长视频推理的工具使用型多模态大语言模型。不同于传统模型对视频进行被动采样或弱监督检查，MedScope 通过**粗到细（coarse-to-fine）**的证据搜索与验证机制，使模型能够像临床医生一样"用视频思考"——在长篇手术及内镜视频中迭代定位、验证并归因时间敏感的关键视觉证据。

本仓库为 ICML 2026 录用论文的官方实现：
> **MedScope: Incentivizing "Think with Videos" for Clinical Reasoning via Coarse-to-Fine Tool Calling**  
> 李文杰\*、张玉洁\*、孙浩然、何新琪、高鸿成、马成龙、胡明、王冠坤、姚诗怡、杨仁豪、任洪亮、王磊、何俊军、蒋炎侃

---

## 🏗️ 核心方法

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

## 🌟 主要贡献

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

## 📊 实验结果

MedScope 在域内与域外评测中均达到了**开源模型的最先进性能**：

**SVU-31K（多粒度视频理解）**
- 完整视频描述：**4.76** DO（最优）
- 细粒度时序推理：**11.30** BLEU-4，**4.56** CIDEr
- 细粒度感知推理：**18.90** CIDEr，**10.93** METEOR

**ClinVideo-Eval（定位临床视频推理）**
- OphVL 时序定位：**64.42** mIoU
- MedVideoCap 定位 VQA：**65.58** mIoU
- 在域外 **SurgVidLM** 上保持强劲泛化能力

<div align="center">
  <img src="assets/figure_results_svu.png" width="85%" alt="Results on SVU-31K and ClinVideo-Eval">
  <p><em>Figure 2. Performance comparison on full and fine-grained video understanding and VQA benchmarks.</em></p>
</div>

<div align="center">
  <img src="assets/figure_results_clinvideo.png" width="90%" alt="Results on ClinVideo-Eval">
  <p><em>Table 1. Performance comparison on ClinVideo-Eval (in-domain and out-of-domain).</em></p>
</div>

---

## 📦 数据集统计

<div align="center">
  <img src="assets/figure_dataset_stats.png" width="90%" alt="ClinVideoSuite Statistics">
  <p><em>Figure 3. Overview of ClinVideoSuite data characteristics across MedVideoCap, SurgVidLM, and OphVL.</em></p>
</div>

---

## 🗂️ 项目结构

```text
MedScope/
├── assets/                          # 图片与视觉素材
├── medscope/                        # 核心模型与训练代码
│   ├── models/                      # 模型架构定义
│   ├── trainers/                    # 三阶段训练器（Warm-up / SFT / GA-GRPO）
│   ├── data/                        # 数据加载与预处理
│   ├── tools/                       # 视觉工具实现（crop_video, get_frame）
│   └── utils/                       # 工具函数与辅助模块
├── scripts/                         # 训练与推理启动脚本
├── configs/                         # 配置文件（模型、训练、数据）
├── checkpoints/                     # 模型检查点保存目录（训练后生成）
├── inference/                       # 推理与演示代码
├── evaluation/                      # 评测脚本与指标实现
├── docs/                            # 补充文档与教程
├── README.md                        # 英文版说明
├── README.zh.md                     # 中文版说明
└── requirements.txt                 # Python 依赖
```

---

## 🚀 快速开始 / 安装

> 🚧 **即将发布！** 代码、模型权重与详细使用说明将在整理完毕后发布，敬请期待！

---

## 🃏 模型卡片

| 属性 | 详情 |
|------|------|
| **基础模型** | Qwen2.5-VL-7B-Instruct |
| **模型类型** | 工具使用型多模态大语言模型（Tool-using LMM） |
| **参数规模** | 7B |
| **训练阶段** | 三阶段流水线 |
| **支持输入** | 临床视频（手术 / 内镜 / 眼科影像）+ 文本问题 |
| **输出形式** | 结构化推理轨迹 + 时序定位证据 + 最终答案 |
| **适用场景** | 临床视频理解、时序定位、视觉问答、手术报告生成 |

### 训练流水线

| 阶段 | 名称 | 数据 | 目标 |
|------|------|------|------|
| Stage 1 | Clinical Reasoning Warm-up | 多源医疗语料 + 长时程视频先验 | 建立医疗语义与长视频推理基础能力 |
| Stage 2 | Visual-CoT Cold-Start SFT | ClinVideo-VCoT-34K / ClinVideo-CoT-84K | 学习显式工具调用与证据检索行为 |
| Stage 3 | GA-GRPO Agentic RL | ClinVideo-Eval 对齐数据 | 优化时间对齐的工具使用与推理多样性 |

### 关键超参数

| 超参数 | 数值 | 说明 |
|--------|------|------|
| 学习率（SFT） | 2e-5 | 余弦退火调度 |
| 学习率（RL） | 1e-6 | 恒定学习率 |
| 批次大小 | 128 | 全局批次 |
| RL 组大小 | 8 | GRPO 每组采样数 |
| 最大视频帧数 | 64 | 视觉编码器输入上限 |
| 最大输出长度 | 2048 tokens | 包含推理轨迹与工具调用 |
| 奖励权重（IoU） | 0.3 | 定位精度奖励系数 |
| 奖励权重（答案） | 0.5 | 答案正确性奖励系数 |
| KL 散度系数 | 0.01 | 策略更新约束 |

---

## 🎬 演示

<div align="center">

  <div style="border: 2px dashed #ccc; border-radius: 12px; padding: 40px; max-width: 640px; margin: 0 auto;">
    <p style="font-size: 48px; margin: 0;">🎥</p>
    <p style="font-size: 18px; color: #666; margin-top: 16px;"><strong>演示视频即将上传</strong></p>
    <p style="font-size: 14px; color: #999;">敬请期待 MedScope 在临床长视频上的实时推理演示</p>
  </div>

</div>

---

## 🛣️ 路线图

| 里程碑 | 内容 | 预计时间 | 状态 |
|--------|------|----------|------|
| 📝 论文发表 | arXiv 预印本 + ICML 2026 会议论文 | 2026.02 | ✅ 已完成 |
| 💻 代码发布 | 训练、推理、评测完整代码 | 2026.06 | 🚧 即将发布 |
| 🤖 模型发布 | MedScope 7B 权重（HuggingFace / ModelScope） | 2026.06 | 🚧 即将发布 |
| 🗃️ 数据集发布 | ClinVideoSuite 训练与评测数据 | 2026.Q3 | 📅 计划中 |
| 🎥 演示视频 | 临床视频实时推理演示 | 2026.06 | 🚧 制作中 |

---

## 📋 发布计划

- [x] 论文（arXiv）
- [ ] **代码与模型** —— *即将发布，敬请期待！*
- [ ] **ClinVideoSuite 数据集** —— *即将发布。*

---

<h2 id="citation">📚 Citation</h2>

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

## 🙏 Acknowledgements

This work is supported by Shanghai Innovation Institute, Shanghai Jiao Tong University School of Medicine, and Shanghai Artificial Intelligence Laboratory. We gratefully acknowledge the clinical data support from Ruijin Hospital.

---

## 📧 Contact

For questions or suggestions, please feel free to open an issue or contact the authors.

---

<div align="center">

  <img src="assets/logo_sii.jpg" height="40" alt="Shanghai Innovation Institute"> &nbsp;&nbsp;&nbsp;&nbsp;
  <img src="assets/logo_sjtu_medicine.png" height="40" alt="Shanghai Jiao Tong University"> &nbsp;&nbsp;&nbsp;&nbsp;
  <img src="assets/logo_fudan.png" height="40" alt="Fudan University"> &nbsp;&nbsp;&nbsp;&nbsp;
  <img src="assets/leapquest.png" height="40" alt="LeapQuest">

  <p><small>Shanghai Innovation Institute &nbsp;|&nbsp; Shanghai Jiao Tong University &nbsp;|&nbsp; Fudan University &nbsp;|&nbsp; LeapQuest</small></p>

</div>
