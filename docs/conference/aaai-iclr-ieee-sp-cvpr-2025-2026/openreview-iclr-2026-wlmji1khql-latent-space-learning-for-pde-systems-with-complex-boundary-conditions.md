---
title: Latent Space Learning for PDE Systems with Complex Boundary Conditions
title_zh: 面向复杂边界条件PDE系统的潜空间学习
authors: "Di Yang, Haoyang Jiang, Yanhai Xiong"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=Wlmji1kHql"
tags: ["query:boundary-opt"]
score: 8.0
evidence: 处理PDE降阶模型中的复杂边界条件
tldr: 科学机器学习中，隐空间降阶模型难以处理时变、非线性等复杂边界条件。本文提出边界感知注意力ROM（BAROM），结合显式边界处理与可学习提升网络，有效捕捉复杂边界影响，在多个偏微分方程模拟中显著提高了预测精度和物理一致性。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有隐空间ROM因表示不匹配而难以准确处理复杂边界条件。
method: 提出BAROM，结合修正ansatz的显式边界处理和注意力机制提升边界保真度。
result: 在多个复杂PDE基准上，BAROM的预测精度明显优于现有方法。
conclusion: 显式边界建模对于提高隐空间ROM的物理可靠性至关重要。
---

## Abstract
Latent space Reduced Order Models (ROMs) in Scientific Machine Learning (SciML) can enhance and accelerate Partial Differential Equation (PDE) simulations. However, they often struggle with complex boundary conditions (BCs) such as time-varying, nonlinear, or state-dependent ones. Current methods for handling BCs in latent space have limitations due to representation mismatch and projection difficulty, impacting predictive accuracy and physical consistency. To address this, we introduce BAROM (Boundary-Aware Attention ROM). BAROM integrates: (1) explicit, RBM-inspired boundary treatment using a modified ansatz and a learnable lifting network for complex BCs; and (2) a non-intrusive, attention-based mechanism, inspired by Galerkin Neural Operators, to learn internal field dynamics within a POD-initialized latent space. Evaluations show BAROM achieves superior accuracy and robustness on benchmark PDEs with diverse complex BCs compared to established SciML approaches.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：科学机器学习（SciML）中的潜空间降阶模型（ROMs）在加速偏微分方程（PDE）仿真时，难以准确处理时变、非线性或状态依赖等复杂边界条件。现有处理边界条件的方法存在表示不匹配和投影困难，导致预测精度下降和物理一致性缺失。
- **整体含义**：论文旨在通过显式建模复杂边界条件，提升潜空间降阶模型对真实物理问题的适用性和可靠性，从而在保证计算效率的同时获得高保真度的预测。

### 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：提出边界感知注意力降阶模型 BAROM（Boundary-Aware Attention ROM），将边界条件处理显式化并与内部场的学习相解耦，以解决表示不匹配问题。
- **关键技术细节**：
  - **显式边界处理**：受简化基方法（RBM）启发，采用修正的 ansatz（假设形式）和可学习的提升网络（learnable lifting network），将复杂边界条件的影响直接注入潜在表示。
  - **内部场动力学学习**：在 POD 初始化的潜空间内，使用非侵入式注意力机制（受 Galerkin Neural Operators 启发）学习内部场的演化规律，避免侵入式投影带来的困难。
- **算法流程**（文字描述）：
  1. 通过 POD 等方法获得全域的潜空间基函数，初始化潜空间。
  2. 使用可学习的提升网络将边界条件信息映射到与潜空间兼容的特征表示，并通过修正的 ansatz 将边界约束显式地强加于解的结构中。
  3. 注意力网络在潜空间中学习从当前状态和边界信息到下一时间步状态的映射，以非侵入方式逼近系统动力学。
  4. 训练时联合优化提升网络和注意力网络，使模型能够同时捕捉边界效应和内部场演化。

### 3. 实验设计：使用了哪些数据集/场景，它的 benchmark 是什么，对比了哪些方法

- **数据集/场景**：在多个具有复杂边界条件的基准 PDE 上进行评估，摘要中未列出具体方程，但提及“benchmark PDEs with diverse complex BCs”，推测可能涵盖热传导、流体流动、结构力学等典型瞬态问题。
- **Benchmark 方法**：与“established SciML approaches”进行对比，具体可能包括：
  - 传统的潜空间 ROM（如 POD-Galerkin 投影法）
  - 其他处理边界条件的潜空间方法（如 Lifting-ROM、DeepONet 等）
  - 纯数据驱动模型（如 Fourier Neural Operator、GNN-based ROM）
- **评估指标**：预测精度（可能为相对 L2 误差）和物理一致性（如守恒律满足程度）。

### 4. 资源与算力

- 在提供的摘要和元数据中 **未提及** 任何关于 GPU 型号、数量、训练时长或推理开销的具体信息。因此无法判断其算力需求和可复现性。

### 5. 实验数量与充分性

- **实验数量**：摘要未给出具体实验组数，仅提到“evaluations show BAROM achieves superior accuracy and robustness on benchmark PDEs with diverse complex BCs”。通常此类工作会包含多个 PDE 场景、不同边界配置以及消融实验（如移除显式边界处理或注意力机制）。但基于现有信息，无法定量评估实验的充分性。
- **公平性与客观性**：对比方法选用了已发表的 SciML 基线，可认为比较基线具有代表性；但无法确认是否进行了超参数调整、多次运行和统计检验。

### 6. 论文的主要结论与发现

- BAROM 在处理复杂边界条件的 PDE 降阶建模任务中，在预测精度和鲁棒性上显著优于现有潜空间 ROM 方法。
- 显式的边界建模和可学习提升网络是提升潜空间 ROM 物理可靠性的关键。
- 非侵入式注意力机制能在保持模型灵活性的同时有效捕获内部场依赖的动力学。

### 7. 优点：方法或实验设计上有哪些亮点

- **显式边界嵌入**：将边界条件处理从内部动力学学习中分离，直接修正解的结构，从根本上缓解表示不匹配问题。
- **非侵入式设计**：利用注意力网络在潜空间学习，避免了传统投影方法的数值稳定性问题和侵入式实现难度。
- **物理一致性提升**：通过强加 ansatz 的显式边界约束，模型预测更符合物理定律。
- **模块化**：提升网络与注意力网络可单独设计和优化，易于扩展到不同边界类型和 PDE 系统。

### 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等

- **实验信息缺失**：目前摘要和元数据中未提供具体 PDE 场景、实验设置、误差表格和消融结果，难以评估实验的覆盖广度和结论的普适性。
- **潜在偏差风险**：仅在少数选定基准上验证，且未见与工业级复杂几何和强非线性问题的对比，可能存在 benchmark 偏差。
- **计算开销未知**：方法是否在大规模问题（如三维、湍流）中仍保持高效尚不明确。
- **理论基础缺失**：摘要未提及收敛性、误差界或稳定性分析，方法停留在数据驱动近似层面。
- **边界条件处理限制**：当边界条件复杂度极高（如自由表面、流固耦合）时，修正的 ansatz 是否足够灵活仍是疑问。

（完）
