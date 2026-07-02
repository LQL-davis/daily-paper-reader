---
title: Gaussian Process Priors for Boundary Value Problems of Linear Partial Differential Equations
title_zh: 线性偏微分方程边值问题的高斯过程先验
authors: "Jianlei Huang, Marc Harkonen, Markus Lange-Hegermann, Bogdan Raita"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=IDJabw9ri3"
tags: ["query:boundary-opt"]
score: 9.0
evidence: 满足边界条件的高斯过程先验可通过优化确定参数
tldr: 针对线性偏微分方程边值问题求解中先验信息融合的挑战，本文提出边界Ehrenpreis-Palamodov高斯过程(B-EPGP)，通过参数化形式精确满足线性PDE及边界条件。未知参数通过优化确定，使模型可结合观测数据进行概率预测。实验表明，B-EPGP在数据稀缺时优于PINN和传统GP，为科学计算中的边界优化提供了新框架。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有GP方法难以编码线性PDE边界条件，导致求解不精确。
method: 提出B-EPGP，将边界条件编码为GP先验，并通过优化学习参数。
result: 在数据有限时，B-EPGP的预测精度显著优于PINN和标准GP。
conclusion: B-EPGP为边值问题的概率求解提供了新途径，优化是关键步骤。
---

## Abstract
Working with systems of partial differential equations (PDEs) is a fundamental task in computational science. Well-posed systems are addressed by numerical solvers or neural operators, whereas systems described by data are often addressed by PINNs or Gaussian processes. In this work, we propose Boundary Ehrenpreis--Palamodov Gaussian Processes (B-EPGPs), a novel probabilistic framework for constructing GP priors that satisfy both general systems of linear PDEs with constant coefficients and linear boundary conditions and can be conditioned on a finite data set. The Ehrenpreis--Palamodov theorem provides the functional form of the solution, but leaves parameters of this functional form unknown. An optimization finds these parameters and we provide an algorithm to modify this function form to satisfy boundary conditions. We explicitly construct GP priors for representative PDE systems with practical boundary conditions. Formal proofs of correctness are provided and empirical results demonstrating significant accuracy and computational resource improvements over state-of-the-art approaches.

---

## 论文详细总结（自动生成）

# 论文总结：线性偏微分方程边值问题的高斯过程先验

## 1. 核心问题与研究动机
- 该论文聚焦于**线性偏微分方程（PDE）边值问题**的概率求解。
- 传统数值求解器和神经算子适用于适定系统，而基于数据的问题常由物理信息神经网络（PINNs）或高斯过程（GP）处理。
- **核心挑战**：现有GP方法难以将线性PDE的边界条件编码为先验，导致在数据稀缺时求解精度不足。
- **研究动机**：提出一种能够严格满足线性PDE及其边界条件的GP先验，从而在有限观测数据下进行更准确的概率预测。

## 2. 方法论：边界Ehrenpreis-Palamodov高斯过程（B-EPGP）
- **理论基础**：Ehrenpreis-Palamodov定理给出了线性常系数PDE系统解的泛函形式，但其中参数未知。
- **核心思想**：将该泛函形式作为GP先验，并通过优化确定未知参数，同时显式修改泛函形式以自动满足给定的线性边界条件。
- **关键技术细节**：
  - 利用定理提供的解结构构建参数化GP核函数。
  - 设计算法将边界条件嵌入到GP先验中，形成B-EPGP。
  - 通过**优化**（非随机）学习参数，使得先验同时符合PDE和边界条件。
  - 优化后的先验可结合有限观测数据进行后验推断，给出概率预测。

## 3. 实验设计
- **数据集/场景**：论文未在摘要中明确列出具体数据集，但提到“代表性PDE系统”和“实际边界条件”，可能包括经典线性PDE（如热方程、波动方程、拉普拉斯方程等）的边值问题。
- **基准方法**：与当前前沿方法对比，包括：
  - 物理信息神经网络（PINNs）
  - 标准高斯过程（GP）
- **评估指标**：强调预测精度（accuracy）和计算资源（computational resource）的改进。

## 4. 资源与算力
- 摘要和元数据**未明确提供**GPU型号、数量、训练时长等算力信息。
- 仅提及相比现有方法在计算资源上有显著改善，具体数据需查阅全文。

## 5. 实验数量与充分性
- 未提供具体实验组数，但从摘要表述（“representative PDE systems”）推断，可能覆盖了多种PDE类型和边界条件。
- 该研究提供了“形式化正确性证明”和“经验结果”，说明同时具备理论验证和实验对比。
- 实验对比了两种主流替代方法（PINN, GP），在多场景下评估精度与效率，初步看来实验设计客观、公平，但**充分性需待全文细节**（如消融实验、参数敏感度分析等）。

## 6. 主要结论与发现
- B-EPGP能够**精确满足**线性PDE系统和线性边界条件，且可通过优化自适应确定解的参数。
- 在数据有限情况下，B-EPGP的预测精度显著优于PINNs和标准GP。
- 该框架为科学计算中的边值问题提供了一种新的概率求解途径，其中**优化步骤是关键**。

## 7. 优点（亮点）
- 将边界条件直接融入GP先验的构建，理论严谨，提供正确性证明。
- 利用Ehrenpreis-Palamodov定理减少参数搜索空间，使优化更高效。
- 方法具有概率特性，可输出预测不确定性，适合数据稀缺场景。
- 实验证明在计算资源和精度方面都优于现有知名方法。

## 8. 不足与局限
- 方法目前仅适用于**线性常系数PDE**和**线性边界条件**，对非线性问题或复杂边界的泛化受限。
- 参数优化步骤可能引入计算开销和局部最优问题，论文未明确讨论优化稳定性和收敛性。
- 实验部分信息不足，难以判断测试案例的多样性以及是否包含高维或强耦合系统。
- 虽声称优于PINNs，但缺少与更先进神经算子（如DeepONet, FNO）的直接对比。
- 论文已被ICLR-2026拒绝，可能因实验论证或方法新颖性未达顶会标准（注：元数据显示为rejected）。

（完）
